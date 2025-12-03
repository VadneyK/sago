# Sago Pitch Deck Intelligence System - Architecture

## Overview
This system implements Use Case 2: automated pitch deck verification and personalized question generation. It demonstrates all three core Sago principles: Seamless Integration, Hyper-Personalization, and True Agency.

---

## System Architecture

### High-Level Flow
```
Pitch Deck Source (Gmail/Slack/Drive) 
    → Document Ingestion 
    → Claim Extraction 
    → Multi-Source Verification 
    → Personalization Engine 
    → Question Generation 
    → Action Execution (Slack/Email)
```

---

## Core Components

### 1. **Ingestion Layer** (Seamless Integration)
**Purpose:** Connect to existing workflows without new interfaces

**Components:**
- **Email Monitor**: Gmail API webhook that detects pitch decks in attachments or links
- **Slack Integration**: Bot listener for shared PDFs in investment channels
- **Drive Watcher**: Google Drive API to monitor shared folders
- **Document Parser**: PDF extraction using `pdf-parse` or `pdfjs-dist`

**Implementation Notes:**
- Use OAuth 2.0 for Gmail/Drive authentication
- Slack Events API for real-time deck sharing detection
- Parse PDF to extract text, images, and structured data (charts, tables)

**Design Consideration:** Users never leave their tools. Sago operates as a background intelligence layer.

---

### 2. **Claim Extraction Engine**
**Purpose:** Identify verifiable claims from pitch decks

**Process:**
1. **LLM-based Extraction** (Claude 3.5 Sonnet via API)
   - Prompt: "Extract all factual claims about revenue, growth rate, customers, market size, team size, partnerships"
   - Structured output: JSON with claim type, value, and context

2. **Entity Recognition**
   - Identify company names, competitor mentions, customer names
   - Extract metrics with units (ARR, MoM growth, user count)

**Example Output:**
```json
{
  "claims": [
    {"type": "revenue", "value": "$2M ARR", "context": "slide 5"},
    {"type": "growth", "value": "40% MoM", "context": "slide 3"},
    {"type": "customers", "value": "3 Fortune 500 partnerships", "context": "slide 7"}
  ]
}
```

---

### 3. **Verification Engine**
**Purpose:** Cross-reference claims with public data sources

**Data Sources:**
- **Web Search**: Brave Search API / Perplexity API for recent news, press releases
- **Company Data**: Crunchbase API, PitchBook API, LinkedIn company pages
- **Traffic Analytics**: SimilarWeb API, Ahrefs API for user metrics validation
- **Market Research**: SEC filings, Gartner reports, industry databases

**Verification Workflow:**
```python
for claim in claims:
    confidence_score = 0
    evidence = []
    
    # Multi-source lookup
    web_results = search_web(claim)
    company_data = query_crunchbase(company_name)
    traffic_data = get_traffic_stats(company_domain)
    
    # LLM-based reasoning
    verification = llm_verify(claim, web_results, company_data, traffic_data)
    
    # Confidence scoring (0-100)
    confidence_score = calculate_confidence(verification)
    
    return {
        "status": "verified" | "partially-verified" | "unverified" | "flagged",
        "confidence": confidence_score,
        "evidence": evidence_sources
    }
```

**Design Consideration:** Use ensemble approach - combine rule-based checks with LLM reasoning for robustness.

---

### 4. **Personalization Engine** (Hyper-Personalization)
**Purpose:** Tailor analysis and questions to investor's thesis and history

**User Profile Building:**
- **Past Investments**: Parse emails, calendar invites, CRM notes in Drive
- **Investment Thesis**: Extract from memos, LP updates, website "About" pages
- **Question Patterns**: Analyze past diligence emails to learn questioning style
- **Portfolio Context**: Identify patterns in successful vs passed deals

**Personalization Techniques:**
1. **Semantic Search**: Vector embeddings (OpenAI `text-embedding-3-large`) of:
   - Current pitch deck
   - Past investment memos
   - Portfolio company decks
   
2. **Similarity Matching**: Find most relevant past deals to surface comparable insights

3. **Thesis Alignment**: Score claims against stated investment criteria
   ```
   If investor focuses on "B2B SaaS with enterprise distribution":
     → Prioritize questions about Fortune 500 partnerships
     → Flag competitive landscape gaps
   ```

**Example Personalization:**
```python
user_profile = {
    "investment_thesis": ["B2B SaaS", "enterprise sales", "strong unit economics"],
    "past_deals": ["WorkOS", "Retool", "Vanta"],
    "common_questions": ["customer concentration", "net dollar retention", "CAC payback"]
}

personalized_questions = generate_questions(
    verification_results,
    user_profile,
    similar_past_deals
)
```

---

### 5. **Question Generation Engine**
**Purpose:** Create insightful, personalized due diligence questions

**Approach:**
- **Context-Aware Prompting**: 
  ```
  Given:
  - Verification results (flagged claims)
  - User's investment thesis
  - Past diligence patterns
  - Competitive intelligence
  
  Generate 5-7 high-priority questions that:
  1. Address verification gaps
  2. Align with investor's past focus areas
  3. Reference specific evidence/data sources
  4. Suggest next steps (calls, data requests)
  ```

- **Question Categorization**: Revenue, Market, Team, Competition, Risks

- **Priority Ranking**: High/Medium based on:
  - Claim confidence score (lower = higher priority)
  - Alignment with user thesis
  - Material impact on valuation

---

### 6. **Action Execution Layer** (True Agency)
**Purpose:** Execute actions, not just provide information

**Automated Actions:**

1. **Slack Integration**
   - Post summary to `#deal-flow` channel
   - Tag relevant partners based on sector expertise
   - Create thread with verification report

2. **Email Draft Generation**
   - Gmail API: Create draft in user's outbox
   - Personalized tone matching user's past emails
   - Attach verification report PDF

3. **Calendar Integration**
   - If high-priority, suggest calendar hold for follow-up call
   - Check availability using Google Calendar API

4. **CRM Update**
   - Log interaction in Affinity/Attio/custom CRM
   - Update deal status to "Diligence - Verification Phase"

**Example Execution:**
```python
async def execute_actions(verification, questions, user):
    # 1. Post to Slack
    await slack_client.post_message(
        channel="#deal-flow",
        text=f"Analyzed {company_name} deck",
        attachments=[verification_summary]
    )
    
    # 2. Draft email
    email_draft = generate_email(questions, user.email_style)
    await gmail_client.create_draft(
        to=founder_email,
        subject=f"Re: {company_name} - Follow-up Questions",
        body=email_draft
    )
    
    # 3. Update CRM
    await crm_client.add_note(
        company_id=company_id,
        note=verification_summary
    )
```

---

## Technical Stack

### Backend
- **Runtime**: Node.js / Python FastAPI
- **LLM Provider**: Anthropic Claude 3.5 Sonnet (for reasoning), OpenAI GPT-4 (for embeddings)
- **Vector Database**: Pinecone / Weaviate (for user profile and historical context)
- **Storage**: PostgreSQL (structured data), S3 (PDFs, reports)
- **Queue**: Redis / BullMQ (for async job processing)

### Integrations
- **Gmail**: Google Workspace APIs
- **Slack**: Slack Web API + Events API
- **Drive**: Google Drive API v3
- **Data Sources**: Crunchbase API, SimilarWeb, Brave Search

### Frontend (Internal Dashboard - Optional)
- **Framework**: React + Next.js
- **UI**: Tailwind CSS
- **Visualization**: Recharts for confidence scores, verification timelines

---

## Design Considerations & Trade-offs

### 1. **Privacy & Security**
- **Challenge**: Accessing sensitive investment data
- **Solution**: 
  - OAuth with minimal scopes (read-only where possible)
  - End-to-end encryption for cached data
  - User controls for data retention policies

### 2. **Verification Accuracy**
- **Challenge**: False positives/negatives in claim verification
- **Solution**:
  - Multi-source validation (never rely on single source)
  - Confidence scores instead of binary true/false
  - Human-in-the-loop: Flag low-confidence claims for manual review
  - Continuous learning from user feedback

### 3. **Real-time vs Batch Processing**
- **Trade-off**: Speed vs thoroughness
- **Decision**: Hybrid approach
  - Fast pass (2-3 min): Basic claim extraction + web search
  - Deep analysis (10-15 min): Full verification with all data sources
  - User can choose based on urgency

### 4. **Personalization Cold Start**
- **Challenge**: New users without historical data
- **Solution**:
  - Onboarding questionnaire for investment thesis
  - Use sector-specific templates until enough data collected
  - Analyze publicly available data (LP website, Twitter/LinkedIn)

### 5. **API Rate Limits & Costs**
- **Challenge**: External API quotas (Crunchbase, SimilarWeb expensive)
- **Solution**:
  - Intelligent caching (TTL based on data freshness needs)
  - Prioritize free/cheaper sources first (web search, LinkedIn)
  - Premium sources only for high-confidence gaps

---

## Future Enhancements

1. **Multi-deck Comparison**: Compare current deck against 50 similar-stage companies
2. **Founder Background Check**: Automated LinkedIn/Twitter analysis of founding team
3. **Competitive Intelligence**: Auto-generate competitive landscape map
4. **Network Intro Paths**: Surface warm intro paths from user's network
5. **Voice Interface**: "Hey Sago, verify this deck" via WhatsApp voice notes

---

## Success Metrics

- **Verification Accuracy**: >85% precision on claim validation
- **Time Saved**: 2-3 hours → 10 minutes for initial diligence
- **User Engagement**: >60% of generated questions used in founder calls
- **Integration Seamlessness**: <2 clicks to go from deck receipt to action execution

---

## Conclusion

This architecture balances sophistication with pragmatism. The system operates invisibly within existing workflows (Gmail, Slack), personalizes insights based on deep user understanding, and executes concrete actions (drafts, posts, updates) rather than just surfacing data. It embodies Sago's vision of being an operating system for investors—always on, deeply integrated, and genuinely helpful.