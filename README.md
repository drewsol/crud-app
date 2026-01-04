# Solana Journal dApp

A full-stack decentralized journal application built on Solana. Users can create, update, and delete journal entries that are stored entirely on-chain using Program Derived Addresses (PDAs).

![Solana](https://img.shields.io/badge/Solana-9945FF?style=flat&logo=solana&logoColor=white)
![Anchor](https://img.shields.io/badge/Anchor-0.31-blue)
![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-4.1-06B6D4?logo=tailwindcss&logoColor=white)

## Features

- **Create** journal entries with a title and message
- **Update** existing entries with automatic account reallocation
- **Delete** entries and recover rent
- **Wallet integration** via Solana Wallet Adapter
- **Cluster switching** between localhost, devnet, and mainnet

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Next.js Frontend                        │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐  │
│  │  Wallet Adapter │  │  React Query    │  │  Anchor     │  │
│  │  (Connect)      │  │  (State Mgmt)   │  │  Client     │  │
│  └────────┬────────┘  └────────┬────────┘  └──────┬──────┘  │
└───────────┼────────────────────┼─────────────────┼──────────┘
            │                    │                 │
            └────────────────────┼─────────────────┘
                                 ▼
┌─────────────────────────────────────────────────────────────┐
│                   Solana Blockchain                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              Anchor Program (Rust)                   │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐    │   │
│  │  │ create      │ │ update      │ │ delete      │    │   │
│  │  │ _entry      │ │ _entry      │ │ _entry      │    │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘    │   │
│  │                                                      │   │
│  │  PDA: seeds = [title, owner_pubkey]                 │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

Each journal entry is stored in a PDA derived from the entry title and owner's public key, ensuring unique entries per user.

## Tech Stack

| Layer      | Technology                          |
|------------|-------------------------------------|
| Frontend   | Next.js 16, React 19, Tailwind CSS  |
| State      | TanStack React Query, Jotai         |
| Blockchain | Solana, Anchor Framework 0.31       |
| Wallet     | Solana Wallet Adapter               |
| Language   | TypeScript, Rust                    |

## Getting Started

### Prerequisites

- Node.js 18+
- Rust and Cargo
- Solana CLI
- Anchor CLI

### Installation

```bash
# Install dependencies
npm install

# Build the Anchor program
npm run anchor-build

# Start local validator with program deployed
npm run anchor-localnet
```

### Development

```bash
# Start the frontend (in a new terminal)
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) and connect your wallet to start creating journal entries.

### Deploy to Devnet

```bash
# Sync program keypair
npm run anchor keys sync

# Deploy
npm run anchor deploy --provider.cluster devnet
```

## Project Structure

```
├── anchor/
│   ├── programs/crud-app/src/
│   │   └── lib.rs          # Solana program (CRUD instructions)
│   ├── target/
│   │   ├── idl/            # Generated IDL
│   │   └── types/          # Generated TypeScript types
│   └── tests/              # Anchor tests
├── src/
│   ├── app/                # Next.js app router pages
│   └── components/
│       ├── counter/        # Journal entry components
│       ├── cluster/        # Network cluster switching
│       ├── solana/         # Wallet provider setup
│       └── ui/             # Reusable UI components
└── package.json
```

## License

MIT
