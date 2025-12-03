# Sago Pitch Deck Intelligence System
## System Architecture & Design

**Assignment Submission for Sago**  
**Use Case**: Pitch Deck Verification & Question Generation  
**Submitted by**: [Your Name]  
**Date**: December 3, 2024

---

## Executive Summary

This system implements Use Case 2: automated pitch deck verification and personalized question generation for investors. It demonstrates all three Sago design principles through a seamless workflow that operates within existing tools (Gmail, Slack), provides hyper-personalized insights based on investor history, and executes concrete actions rather than just surfacing data.

**Current Implementation**: UI prototype with complete user flow and sample data  
**Production Architecture**: Comprehensive design for full system implementation

---

## System Architecture Overview

```
Pitch Deck Source (Gmail/Slack) 
    ↓
Document Ingestion & Parsing
    ↓
LLM-Based Claim Extraction
    ↓
Multi-Source Verification Engine
    ↓
Personalization Engine (User Profile + History)
    ↓
AI Question Generation
    ↓
Action Execution (Email Draft + Slack Post + CRM Update)
```

---

## Core Components & Design Decisions

### 1. Ingestion Layer (Seamless Integration)
**Design Choice**: Webhook-based monitoring of Gmail and Slack  
**Why**: Users never leave their existing workflow. Sago operates invisibly in the background.

**Implementation**:
- Gmail API webhooks detect PDF attachments from founders
- Slack Events API monitors shared files in investment channels
- Google Drive API watches shared folders for new pitch decks

### 2. Claim Extraction Engine
**Design Choice**: LLM-first approach with structured prompting  
**Why**: Traditional NLP lacks context understanding for business claims.

**Architecture**:
- Claude 3.5 Sonnet for reasoning about business context
- Structured JSON output with confidence scoring
- Focus on verifiable metrics: revenue, growth, customers, partnerships

### 3. Verification Engine
**Design Choice**: Multi-source triangulation with confidence scoring  
**Why**: Single sources can be outdated/biased. Transparency builds trust.

**Data Sources**:
- **Primary**: Crunchbase, LinkedIn, company websites
- **Secondary**: SimilarWeb traffic data, press releases, SEC filings
- **Tertiary**: Social media signals, hiring patterns

**Confidence Algorithm**:
- Multiple source agreement = higher confidence
- Recent data weighted more heavily
- Source reliability scoring based on track record

### 4. Personalization Engine (Hyper-Personalization)
**Design Choice**: Vector embeddings + historical pattern matching  
**Why**: Generic questions waste time. Personalized insights reflect investor's actual decision criteria.

**Approach**:
- Parse past investment memos to extract thesis and preferences
- Vector similarity matching between current deck and portfolio companies
- Question prioritization based on investor's historical focus areas
- Tone matching using past email communication patterns

### 5. Action Execution (True Agency)
**Design Choice**: Pre-draft actions requiring minimal human oversight  
**Why**: Information without action doesn't save time. System should do work, not just suggest it.

**Automated Actions**:
- Gmail draft creation with personalized questions
- Slack channel updates with verification summary
- Calendar holds for high-priority deals
- CRM note generation with key insights

---

## Key Technical Decisions

### LLM Strategy
**Choice**: Claude 3.5 Sonnet as primary reasoning engine  
**Rationale**: Superior reasoning for complex business context vs GPT-4

### Database Architecture
**Choice**: PostgreSQL + Pinecone vector database  
**Rationale**: Structured data (users, deals) + vector search (personalization)

### Real-time Processing
**Choice**: Async job queue (Redis/BullMQ) with 2-tier processing  
**Rationale**: Fast preliminary analysis (2-3 min) + deep verification (10-15 min)

### Privacy & Security
**Choice**: End-to-end encryption with user data controls  
**Rationale**: Investor data is highly sensitive. Trust is paramount.

---

## Demonstration of Sago Principles

### Seamless Integration ✅
- **No New Apps**: Operates within Gmail, Slack, Google Drive
- **Zero Learning Curve**: Uses existing communication patterns
- **Native Feel**: Actions appear as if user performed them manually

### Hyper-Personalization ✅
- **Thesis-Aware**: Questions align with stated investment criteria
- **History-Informed**: References patterns from past deals
- **Context-Rich**: Understands user's portfolio and preferences
- **Adaptive**: Learns from user feedback and choices

### True Agency ✅
- **Action-Oriented**: Drafts emails, posts updates, schedules calls
- **Proactive**: Surfaces competitive intelligence and red flags
- **Decision-Supporting**: Provides clear next steps and recommendations
- **Time-Saving**: Reduces 3-hour diligence prep to 10-minute review

---

## Sample Data Flow

**Input**: Email from founder@techflow.ai with Series A deck  
**Processing**: 
1. Extract 4 key claims (revenue, growth, partnerships, users)
2. Verify against Crunchbase, LinkedIn, SimilarWeb
3. Generate confidence scores (30-85% range)
4. Create 6 personalized questions based on investor's B2B SaaS thesis
5. Draft contextual email referencing past deal patterns

**Output**: 
- Slack post in #deal-flow with verification summary
- Gmail draft with personalized questions
- CRM update with priority scoring
- Calendar hold suggestion for high-confidence deals

---

## Implementation Status

**✅ Completed**: UI prototype demonstrating full user experience  
**🏗️ Next Phase**: Backend API development with LLM integration  
**📅 Timeline**: 4-6 weeks for MVP with basic verification  
**🎯 Goal**: Production system handling 50+ decks/month per investor

---

## Conclusion

This architecture balances sophistication with pragmatism. The system operates invisibly within existing workflows (Gmail, Slack), personalizes insights based on deep user understanding, and executes concrete actions rather than just surfacing data. It embodies Sago's vision of being an operating system for investors—always on, deeply integrated, and genuinely helpful.

The current prototype demonstrates the complete user experience, while this architecture provides the roadmap for production implementation.