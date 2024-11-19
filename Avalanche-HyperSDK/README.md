# Metacrafters - Avalanche HyperSDK

## Introduction To tokevm 


Welcome to [`TokenVM`](./examples/tokenvm), a demonstration application built using the `HyperSDK` framework. This project focuses on two key blockchain use cases: **token minting** and **token trading**. With `TokenVM`, users can:

- Create and mint custom tokens.
- Update token metadata.
- Transfer or burn tokens.
- Trade tokens directly on-chain using an embedded decentralized exchange.

To simplify exploration, `TokenVM` includes a user-friendly CLI tool and supports RPC-based trades via an in-memory order book that tracks on-chain activity in real-time.

If you're curious about how exchanges integrate with blockchain technology, `TokenVM` is a must-read—the core trading logic is implemented in under 100 lines of code!

---

## Project Status

`TokenVM` is currently in **ALPHA** and should not be used in production environments. The framework is actively evolving, with significant updates and optimizations anticipated in the coming months.

---

## Key Features

### Flexible Token Minting
`TokenVM` makes it easy to create, mint, and manage user-generated tokens. Asset creators hold **admin control**, allowing them to:

- Mint additional tokens.
- Update metadata (e.g., during information reveals).
- Transfer or revoke ownership.

Token balances are natively managed, with an optimized storage format that enables efficient and parallelized transfers.

---

### On-Chain Token Trading
`TokenVM` supports fully on-chain trading, empowering users to:

- Create and fill trade offers for token pairs.
- Utilize an in-memory order book to interact with live orders.

#### Optimized for Scalability
Orders and balances are stored efficiently, allowing parallel execution for fills and transfers. The storage structure ensures seamless performance, even with a high volume of orders.

#### Secure Trading Mechanisms
- **Sandwich Resistance:** Transactions explicitly specify which orders to fill, preventing manipulation by frontrunning bots.
- **Partial Fills & Refunds:** Users can partially fill orders and receive refunds for any unused assets.
- **Expiring Fills:** Transactions can include expiration times, reducing stale or unwanted fills.

---

### Avalanche Warp Messaging (AWM) Integration
`TokenVM` leverages Avalanche Warp Messaging for seamless cross-chain token transfers without requiring third-party relayers. Features include:

- **Secure Cross-Chain Transfers:** Messages are validated by 80% of the source chain’s stake weight, ensuring safety.
- **Controlled Imports:** Tokens are restricted to native assets for transparent and secure inter-VM transfers.
- **Cost-Efficient Communication:** AWM scales efficiently, enabling connections to multiple Subnets at minimal cost.
- **Relayer Tips and Asset Swaps:** Users can incentivize relayers and swap tokens upon message imports to cover fees.

---

## Getting Started

### Running `TokenVM`
To launch your own `TokenVM` Subnet, run:
```bash
./scripts/run.sh
```
_For a single Subnet setup, use: `MODE="run-single" ./scripts/run.sh`._

This will allocate funds to:
**Address:** `token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp`  
**Private Key:** `0x323b1d8f...2fa7`

### Building the CLI
To interact with `TokenVM`, build the CLI tool:
```bash
./scripts/build.sh
```
_The CLI binary will be located in `./build/token-cli`._

### Setting Up the CLI
1. Import the demo key:
   ```bash
   ./build/token-cli key import demo.pk
   ```
2. Import chain details:
   ```bash
   ./build/token-cli chain import-anr
   ```

---

## Usage Walkthrough

### Step 1: Create an Asset
Run the following command to create your custom token:
```bash
./build/token-cli action create-asset
```
Example output:
```
metadata: MarioCoin
✅ txID: 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
```
_The `txID` represents your new `assetID`._

---

### Step 2: Mint Tokens
Mint tokens for your created asset:
```bash
./build/token-cli action mint-asset
```
Example output:
```
assetID: 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
amount: 10000
✅ txID: X1E5CVFgFFgniFyWcj5wweGg66TyzjK2bMWWTzFwJcwFYkF72
```

---

### Step 3: Check Balances
Verify your token balance:
```bash
./build/token-cli key balance
```
Example output:
```
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
balance: 10000 MarioCoin
```

---

Thank You
