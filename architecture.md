# Hedera Twitter Pay - Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│                             FRONTEND (Next.js)                              │
│                                                                             │
│  ┌──────────────┐   ┌────────────────┐   ┌────────────┐   ┌─────────────┐  │
│  │    Auth      │   │   Dashboard    │   │ Embed      │   │ Transaction │  │
│  │  (Privy)     │   │  Components    │   │ Widget     │   │   History   │  │
│  └──────┬───────┘   └────────┬───────┘   └─────┬──────┘   └──────┬──────┘  │
│         │                    │                  │                 │         │
│         └───────────┬────────┴──────────┬──────┴─────────────────┘         │
│                     │                   │                                   │
└─────────────────────┼───────────────────┼───────────────────────────────────┘
                      │                   │
                      ▼                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│                          BACKEND API (Express)                              │
│                                                                             │
│  ┌──────────────┐   ┌────────────────┐   ┌─────────────┐   ┌─────────────┐ │
│  │  Twitter     │   │  Hedera        │   │  Eliza      │   │  User       │ │
│  │  Service     │   │  Service       │   │  Service    │   │  Service    │ │
│  └──────┬───────┘   └────────┬───────┘   └─────┬───────┘   └──────┬──────┘ │
│         │                    │                  │                  │        │
└─────────┼────────────────────┼──────────────────┼──────────────────┼────────┘
          │                    │                  │                  │
          ▼                    ▼                  ▼                  ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐
│                 │  │                 │  │                 │  │             │
│ Twitter API     │  │ Hedera Network  │  │ ElizaOS with   │  │ Database    │
│ (agent-twitter- │  │ - HBAR Transfer │  │ Hedera Plugin  │  │ (SQLite)    │
│  client)        │  │ - Token Service │  │                │  │             │
│                 │  │ - HCS           │  │                │  │             │
└─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────┘

```

## Data Flow

1. **User Authentication**:
   - User authenticates with Twitter via Privy
   - Frontend stores authenticated user session

2. **Twitter Command Processing**:
   - User mentions the bot on Twitter with a payment command
   - Backend Twitter Service monitors mentions via agent-twitter-client
   - Commands are sent to ElizaOS for natural language processing
   - ElizaOS Hedera plugin interprets payment instructions

3. **Payment Execution**:
   - Hedera Service executes the transaction on Hedera Network
   - Transaction details are logged to Hedera Consensus Service (HCS)
   - User's transaction history is updated in the database

4. **Notification Flow**:
   - Transaction confirmation is sent to the user via Twitter reply
   - Dashboard is updated in real-time if user has the web app open
   
5. **Widget Embedding**:
   - Embeddable widget can be integrated into Twitter or other platforms
   - Widget communicates with Backend API to process commands
   - Provides a streamlined payment experience directly in Twitter

## Security Architecture

- Authentication handled by Privy (OAuth with Twitter)
- Sensitive operations require confirmation
- Private keys never exposed to frontend
- All API endpoints are properly authenticated
- HCS provides immutable audit trail of all transactions 