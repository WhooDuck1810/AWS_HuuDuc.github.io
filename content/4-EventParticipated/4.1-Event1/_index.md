---
title: "Event 1: AWS Agentic AI Build Week (AABW)"
date: 2026-07-27
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: “AWS Agentic AI Build Week (AABW)”

### Event Overview
- **Event Name:** AWS Agentic AI Build Week (AABW)
- **Date & Time:** July 27, 2026
- **Location:** 26th Floor, Bitexco Financial Tower, 02 Hai Trieu Street, Sai Gon Ward, Ho Chi Minh City
- **Role:** Attendee

---

### 1. Event Objectives

The **AWS Agentic AI Build Week (AABW)** was an intensive 24-hour hackathon designed to challenge participants to build practical, real-world solutions powered by Next-Generation Autonomous AI Agents (Agentic AI). The core goals of the event included:

- **Rapid Product Development:** Constructing a fully functional Minimum Viable Product (MVP) under strict 24-hour time constraints.
- **Hands-On Cloud & AI Experience:** Integrating specialized AWS services alongside custom AI architectures.
- **Real-World System Operations:** Bridging academic theory with enterprise-grade cloud system deployment.
- **High-Velocity Teamwork:** Mastering rapid ideation, task division, parallel execution, and live product demonstrations under pressure.

---

### 2. Analysis of Featured Hackathon Projects

During the showcase, I had the opportunity to observe and analyze three outstanding projects that demonstrated innovative uses of Agentic AI across diverse industries:

#### 2.1. Team 3KA – S.H.E.P.H.E.R.D (Autonomous Crowd Flow Management)
- **Problem Statement:** Traditional crowd management at high-density venues relies on manual monitoring, leading to delayed hazard responses and poor scalability.
- **Proposed Solution:** An intelligent computer-vision AI camera system that tracks human movement, measures crowd density in real time, forecasts bottleneck congestion, and triggers automated emergency alerts.
- **Tech Stack:** YOLO, ByteTrack, Amazon SageMaker, Amazon Bedrock Agent, Core + Strands Agent Framework, and React Dashboard.
- **Agentic Architecture:** Features a dual-layer design:
  1. *Autonomous Monitor Layer:* Continuously streams and processes visual analytics.
  2. *Operator Copilot Layer:* Enables event managers to query real-time status and receive operational recommendations via natural language.

#### 2.2. Team Signal Scout – Enterprise Strategic Intelligence System
- **Problem Statement:** Corporate financial and operational data is fragmented across siloed systems, making early detection of strategic risks difficult.
- **Proposed Solution:** An automated telemetry pipeline that aggregates corporate performance metrics and uses AI Agents to detect early signals of corporate restructuring or market pivots.
- **Architecture Highlights:** Emphasized decentralized microservice deployment, end-to-end observability, and automated CI/CD. The team provided a transparent AWS cost model (ranging from a baseline of ~$81/month to ~$359/month under peak traffic) along with architectural recommendations for cost reduction.

#### 2.3. Team Plan V – SA Professional (Automated Solutions Architecture Assistant)
- **Problem Statement:** Solutions Architects (SAs) spend extensive hours manually parsing requirement documents (BRD/PRD), drafting diagrams, and estimating cloud costs for complex proposals.
- **Proposed Solution:** An interactive AI assistant that ingests business requirements, drafts high-level cloud architectures, generates editable Draw.io / AWS diagrams, and calculates localized pricing for the `ap-southeast-1` region.
- **Impact:** Replaces manual drafting with conversational iterative refinement, automatically generating production-ready Infrastructure as Code (IaC) scripts within minutes.

---

### 3. Key Technical Takeaways

- **Complex System Integration:** Team 3KA demonstrated how to pair real-time edge processing (YOLO) with cloud-based AI inference (SageMaker & Bedrock) while maintaining minimal end-to-end latency.
- **Cloud Cost Optimization (FinOps):** Signal Scout’s detailed breakdown of Bedrock token usage, compute instance scaling, and WAF rules highlighted that effective software engineering requires balancing technical performance with cloud cost efficiency.
- **Workflow Automation:** Plan V illustrated how AI can automate technical documentation and IaC template generation, a capability directly applicable to full-stack cloud projects.

---

### 4. Management & Teamwork Insights

Participating in and observing a 24-hour hackathon provided deep insights into managing technical stress and team dynamics:

- **Navigating Technical Crisis:** Teams encountered unexpected hurdles such as Git merge conflicts, sudden API rate limits, and accidental credential leaks. Achieving a deep state of focus ("flow state") helped teams convert panic into structured troubleshooting.
- **Four Pillars of Execution:** Successful teams relied on:
  1. Clear project boundaries.
  2. Modular boilerplate templates prepared in advance.
  3. Strict role division.
  4. Ample time reserved for rehearsing the live product demo.
- **Execution Mindset:** *"Showing up is half the battle won."* A polished, working core feature set delivers significantly more value than an ambitious but non-functional concept.

---

### 5. Practical Application to My Work

- **Security & Secret Management:** Observing Team 3KA’s accidental credential commit reinforced the critical importance of strict `.gitignore` configurations, environment variables (`.env`), and immediate API key rotation protocols — practices I actively implemented in the JWT authentication layer for the **Tracker Maintenance** project.
- **Architecture-First Development:** Moving forward, I will systematically model system architecture diagrams and perform cloud cost projections (inspired by Plan V) prior to writing code for any AWS deployment.
