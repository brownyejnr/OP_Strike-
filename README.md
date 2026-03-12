[README.md](https://github.com/user-attachments/files/25926592/README.md)
# OP_Strike — Bitcoin DeFi Super-App

> **Week 3 — The Breakthrough · OPNet Vibecoding Challenge**
> 
> A full protocol proving Bitcoin Layer 1 can handle advanced DeFi ecosystems.

---

## 🌟 Overview

**OP_Strike** is an all-in-one Web3 super-app built natively on **Bitcoin L1** via **OPNet** — featuring:

| Module | Contract | Description |
|--------|----------|-------------|
| 🖼 NFT Marketplace | `StrikeMarket` | Browse, list, buy & sell OP_721 artifacts |
| 💰 Lending Protocol | `StrikeLend` | Deposit OP_20 assets, borrow, flash loans |
| 🗳 DAO Hub | `StrikeDAO` | On-chain governance, proposals, treasury |

---

## 📁 Architecture

```
op-strike/
├── contracts/
│   ├── strike-market/          # StrikeMarket — OP_721 NFT Marketplace
│   │   ├── src/
│   │   │   ├── StrikeMarket.ts # Contract logic
│   │   │   └── index.ts        # Entry point
│   │   ├── tests/
│   │   │   └── StrikeMarket.test.js  # 16 unit tests
│   │   ├── asconfig.json
│   │   └── package.json
│   │
│   ├── strike-lend/            # StrikeLend — Lending Protocol
│   │   ├── src/
│   │   │   ├── StrikeLend.ts   # Contract + APY math
│   │   │   └── index.ts
│   │   ├── tests/
│   │   │   └── StrikeLend.test.js    # 21 unit tests
│   │   ├── asconfig.json
│   │   └── package.json
│   │
│   └── strike-dao/             # StrikeDAO — Governance Hub
│       ├── src/
│       │   ├── StrikeDAO.ts    # Contract + voting logic
│       │   └── index.ts
│       ├── tests/
│       │   └── StrikeDAO.test.js     # 23 unit tests
│       ├── asconfig.json
│       └── package.json
│
├── frontend/
│   └── public/
│       └── index.html          # Complete responsive frontend
│
├── vercel.json                 # Deployment config
├── package.json                # Monorepo root
└── README.md
```

---

## 🔗 Smart Contracts

### `StrikeMarket.ts` — NFT Marketplace

**Methods:**
- `listItem(collection, tokenId, price)` → Lists NFT, updates floor price
- `buyItem(collection, tokenId)` → Purchases listed NFT
- `delistItem(collection, tokenId)` → Removes listing
- `getFloorPrice()` → Returns current floor
- `getStats()` → Returns listings, floor, volume, royaltyBps
- `setRoyalty(bps)` → Updates royalty (max 10%, deployer only)

### `StrikeLend.ts` — Lending Protocol

**APY Model:** Utilisation-based kink model
- Supply APY = Borrow APY × Utilisation × (1 − Reserve Factor)
- Borrow APY jumps 5× slope above 80% utilisation
- Interest accrues per Bitcoin block (~10 min)

**Methods:**
- `deposit(amount)` → Add assets, earn yield
- `withdraw(shares)` → Redeem deposits + interest
- `borrow(amount)` → Borrow against collateral
- `repay(amount)` → Repay borrow position
- `flashLoan(amount, callback)` → 0.09% fee flash loan
- `getLoanHealth(borrower)` → Collateral/debt ratio
- `getProtocolStats()` → TVL, utilisation, APYs
- `liquidate(borrower)` → Liquidate underwater positions

### `StrikeDAO.ts` — Governance

**Governance Flow:**
1. Token holder creates proposal (min 100 tokens)
2. Community votes FOR/AGAINST (1 token = 1 vote)
3. Voting runs for 144 blocks (~1 day)
4. If quorum (4%) met and FOR > AGAINST → auto-execute

**Methods:**
- `createDao(name, initialTreasury)` → Deploy DAO
- `createProposal(description, daoId)` → Submit proposal
- `vote(proposalId, support)` → Cast weighted vote
- `executeProposal(proposalId)` → Execute after voting
- `getTreasury()` / `depositTreasury()` / `withdrawTreasury()`
- `setVotingPeriod(blocks)` → 6–4032 blocks range

---

## 🧪 Tests — 60 Total, All Passing

```
StrikeMarket: 16 tests ✅
StrikeLend:   21 tests ✅
StrikeDAO:    23 tests ✅
```

Run tests:
```bash
npm test
```

---

## 🎨 Frontend

- **Palette:** Deep charcoal `#0e0e0e` + Bitcoin Yellow `#eab308`
- **Fonts:** Syne (headings) + Space Grotesk (body)
- **Responsive:** Mobile-first, hamburger menu on mobile
- **Wallet:** OP_WALLET connect button (top-right, sticky nav)
- **Features:** NFT grid with generative art, lending dashboard, DAO proposals, FAQ

---

## 🚀 Deployment

### Deploy to Vercel

```bash
# From project root
npx vercel --prod --yes
```

### Build Contracts (requires OPNet toolchain)

```bash
# Install deps per contract
cd contracts/strike-market && npm install
npm run build

cd contracts/strike-lend && npm install
npm run build

cd contracts/strike-dao && npm install
npm run build
```

---

## 🔐 Security

Contracts follow OPNet security guidelines:
- ✅ SafeMath for all arithmetic (no overflow/underflow)
- ✅ Input validation on all public methods
- ✅ `onlyDeployer` access control for privileged operations
- ✅ No bare `Map<Address, T>` — using `AddressMemoryMap`
- ✅ Proper `BytesWriter` sizing to prevent buffer issues
- ✅ Selector-based dispatch with `super.callMethod()` fallthrough
- ✅ Constructor/onDeployment separation

---

## 📜 License

MIT — Built for the OPNet Vibecoding Challenge Week 3
