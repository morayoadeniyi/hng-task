## System Architecture - Discoverio
## Overview
Discoverio is an intelligent discovery system designed to integrate seamlessly into existing Nigerian banking applications. The architecture consists of a React Native mobile frontend, a Node.js API layer for real-time processing, a Python-based behavioral analysis engine for machine learning, and a robust data infrastructure using PostgreSQL and Redis. The system processes transaction events in real-time through Apache Kafka, analyzes behavioral patterns using TensorFlow models, and delivers contextual feature recommendations through smart triggers.

## Tech Stack
Frontend: React Native (A cross-platform mobile framework for iOS and Android integration into existing banking apps), TypeScript (A type-safe development for banking-grade reliability), Redux Toolkit (A management tool for user sessions and feature prompts), React Native Async Storage (A local caching for user preferences).

Backend: Node.js + Express ( An API server for real-time trigger processing and feature recommendations), Python + FastAPI (Behavioral analysis microservice for ML model inference), Apache Kafka ( An event streaming platform for real-time transaction monitoring), TensorFlow Serving (A ML model deployment for behavioral pattern recognition).

Database: PostgreSQL (Primary database for feature catalog, trigger rules, user interactions, and analytics), Redis (In-memory cache for real-time user behavior tracking, session management, and feature prompt throttling)

Analytics & Intelligence: TensorFlow (Machine learning models for behavioral pattern recognition eg. salary detection, spending patterns, transfer frequency), Pandas + NumPy (Data processing for behavioral analysis), Scikit-learn (Feature engineering and model evaluation).

DevOps & Infrastructure: Docker (Containerization for all services), GitHub Actions - CI/CD pipeline, Prometheus + Grafana (Monitoring and observability).

## Component Diagram / Flow
┌─────────────────────────────────────────────────────────────────┐
│                         MOBILE APP LAYER                         │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Existing Banking App (GTBank, Access, FirstBank, etc.)    │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │          Discoverio SDK (React Native)               │  │ │
│  │  │  - Prompt UI Components                              │  │ │
│  │  │  - Local behavior tracker                            │  │ │
│  │  │  - Event interceptor                                 │  │ │
│  │  └──────────────────────────────────────────────────────┘  │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTPS/REST API
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         API GATEWAY LAYER                        │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Node.js + Express API Server                             │ │
│  │  - Authentication & authorization                         │ │
│  │  - Rate limiting & request validation                     │ │
│  │  - Feature recommendation endpoint                        │ │
│  │  - Analytics tracking endpoint                            │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
           │                              │
           │                              │
           ▼                              ▼
┌──────────────────────┐      ┌──────────────────────────────────┐
│   REDIS CACHE        │      │  APACHE KAFKA EVENT STREAM       │
│                      │      │                                  │
│ - User sessions      │      │ Topics:                          │
│ - Behavior counters  │      │  - transaction.created           │
│ - Feature throttling │      │  - balance.checked               │
│ - Prompt history     │      │  - transfer.completed            │
└──────────────────────┘      │  - feature.interacted            │
                              └──────────────────────────────────┘
                                             │
                                             │
                                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                    BEHAVIORAL ANALYSIS ENGINE                    │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Python + FastAPI Microservice                            │ │
│  │  ┌──────────────────────────────────────────────────────┐ │ │
│  │  │  Kafka Consumer                                      │ │ │
│  │  │  - Reads transaction events                         │ │ │
│  │  │  - Aggregates behavioral patterns                   │ │ │
│  │  └──────────────────────────────────────────────────────┘ │ │
│  │  ┌──────────────────────────────────────────────────────┐ │ │
│  │  │  TensorFlow ML Models                               │ │ │
│  │  │  - Salary detection model                           │ │ │
│  │  │  - Spending pattern classifier                      │ │ │
│  │  │  - Transfer frequency predictor                     │ │ │
│  │  │  - Feature affinity scorer                          │ │ │
│  │  └──────────────────────────────────────────────────────┘ │ │
│  │  ┌──────────────────────────────────────────────────────┐ │ │
│  │  │  Trigger Engine                                      │ │ │
│  │  │  - Evaluates trigger rules                          │ │ │
│  │  │  - Generates feature recommendations               │ │ │
│  │  │  - Prioritizes prompts by relevance                 │ │ │
│  │  └──────────────────────────────────────────────────────┘ │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      POSTGRESQL DATABASE                         │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Tables:                                                   │ │
│  │  - banks: Bank metadata and configuration                 │ │
│  │  - features: Feature catalog (QuickSave, Spend Analysis)  │ │
│  │  - trigger_rules: Conditions for feature recommendations  │ │
│  │  - users: Anonymized user identifiers                     │ │
│  │  - user_behaviors: Aggregated behavioral patterns         │ │
│  │  - feature_interactions: User engagement with prompts     │ │
│  │  - prompt_history: All delivered prompts and responses    │ │
│  │  - analytics_events: Raw event logs for reporting         │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      ADMIN DASHBOARD                             │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  React Web Application                                     │ │
│  │  - Feature adoption metrics                               │ │
│  │  - Trigger performance analytics                          │ │
│  │  - A/B testing dashboard                                  │ │
│  │  - Feature catalog management                             │ │
│  │  - Trigger rule configuration                             │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
## Request Flow

**User Action** → **Event Capture** → **Intelligence** → **Recommendation** → **User Sees Prompt**

1. Event Capture: User performs action in banking app (e.g., receives salary, makes transfer)
   
2. Event Publishing: Discoverio SDK intercepts event and publishes to Kafka topic
   
3. Stream Processing: Kafka consumer in Behavioral Analysis Engine processes event in real-time
   
4. Behavior Aggregation: Event data is aggregated with historical patterns stored in Redis and PostgreSQL
   
5. ML Inference: TensorFlow models analyze patterns and score feature affinity
   
6. Trigger Evaluation: Trigger Engine evaluates rules and generates recommendations
   
7. Prompt Delivery: API Gateway receives recommendation, checks throttling rules in Redis
   
8. UI Display: SDK receives prompt via REST API and displays contextual UI component
   
9. Interaction Tracking: User response (dismissed, accepted, viewed) is logged to PostgreSQL and Kafka
   
10. Analytics Update: Admin Dashboard reflects real-time adoption metrics

## Data Storage

### PostgreSQL (Persistent Storage)

**How:**
- Feature Catalog: Complete inventory of banking features per institution (QuickSave, Spend Analysis, Standing Orders, etc.)
- Trigger Rules: Configurable conditions that determine when to surface features (e.g., "after 3 transfers to same recipient)
- User Behavior Profiles: Aggregated patterns (salary cycle, average spending, transfer frequency, feature usage history)
- Interaction History: Complete log of every prompt shown, user response, and resulting action
- Analytics Events: Raw event data for reporting and model training
- Bank Configuration: Per-bank feature availability, branding, and integration settings

**Why PostgreSQL?**
- Ensures data integrity for financial analytics
- Rich querying capabilities for complex behavioral pattern analysis
- Proven reliability in banking environments
- Support for JSON columns for flexible feature metadata

### Redis (In-Memory Cache)

**How:**
- User Session Data: Active session tokens and current app state
- Real-Time Counters: Balance check count today, transfers this week, spending this month
- Prompt Throttling: Track when prompts were last shown to prevent notification fatigue
- Feature Cooldowns: Ensure users aren't shown the same feature suggestion repeatedly
- Behavioral Triggers: Temporary flags for immediate trigger evaluation (e.g., "salary_received_today")

**Why Redis?**
- Sub-millisecond response times for real-time trigger evaluation
- Built-in expiration for automatic cleanup of temporary data
- Atomic operations for counters prevent race conditions
- Pub/Sub capabilities for cross-service communication

### Apache Kafka (Event Streaming)

**How:**
- Transaction Events: Real-time stream of deposits, withdrawals, transfers
- App Interaction Events: Balance checks, feature views, navigation patterns
- Feature Engagement Events: Prompt impressions, clicks, dismissals, conversions

**Why Kafka?**
- Handles high-throughput transaction streams from multiple banking apps
- Decouples event producers from consumers for scalability
- Event replay capability enables model retraining and auditing
- Guarantees message delivery in banking-critical environments
  
## Authentication & Security
### Authentication Method
**JWT (JSON Web Tokens) + OAuth 2.0**

### Implementation: #### Bank Integration Authentication
Each banking app receives unique API credentials (`client_id`, `client_secret`) via OAuth 2.0 client credentials flow.

#### User Session Management
JWTs are issued per user session containing anonymized `user_id`, `bank_id`, and session expiration.

#### Token Structure
```json
{
  "sub": "anonymous_user_abc123",
  "bank_id": "gtbank",
  "iat": 1699564800,
  "exp": 1699651200,
  "scopes": ["read:transactions", "write:interactions"]
}
```
  
**Why JWT + OAuth?**
Industry standard for API security in financial services

Stateless authentication reduces database load

OAuth 2.0 provides secure bank-to-service integration

JWTs enable distributed systems without session store bottlenecks
 
## Security Measures

- **Data Anonymization**: No PII stored; users identified by hashed IDs generated by banking apps
- **Encryption in Transit**: All API communication over TLS 1.3
- **Encryption at Rest**: PostgreSQL transparent data encryption (TDE) enabled
- **Rate Limiting**: Redis-backed rate limiting (100 requests/min per user)
- **API Gateway**: Request validation, input sanitization, and SQL injection prevention
- **Access Control**: Role-based access control (RBAC) for admin dashboard
- **Audit Logging**: All data access logged with timestamp, user, and action
- **PCI DSS Compliance**: No card data stored; transaction metadata only (amount, timestamp, category)

## Privacy Design

- **Minimal Data Collection**: Only uses existing transaction logs already collected by banking apps
- **No New Tracking**: No additional user tracking beyond what banks already implement
- **User Consent**: Optional feature that can be disabled in app settings
- **Data Retention**: Interaction logs deleted after 90 days unless needed for analytics

## Hosting / Deployment

### Primary Hosting: **AWS (Amazon Web Services)**

**Services Used:**
- **ECS (Elastic Container Service)** - Orchestrates Docker containers for Node.js API and Python ML service
- **RDS (Relational Database Service)** - Managed PostgreSQL with Multi-AZ deployment for high availability
- **ElastiCache** - Managed Redis cluster for caching and session management
- **MSK (Managed Streaming for Kafka)** - Fully managed Apache Kafka for event streaming
- **S3** - TensorFlow model storage and backup snapshots
- **CloudFront** - CDN for admin dashboard static assets
- **ALB (Application Load Balancer)** - Traffic distribution across API instances
- **Route 53** - DNS management and health checks
- **CloudWatch** - Logging, monitoring, and alerting

## CI/CD Pipeline
GitHub Repository
    ↓
GitHub Actions (on push to main)
    ↓
Run Tests (Jest, PyTest)
    ↓
Build Docker Images
    ↓
Push to ECR (Elastic Container Registry)
    ↓
Deploy to ECS (Rolling Update)
    ↓
Run Smoke Tests
    ↓
CloudWatch Alarms Monitor Health

Why AWS?
Compliance: ISO 27001, SOC 2, PCI DSS certifications required for banking partnerships

Reliability: 99.99% SLA for mission-critical financial services

Scalability: Proven ability to handle Nigerian banking transaction volumes

Ecosystem: Comprehensive managed services reduce operational overhead

Security: Advanced security features (VPC, Security Groups, IAM) built-in

Local Presence: AWS Direct Connect available in Lagos for low-latency integration

## Alternative for MVP/Testing: DigitalOcean
App Platform: For Node.js API (simpler than ECS)

Managed PostgreSQL: For database

Managed Redis: For caching

Spaces: For model storage

Lower cost: ~$200/month vs AWS ~$500/month

Migration Path: Docker containers portable to AWS when scaling

## Reasoning
Why This Architecture is Feasible
1. It leverages Existing Infrastructure: The architecture is designed as an overlay system that integrates with existing banking apps rather than replacing them. Banks can adopt Discoverio by embedding the React Native SDK without rewriting their applications. This reduces implementation time from months to weeks and minimizes risk.

2. Microservices Enable Gradual Rollout: The separation of concerns (API Gateway, Behavioral Engine, Admin Dashboard) allows banks to start with basic rule-based triggers (no ML required initially), add behavioral analysis as transaction data accumulates, scale individual components independently based on load, and test with a small user cohort before full deployment.

3. Event-Driven Architecture Handles Banking Scale: Nigerian banks process millions of daily transactions. Kafka's event streaming architecture decouples transaction processing from feature recommendation, prevents API bottlenecks during peak hours (salary day, month-end), allows asynchronous processing of non-critical operations, and enables horizontal scaling by adding Kafka partitions.

4. ML Models are Pragmatic, Not Experimental: The TensorFlow models solve well-defined problems with abundant training data for salary detection patterns, spending patterns, and transfer frequencies. These aren't cutting-edge AI challenges—they're proven ML applications with high accuracy thresholds achievable in weeks, not months.

5. Proven Tech Stack: Every technology chosen has been battle-tested in financial services, including React Native, which powers major banking apps, Node.js + Express, which runs production systems at PayPal, Netflix, PostgreSQL, which is trusted by financial institutions worldwid,e and Kafka which processes trillions of events at banks like Goldman Sachs.
