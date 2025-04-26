# SmartFolio - Decentralized Portfolio Management Protocol

SmartFolio is a Bitcoin-secured decentralized portfolio management system built on the Stacks blockchain. It allows users to create, manage, and automatically rebalance multi-asset portfolios according to customized target allocations, bringing transparency, security, and automation to portfolio management.

## Overview

SmartFolio empowers users to:

- **Create custom portfolios** containing up to 10 different tokens.
- **Assign target allocations** in basis points (0–10,000, representing 0%–100%).
- **Schedule and execute automated rebalancing** based on time intervals.
- **Maintain ownership control** and verifiable security through Bitcoin finality.
- **Pay transparent protocol fees** for management services.
- **Interact with portfolios** programmatically or via a frontend UI (integrations coming soon).

SmartFolio leverages the security of Bitcoin through Stacks smart contracts while maintaining decentralized, user-first asset management principles.

## Protocol Components

### Key Data Structures

- **Portfolios**  
  Stores portfolio metadata including owner, creation time, last rebalance, and token count.

- **PortfolioAssets**  
  Maps each asset inside a portfolio with target allocation percentage, token address, and current holdings.

- **UserPortfolios**  
  Maps each user to a list of their owned portfolio IDs.

### Error Codes

| Error Code                 | Description                        |
| :-------------------------  | :--------------------------------- |
| `ERR-NOT-AUTHORIZED`         | Unauthorized caller               |
| `ERR-INVALID-PORTFOLIO`      | Portfolio does not exist or inactive |
| `ERR-INSUFFICIENT-BALANCE`   | Insufficient funds for operation  |
| `ERR-INVALID-TOKEN`          | Invalid token provided            |
| `ERR-REBALANCE-FAILED`       | Rebalance operation failed        |
| `ERR-PORTFOLIO-EXISTS`       | Portfolio already exists          |
| `ERR-INVALID-PERCENTAGE`     | Provided percentage is invalid    |
| `ERR-MAX-TOKENS-EXCEEDED`    | Maximum token count exceeded      |
| `ERR-LENGTH-MISMATCH`        | Tokens and percentages count mismatch |
| `ERR-USER-STORAGE-FAILED`    | Unable to store user data         |
| `ERR-INVALID-TOKEN-ID`       | Token ID not valid for portfolio  |

## Key Features

### Portfolio Creation

Users can create a portfolio specifying:

- Up to 10 token addresses
- Target allocation percentages

Each portfolio is owned exclusively by the creator.

```clojure
(create-portfolio (list principal ...) (list uint ...))
```

### Automated Rebalancing

Portfolios can be rebalanced every ~24 hours (based on Stacks block height).

```clojure
(rebalance-portfolio (portfolio-id uint))
```

### Allocation Adjustment

Users can update allocation percentages for individual tokens within a portfolio.

```clojure
(update-portfolio-allocation (portfolio-id uint) (token-id uint) (new-percentage uint))
```

### Ownership Management

The protocol owner can transfer protocol control to another principal.

```clojure
(initialize (new-owner principal))
```

## Core Constants

| Constant                     | Value             | Description                         |
| :--------------------------- | :---------------  | :---------------------------------- |
| `MAX-TOKENS-PER-PORTFOLIO`    | `10`               | Max tokens per portfolio           |
| `BASIS-POINTS`                | `10000`            | 100% expressed in basis points     |
| `protocol-fee`                | `25` (0.25%)       | Default protocol fee in basis points |
| `portfolio-counter`           | Dynamic            | Portfolio ID generator             |

## Functions Breakdown

### Read-Only Functions

- `get-portfolio(portfolio-id)` – Fetch portfolio metadata.
- `get-portfolio-asset(portfolio-id, token-id)` – Fetch details of an asset inside a portfolio.
- `get-user-portfolios(user)` – List all portfolio IDs owned by a user.
- `calculate-rebalance-amounts(portfolio-id)` – Check if portfolio needs rebalancing.

### Private Functions

- Internal validation helpers (e.g., token ID validity, percentage validation).
- Storage helpers (e.g., appending portfolio IDs to user mappings).

### Public Functions

- `create-portfolio`
- `rebalance-portfolio`
- `update-portfolio-allocation`
- `initialize` (ownership transfer)

## Security Features

- **Ownership verification**: Only portfolio owners can update or rebalance their portfolios.
- **Percentage validation**: Total target percentages must be within valid 0–100% range.
- **Token ID validation**: Prevents invalid asset modifications.
- **Rebalance timing checks**: Prevents unnecessary frequent rebalancing.

## Example Workflow

1. **Create Portfolio**

    ```clojure
    (create-portfolio
      (list 'token-address-1' 'token-address-2')
      (list u6000 u4000)) ;; 60%-40%
    ```

2. **Wait 24 hours (or more)**

3. **Rebalance Portfolio**

    ```clojure
    (rebalance-portfolio portfolio-id)
    ```

4. **Update Allocation**

    ```clojure
    (update-portfolio-allocation portfolio-id token-id u5000) ;; Set to 50%
    ```

## Future Improvements

- Fee distribution system
- More advanced rebalance strategies (e.g., threshold-based)
- Frontend dApp
- Notifications for pending rebalance
- Integration with Wrapped Bitcoin (xBTC) and DeFi primitives on Stacks

## Get Involved

We welcome developers and contributors to build on top of SmartFolio!  
