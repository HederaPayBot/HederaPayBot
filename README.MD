# Hedera Twitter Pay

Send cryptocurrency payments to Twitter users with simple commands. This project integrates Hedera's cryptocurrency capabilities with a Twitter bot to enable seamless social payments.

<div align="center">
  <img src="public/images/logo.svg" alt="Hedera Twitter Pay Logo" width="200" />
  <p><i>Empowering social transactions on the Hedera network</i></p>
  <p>
    <a href="https://hederapaybot.netlify.app/">Live Demo</a> | 
    <a href="https://x.com/HederaPayBot">Twitter Bot (@HederaPayBot)</a>
  </p>
</div>

## Hackathon Submission

<div align="center">
  <a href="https://dorahacks.io/hackathon/hedera-ai-agents">
    <img src="https://dorahacks.io/static/media/logo.9c8a7f5f.svg" alt="DoraHacks Logo" width="150" />
    <h3>Hedera AI Agents Hackathon</h3>
    <p>April-May 2025</p>
  </a>
</div>

This project is a submission for the **[Hedera AI Agents Hackathon](https://dorahacks.io/hackathon/hedera-ai-agents)**. It competes in the following tracks:

- ✅ **Best use of Hedera ElizaOS Plug-in**: Integrates ElizaOS Hedera plugin for processing Twitter commands and handling cryptocurrency transactions
- ✅ **Best use of HCS-10**: Uses Hedera Consensus Service for immutable logging of all payment transactions
- ✅ **Best use of Hedera AgentKit**: Leverages HederaAgentKit for Twitter message processing and Hedera transaction execution

## Features

- 🐦 **Twitter Integration**: Send payments via Twitter commands using agent-twitter-client
- 💸 **Hedera Powered**: Fast, secure transactions with low fees
- 🛡️ **Secure Authentication**: Connect with your Twitter account using Privy
- 📊 **Transaction History**: View all your past payments
- 🔄 **HCS Logging**: All transactions logged to Hedera Consensus Service
- 💼 **Multi-Wallet Support**: Link your existing Hedera account
- 🔌 **Embeddable Widget**: Integrate payment functionality into any Twitter timeline

## Twitter Commands

### Send HBAR
```
@HederaPayBot send 5 HBAR to @recipient
```

### Send Custom Token
```
@HederaPayBot send 10 TTT to @recipient
```

### Check Balance
```
@HederaPayBot show my HBAR balance
```

### Create Token
```
@HederaPayBot create token GameCoin with symbol GC, 2 decimals, and starting supply of 1000000
```

For a complete list of commands, see [BOT_COMMANDS.md](backend/api-endpoint/docs/BOT_COMMANDS.md)

## System Architecture

Our system follows a modern, layered architecture designed for scalability, security, and seamless user experience:

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

### System Components

1. **Frontend Layer**:
   - **Auth**: Privy-powered Twitter authentication with secure session management
   - **Dashboard**: Real-time transaction monitoring and account management
   - **Embed Widget**: Embeddable Twitter payment widget for seamless integration
   - **Transaction History**: Comprehensive view of all past transactions

2. **Backend Services**:
   - **Twitter Service**: Handles all Twitter interactions using agent-twitter-client
   - **Hedera Service**: Manages HBAR transfers, token operations, and HCS messaging
   - **Eliza Service**: Processes natural language commands with ElizaOS integration
   - **User Service**: Handles user profiles, preferences, and account linking

3. **External Systems**:
   - **Twitter API**: Communication via agent-twitter-client for monitoring and replies
   - **Hedera Network**: Secure blockchain infrastructure for all transactions
   - **ElizaOS Plugin**: Custom Hedera plugin for command interpretation
   - **Database**: Persistent storage for user data and transaction history

### Twitter Integration

The system uses agent-twitter-client to:
- Monitor Twitter for payment commands
- Process commands through ElizaOS
- Send transaction confirmations as replies
- Handle authentication and user management

Required environment variables:
- TWITTER_USERNAME, TWITTER_PASSWORD, TWITTER_EMAIL (Required)
- TWITTER_API_KEY, TWITTER_API_KEY_SECRET, TWITTER_ACCESS_TOKEN, TWITTER_ACCESS_TOKEN_SECRET (Optional)

## Technical Implementation

### ElizaOS Hedera Plugin

We've implemented a custom ElizaOS plugin that handles:
- HBAR and token transfers between Twitter users
- Account creation and management
- Transaction history retrieval
- HCS topic management for transaction logging

### Hedera Consensus Service (HCS)

All transactions are logged to HCS topics, providing:
- Immutable record of payment history
- Decentralized audit trail
- Transparent transaction verification

### HederaAgentKit Integration

The application leverages HederaAgentKit for:
- HBAR transfers
- Token management
- Topic creation and message submission
- Transaction processing

## Getting Started

### Prerequisites

- Node.js v16 or higher
- npm or yarn
- A Hedera testnet account with HBAR
- A Privy developer account (for Twitter authentication)
- Twitter developer account (for API access)

The application repositories:
- Frontend: https://github.com/HederaPayBot/frontend
- Backend API: https://github.com/HederaPayBot/api-endpoint

## Usage

1. **Connect Your Account**: 
   - Sign in with Twitter
   - Link your Hedera account ID

2. **Send Payments**:
   - Use the web interface to send payments directly
   - Or mention the bot on Twitter with a command like:
     ```
     @HederaPayBot send 5 HBAR to @friend456 for lunch
     ```

3. **View History**:
   - Check your transaction history on the dashboard
   - Each payment is recorded with status and transaction details

4. **Embed the Widget**:
   - Add the payment widget to your website or Twitter profile
   - Enable your followers to send you HBAR with just a few clicks

## Security Considerations

- Private keys are never stored in the frontend
- All payments are processed through the authorized backend
- Twitter identity verified through Privy
- Hedera accounts must be explicitly linked to use the service
- HCS provides immutable transaction records

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Built for the Hedera AI Agents Hackathon (April-May 2025)
- Powered by Hedera Hashgraph
- Uses ElizaOS Hedera plugin
- Implements HederaAgentKit and HCS
