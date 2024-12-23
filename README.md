# mBridge Lending Protocol

A decentralized lending protocol that enables users to use mBridge AI tokens as collateral to borrow WBTC. The protocol implements secure lending mechanisms with automated liquidation features to maintain system solvency.

## Overview

The mBridge Lending Protocol allows users to:
- Deposit mBridge AI tokens (0xbdd16F285d8ffD167eD79Bb5cA73456C8F50a21f) as collateral
- Borrow WBTC against their collateral
- Maintain a minimum collateralization ratio of 150%
- Participate in liquidations of undercollateralized positions

## Key Features

- **Collateralized Borrowing**: Users can borrow WBTC by depositing mBridge tokens as collateral
- **Automated Liquidations**: Positions become eligible for liquidation if they fall below 130% collateralization
- **Security First**: Implements OpenZeppelin contracts for maximum security
- **Non-custodial**: Users maintain control of their assets until liquidation conditions are met
- **Gas Efficient**: Optimized for minimal gas consumption
- **Reentrancy Protection**: Secured against reentrancy attacks

## Technical Specifications

### Collateralization Parameters
- Minimum Collateral Ratio: 150%
- Liquidation Threshold: 130%
- Platform Fee: 1%

### Contract Dependencies
- OpenZeppelin Contracts v4.8.0
  - ReentrancyGuard
  - Ownable
  - IERC20

## Installation

```bash
# Clone the repository
git clone [repository-url]

# Install dependencies
npm install

# Install OpenZeppelin contracts
npm install @openzeppelin/contracts
```

## Usage

### Deployment

```solidity
// Deploy with truffle or hardhat
const MBridgeLendingVault = artifacts.require("MBridgeLendingVault");

module.exports = function(deployer) {
  deployer.deploy(
    MBridgeLendingVault,
    "0xbdd16F285d8ffD167eD79Bb5cA73456C8F50a21f", // mBridge token address
    "WBTC_ADDRESS" // Replace with actual WBTC address
  );
};
```

### Interacting with the Contract

```javascript
// Example interactions using web3.js or ethers.js
// Deposit collateral
await mBridgeLendingVault.depositCollateral(amount);

// Borrow WBTC
await mBridgeLendingVault.borrow(amount);

// Repay loan
await mBridgeLendingVault.repay(amount);

// Withdraw collateral
await mBridgeLendingVault.withdrawCollateral(amount);
```

## Functions

### Core Functions

1. `depositCollateral(uint256 amount)`
   - Deposits mBridge tokens as collateral
   - Requires approval for token transfer

2. `withdrawCollateral(uint256 amount)`
   - Withdraws collateral if collateralization ratio permits
   - Checks remaining collateral maintains required ratio

3. `borrow(uint256 amount)`
   - Borrows WBTC against deposited collateral
   - Validates against maximum allowed borrow amount

4. `repay(uint256 amount)`
   - Repays borrowed WBTC
   - Reduces user's debt position

5. `liquidate(address user)`
   - Liquidates undercollateralized positions
   - Transfers collateral to liquidator

### View Functions

1. `getMaxBorrowAmount(uint256 collateralAmount)`
   - Calculates maximum borrowable amount based on collateral

## Security Considerations

1. **Reentrancy Protection**
   - All external functions are protected against reentrancy attacks
   - Follows checks-effects-interactions pattern

2. **Access Control**
   - Owner-only functions for protocol management
   - Public functions properly validated

3. **Input Validation**
   - All function inputs are validated
   - Arithmetic operations protected against overflow

## Events

```solidity
event Deposit(address indexed user, uint256 amount);
event Withdraw(address indexed user, uint256 amount);
event Borrow(address indexed user, uint256 amount);
event Repay(address indexed user, uint256 amount);
event Liquidated(address indexed user, address indexed liquidator, uint256 amount);
```

## Testing

```bash
# Run tests
npm test

# Run coverage
npm run coverage
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Disclaimer

This code is provided as-is. Users should conduct their own security audit before using in production. The developers assume no responsibility for any losses incurred through the use of this protocol.
