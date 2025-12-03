# Sago Pitch Deck Intelligence System

**Assignment Submission for Sago**  
**Use Case**: Pitch Deck Verification & Question Generation  
**Design Principles**: Seamless Integration, Hyper-Personalization, True Agency

> **📋 Assignment Deliverables:**
> - ✅ **GitHub repo + README**: Complete codebase with documentation
> - ✅ **System architecture PDF**: See `SAGO_SYSTEM_ARCHITECTURE.md` (convert to PDF for submission)
> - ✅ **Sample inputs and outputs**: Comprehensive examples throughout this document

---

## 🎯 Overview

This prototype demonstrates an AI agent that automatically verifies claims in pitch decks and generates personalized due diligence questions for investors. The system integrates seamlessly with Gmail and Slack, personalizes insights based on the investor's thesis and history, and executes concrete actions like drafting emails and posting to Slack channels.

---

## 🏗️ Architecture

See `system_architecture.pdf` for detailed design documentation.

**Key Components:**
1. **Ingestion Layer**: Monitors Gmail, Slack, Google Drive for incoming pitch decks
2. **Claim Extraction**: LLM-based parsing to identify verifiable claims
3. **Verification Engine**: Multi-source validation using web search, Crunchbase, traffic data
4. **Personalization Engine**: Tailors questions based on investor's past behavior and thesis
5. **Action Execution**: Drafts emails, posts to Slack, updates CRM

---

## 🚀 Quick Start (UI Demo Only)

### Prerequisites
```bash
node >= 18.x
```

### Installation
```bash
# Clone repository
cd sago-demo
# Install dependencies
npm install
```

### Run UI Demo
```bash
npm start
# Opens http://localhost:3000 - Interactive mockup with sample data
```

> **Note**: This runs a React demo that simulates the user experience. No actual AI analysis, API calls, or integrations occur. All data shown is pre-written sample content.

---

## 📊 Sample Inputs & Outputs

> **⚠️ DISCLAIMER**: All examples below are **completely fictional**. TechFlow AI does not exist, `founder@techflow.ai` is not a real email, and `techflow_deck.pdf` is not a real file. This is sample data created to demonstrate how the system would work.

### Sample Input 1: **Fictional** Pitch Deck (TechFlow AI)

**Source**: Simulated email attachment from fictional founder@techflow.ai  
**File**: `techflow_deck.pdf` (does not exist - this is mock data)

**Extracted Claims:**
```json
{
  "company_name": "TechFlow AI",
  "claims": [
    {
      "type": "revenue",
      "claim": "$2M ARR",
      "source": "Slide 5",
      "timestamp": "Q3 2024"
    },
    {
      "type": "growth",
      "claim": "Growing at 40% MoM",
      "source": "Slide 3",
      "timestamp": "Last 3 months"
    },
    {
      "type": "customers",
      "claim": "Partnerships with 3 Fortune 500 companies",
      "source": "Slide 7",
      "timestamp": "Current"
    },
    {
      "type": "users",
      "claim": "10,000+ users",
      "source": "Slide 4",
      "timestamp": "Current"
    }
  ]
}
```

---

### Sample Output 1: **Fictional** Verification Report

> **Note**: This verification report is entirely made up to show what the system's output would look like. No actual verification was performed.

```json
{
  "company_name": "TechFlow AI",
  "verification_date": "2024-12-03T10:30:00Z",
  "overall_confidence": 58,
  "verified_claims": [
    {
      "claim": "Growing at 40% MoM",
      "status": "verified",
      "confidence": 85,
      "evidence": [
        {
          "source": "LinkedIn",
          "data": "Team grew from 5 to 12 employees in 3 months",
          "relevance": "Team growth typically correlates with revenue growth at seed stage"
        },
        {
          "source": "Twitter/X",
          "data": "CEO posted about 3 new hires in September, 2 in October",
          "relevance": "Confirms rapid hiring pace"
        }
      ],
      "reasoning": "Multiple independent signals (team growth, hiring velocity) support strong growth trajectory. Common pattern for seed-stage SaaS companies."
    }
  ],
  "partially_verified_claims": [
    {
      "claim": "$2M ARR",
      "status": "partially-verified",
      "confidence": 60,
      "evidence": [
        {
          "source": "Crunchbase",
          "data": "$500K seed round raised 6 months ago",
          "relevance": "Typical 18-24 month runway suggests $20-30K MRR burn, implying lower revenue"
        },
        {
          "source": "Web Search",
          "data": "Pricing page shows $99-$499/month tiers",
          "relevance": "Would need 200-500 customers for $2M ARR claim"
        }
      ],
      "reasoning": "Burn rate analysis and pricing model suggest $1-1.5M ARR is more realistic. Discrepancy warrants clarification.",
      "flag": "⚠️ Requires founder clarification"
    }
  ],
  "unverified_claims": [
    {
      "claim": "Partnerships with 3 Fortune 500 companies",
      "status": "unverified",
      "confidence": 30,
      "evidence": [
        {
          "source": "Web Search",
          "data": "No press releases found announcing Fortune 500 partnerships",
          "relevance": "Major partnerships typically announced publicly"
        },
        {
          "source": "Company Blog",
          "data": "Blog mentions pilot programs with 2 mid-market companies (500-1000 employees)",
          "relevance": "Suggests partnerships may be pilots, not enterprise contracts"
        }
      ],
      "reasoning": "No public evidence of Fortune 500 relationships. May be pilots, POCs, or overstated.",
      "flag": "🚩 High priority verification needed"
    }
  ],
  "flagged_claims": [
    {
      "claim": "10,000+ users",
      "status": "flagged",
      "confidence": 40,
      "evidence": [
        {
          "source": "SimilarWeb",
          "data": "~2,000 monthly active users on web platform",
          "relevance": "5x discrepancy with claimed user base"
        },
        {
          "source": "Chrome Web Store",
          "data": "Browser extension shows 800 installs",
          "relevance": "Low adoption for claimed user count"
        }
      ],
      "reasoning": "Significant gap between claimed users and traffic data. Likely discrepancy between total signups vs. active users. Definition clarification needed.",
      "flag": "🚩 Major discrepancy - investigate"
    }
  ],
  "competitive_intelligence": {
    "competitors_identified": [
      "Notion AI",
      "ClickUp AI",
      "Monday.com AI",
      "Asana Intelligence",
      "Linear AI",
      "Coda AI",
      "Airtable AI"
    ],
    "competitors_mentioned_in_deck": [],
    "gap_analysis": "Deck omits all major competitors in workflow automation space. Raises questions about market awareness."
  },
  "market_validation": {
    "tam_claim": "$50B workflow automation market by 2027",
    "tam_verification": "Aligns with Gartner research showing $47B TAM for enterprise workflow software",
    "confidence": 90
  }
}
```

---

### Sample Output 2: **Fictional** Personalized Questions

**Context**: Simulated investor profile showing focus on B2B SaaS, enterprise distribution, unit economics

> **Note**: These questions are pre-written examples, not generated by AI. The "personalization" references are fictional.

```json
{
  "questions": [
    {
      "id": 1,
      "category": "Revenue Verification",
      "priority": "high",
      "question": "Can you walk me through your revenue breakdown? I'm seeing public data that suggests you might be closer to $1-1.5M ARR rather than $2M. What's driving the difference?",
      "rationale": "Large discrepancy between claimed and estimated ARR based on burn rate analysis and pricing tiers. This is a critical metric for valuation.",
      "evidence_references": ["Crunchbase seed round", "Pricing page analysis"],
      "personalization_note": "Based on your past investments, you typically prioritize revenue validation early. You asked similar questions during the WorkOS diligence."
    },
    {
      "id": 2,
      "category": "Customer Claims",
      "priority": "high",
      "question": "You mentioned partnerships with 3 Fortune 500 companies. Can you share which companies these are and whether these are pilots, POCs, or revenue-generating contracts?",
      "rationale": "No public evidence of Fortune 500 partnerships. Understanding true partnership status is critical for enterprise validation thesis.",
      "evidence_references": ["Press release search", "Company blog analysis"],
      "personalization_note": "Enterprise distribution is a key criterion in your investment thesis. Your portfolio shows preference for companies with validated enterprise sales motion."
    },
    {
      "id": 3,
      "category": "User Metrics",
      "priority": "high",
      "question": "Your deck shows 10,000+ users. What's your definition here - total signups, active users, or paying users? Traffic data suggests lower engagement than this number would imply.",
      "rationale": "Significant gap between claimed users (10,000+) and web traffic data (~2,000 MAU). Need clarity on retention and activation metrics.",
      "evidence_references": ["SimilarWeb data", "Chrome Store installs"],
      "personalization_note": "You've previously passed on deals with unclear user definitions (e.g., XYZ Corp in 2023). Clarity here will help avoid similar issues."
    },
    {
      "id": 4,
      "category": "Competitive Landscape",
      "priority": "medium",
      "question": "I noticed you didn't mention Notion AI, ClickUp AI, or Monday.com's recent AI features in your competitive analysis. How do you think about competing with these established players who already have large user bases and distribution?",
      "rationale": "Deck omits all major competitors in the workflow automation space. Understanding competitive moats is crucial given your focus on defensibility.",
      "evidence_references": ["Competitor product analysis", "Recent product launches"],
      "personalization_note": "Based on your past investments in collaboration tools (Slack, Figma), understanding competitive positioning is a key evaluation criterion for you."
    },
    {
      "id": 5,
      "category": "Go-to-Market",
      "priority": "medium",
      "question": "Given your Series A stage and the competitive landscape, what's your wedge for customer acquisition beyond the pilot programs you've mentioned? What does your CAC payback period look like?",
      "rationale": "Unclear differentiation in crowded market raises questions about sustainable GTM motion and unit economics.",
      "evidence_references": ["Competitive landscape analysis", "Pricing model"],
      "personalization_note": "Your portfolio shows preference for companies with CAC payback under 18 months. You asked similar GTM questions during the Retool and Vanta diligence processes."
    },
    {
      "id": 6,
      "category": "Team & Execution",
      "priority": "low",
      "question": "I see you've scaled the team significantly in the last 3 months. What key roles are you still hiring for, and how are you thinking about maintaining culture and execution velocity during this growth phase?",
      "rationale": "Rapid team growth (5 to 12 in 3 months) is positive signal for traction but raises questions about org scaling.",
      "evidence_references": ["LinkedIn hiring data"],
      "personalization_note": "Lower priority given your primary focus on product-market fit and unit economics at this stage."
    }
  ],
  "suggested_follow_up_actions": [
    {
      "action": "Request data room access",
      "items": ["Revenue dashboards", "Customer list", "Unit economics model"]
    },
    {
      "action": "Reference checks",
      "contacts": ["Fortune 500 partnership contacts", "Pilot program users"]
    },
    {
      "action": "Competitive deep dive",
      "task": "Have analyst compare feature set vs. Notion AI, ClickUp AI"
    }
  ]
}
```

---

### Sample Output 3: **Fictional** Automated Actions

> **Note**: These are mock examples of what the system would generate. No actual Slack posts, emails, or CRM updates occur.

**Slack Post** (`#deal-flow` channel) - *Simulated*:
```
🔔 New Pitch Deck Analyzed: TechFlow AI

📊 Verification Summary:
✅ Growth rate (40% MoM) - Verified (85% confidence)
⚠️ Revenue ($2M ARR) - Partially verified (60% confidence) 
🚩 Fortune 500 partnerships - Unverified (30% confidence)
🚩 User count (10,000+) - Flagged for discrepancy (40% confidence)

🎯 Key Insights:
• 7 major competitors not mentioned in deck
• Revenue likely $1-1.5M based on public data
• Enterprise partnerships need validation

📋 6 personalized questions generated based on @john's investment thesis

🔗 Full Report: [View in Drive]
✉️ Draft email created in Gmail

React with 👍 to schedule partner meeting or 👎 to pass
```

**Gmail Draft** (Auto-created in investor's outbox) - *Simulated*:
```
To: founder@techflow.ai (fictional email)
Subject: Re: TechFlow AI - Follow-up Questions
Status: DRAFT (not actually created)

Hi [Founder Name],

Thank you for sharing the TechFlow deck earlier today. I'm intrigued by your approach to AI-powered workflow automation and the early traction you've demonstrated.

Before we schedule a deeper conversation, I'd love to clarify a few points that came up during my initial review:

1. **Revenue Breakdown**: Can you walk me through your current revenue composition? Based on your pricing model and publicly available data, I want to make sure I understand the path to your stated $2M ARR.

2. **Enterprise Partnerships**: You mentioned partnerships with 3 Fortune 500 companies. Could you share which companies these are and clarify whether these are pilots, POCs, or revenue-generating contracts? Understanding your enterprise motion is important for our evaluation.

3. **User Metrics**: Your deck mentions 10,000+ users. Could you clarify if this represents total signups, monthly active users, or paying customers? I'd also love to see retention metrics if you have them.

4. **Competitive Positioning**: I noticed Notion AI, ClickUp AI, and Monday.com recently launched similar features. How do you think about competing with these established players, and what's your sustainable differentiation?

5. **Customer Acquisition**: What's been your primary customer acquisition channel, and what does your CAC payback period look like at the moment?

These questions come from both the deck review and my general approach to evaluating B2B SaaS companies in this space. I think there's real potential here, and I'd love to understand these areas in more depth.

Would you be open to a 30-minute call later this week to discuss? My calendar link is [link] if you'd like to find a time that works.

Looking forward to learning more about TechFlow.

Best,
[Your Name]

---
📎 Verification Report Attached
```

**CRM Update** (Affinity/Attio) - *Simulated*:
```
Company: TechFlow AI (fictional company)
Status: Diligence → Verification Phase
Last Activity: Pitch deck analyzed via Sago
Next Step: Await founder response to verification questions
Priority: Medium (revenue validation needed before proceeding)
Notes: 
- Strong growth signals (40% MoM verified)
- Revenue and partnership claims need clarification
- Competitive landscape crowded but potential for differentiation
- Aligns with enterprise SaaS thesis
```

---

## 🧪 Future Testing

```bash
# Run unit tests
npm test

# Test claim extraction
npm run test:extraction

# Test verification engine
npm run test:verification

# Test personalization
npm run test:personalization
```

---

## 🔑 Key Features Demonstrated

### ✅ Seamless Integration
- **No New Apps**: Operates within Gmail, Slack, Drive
- **Passive Monitoring**: Automatically detects new pitch decks
- **Native Actions**: Drafts emails, posts to Slack in user's voice

### ✅ Hyper-Personalization
- **Thesis Alignment**: Questions match investor's stated criteria
- **Historical Context**: References past deals and patterns
- **Tone Matching**: Email drafts mirror user's communication style
- **Priority Ranking**: Surfaces questions most relevant to user's focus areas

### ✅ True Agency
- **Verification, Not Just Search**: Multi-source validation with confidence scores
- **Actionable Output**: Pre-written emails, Slack posts, CRM updates
- **Decision Support**: Clear recommendations (schedule call, request data, pass)
- **Proactive Intelligence**: Surfaces competitors/risks not mentioned in deck

---

## 📂 Future Project Structure

```
sago-pitch-deck-analyzer/
├── src/
│   ├── ingestion/          # Gmail, Slack, Drive integrations
│   ├── extraction/          # LLM-based claim extraction
│   ├── verification/        # Multi-source validation engine
│   ├── personalization/     # User profile and question generation
│   ├── actions/             # Email, Slack, CRM execution
│   └── utils/               # Helpers, API clients
├── tests/                   # Unit and integration tests
├── examples/                # Sample inputs and outputs
├── docs/                    # Architecture diagrams
├── .env.example             # Environment variables template
├── package.json
└── README.md
```

---

## 🚧 Current Implementation Status

> **Important**: This is a **UI prototype/mockup** that demonstrates the proposed user experience. The backend intelligence system described in this README is **not implemented**. Additionally, **ALL SAMPLE DATA IS COMPLETELY FICTIONAL** - TechFlow AI doesn't exist, founder@techflow.ai is not real, and techflow_deck.pdf was never created.

**✅ Actually Implemented**
- React demo UI (`/sago-demo`) showing complete user flow
- Sample data structures matching proposed JSON schemas
- Interactive mockup of all described features with **fictional data**
- Professional UI/UX design with Tailwind CSS
- Mobile-responsive interface

**❌ Not Implemented (Requires Development)**
- **No backend services** - no API endpoints, databases, or server logic
- **No LLM integration** - no Claude/GPT API connections
- **No external APIs** - no Crunchbase, SimilarWeb, Gmail, or Slack integrations
- **No real verification** - all results are pre-written **fictional sample data**
- **No personalization engine** - questions are hardcoded for **fictional demo company**
- **No file processing** - cannot actually upload or analyze PDFs
- **No authentication** - no OAuth flows or user management

**What the Demo Shows:** Complete user experience as if all features worked on **fictional TechFlow AI data**  
**What Actually Works:** Interactive UI walkthrough with **100% fictional mock data only**

---

## 🎯 Next Steps for Production

1. **Security & Privacy**
   - Implement OAuth 2.0 for all integrations
   - Add encryption for cached user data
   - Build user consent and data retention controls

2. **Accuracy Improvements**
   - Expand verification data sources (PitchBook, PrivCo, etc.)
   - Add confidence calibration based on user feedback
   - Implement ensemble verification (multiple LLM cross-check)

3. **Scale & Performance**
   - Add job queuing for async processing
   - Implement caching layer for expensive API calls
   - Optimize LLM prompts for speed and cost

4. **User Experience**
   - Add "Sago Score" (overall deck quality metric)
   - Build notification preferences (Slack vs email)
   - Create weekly digest of analyzed decks

---

## 🤔 Design Decisions & Trade-offs

### Why Claude 3.5 Sonnet?
- Best-in-class reasoning for claim verification
- Strong at structured output generation
- Handles nuanced business context well

### Why Multi-Source Verification?
- Single sources can be outdated or biased
- Confidence scoring requires triangulation
- Builds trust through transparency

### Why Email Drafts (not auto-send)?
- Maintains human oversight for sensitive communication
- Allows personalization/editing before sending
- Reduces risk of AI errors in investor-founder relationships

### Why Async Processing?
- Verification can take 10-15 minutes with deep analysis
- User doesn't need to wait - Sago works in background
- Enables batch processing during off-hours

---

## 🙏 Acknowledgments

This prototype demonstrates the core concepts of the Sago platform. The React demo provides a visual walkthrough of the user experience, while the architecture document details the production-ready implementation plan.

Thank you for the opportunity to work on this challenge. I'm excited about Sago's vision of building the operating system for investors and would love to discuss how this could evolve into a production system.

---

**Time Spent**: ~4 hours  
**Tools Used**: Claude 3.5 Sonnet (for design and prototyping), React, Tailwind CSS