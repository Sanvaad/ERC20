# OurToken - ERC-20 Token Implementation

This repository contains a custom implementation of an ERC-20 token, **OurToken (OT)**, built using Solidity and OpenZeppelin's ERC-20 standard library. It includes the token contract, deployment script, and comprehensive unit tests to validate its functionality.

---

## Overview

### Features

1. **ERC-20 Compliant**: Implements the ERC-20 standard with OpenZeppelin's library for robust and secure token functionality.
2. **Initial Supply**: The total initial supply is minted and assigned to the deployer during deployment.
3. **Allowance Mechanism**: Supports token approvals and transfers via `transferFrom`.
4. **Testing**: Comprehensive unit tests cover all essential functionality, including transfers, approvals, and edge cases like exceeding allowances.

### Technology Stack

- **Solidity**: Smart contract programming language.
- **OpenZeppelin**: Industry-standard library for secure smart contracts.
- **Foundry**: Framework for contract development, deployment, and testing.

---

## Prerequisites

Ensure the following are installed:

- [Foundry](https://book.getfoundry.sh/) (Solidity development and testing framework)
- [Node.js](https://nodejs.org/) (Optional, for dependency management)
- Ethereum client (e.g., Hardhat, Ganache, or Infura)

---

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/our-token.git
   cd our-token
   ```

2. Install Foundry (if not installed):

   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   foundryup
   ```

3. Build the smart contracts:

   ```bash
   forge build
   ```

---

## Contract Details

### OurToken Contract

`OurToken` is an ERC-20 token with the following key details:

- **Symbol**: `OT`
- **Name**: `OurToken`
- **Initial Supply**: 1000 tokens (denoted in `ether` for 18 decimal places)
- **Minting**: Disabled for external users; only the deployer can mint during deployment.

#### Contract Code

```solidity
contract OurToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("OurToken", "OT") {
        _mint(msg.sender, initialSupply);
    }
}
```

---

## Deployment Script

The `DeployOurToken` script automates the deployment of the `OurToken` contract, specifying the initial supply.

#### Deployment Instructions

1. Configure your RPC URL (via `.env` or an environment variable).
2. Deploy the contract using the script:

   ```bash
   forge script script/DeployOurToken.s.sol --broadcast --rpc-url <YOUR_RPC_URL>
   ```

---

## Testing

The project includes unit tests written using Foundry's `forge-std` library. These tests ensure proper functionality of `OurToken`, including:

1. **Balance Transfers**: Verifies that tokens can be transferred correctly.
2. **Allowances**: Validates the `approve` and `transferFrom` functions.
3. **Minting Restrictions**: Ensures unauthorized users cannot mint new tokens.
4. **Initial Supply Validation**: Confirms the initial supply matches the specified value.

### Running Tests

Run the following command to execute all tests:

```bash
forge test
```

---

## Key Test Cases

1. **Initial Supply Validation**

   Verifies the initial total supply is correctly assigned to the deployer.

   ```solidity
   function testInitialSupply() public {
       assertEq(ourToken.totalSupply(), deployer.INITIAL_SUPPLY());
   }
   ```

2. **Transfer Functionality**

   Tests that tokens can be transferred between accounts and balances are updated accordingly.

   ```solidity
   function testBobBalance() public {
       assertEq(STARTING_BALANCE, ourToken.balanceOf(bob));
   }
   ```

3. **Allowance Mechanism**

   Confirms that allowances work correctly and reverts if transfers exceed the approved amount.

   ```solidity
   function testAllowancesWorks() public {
       vm.prank(bob);
       ourToken.approve(alice, 1000);

       vm.prank(alice);
       ourToken.transferFrom(bob, alice, 500);

       assertEq(ourToken.balanceOf(alice), 500);
       assertEq(ourToken.balanceOf(bob), STARTING_BALANCE - 500);
   }
   ```

4. **Minting Restrictions**

   Ensures unauthorized minting attempts are reverted.

   ```solidity
   function testUsersCantMint() public {
       vm.expectRevert();
       MintableToken(address(ourToken)).mint(address(this), 1);
   }
   ```

---

## Directory Structure

```
├── src/
│   └── OurToken.sol           # ERC-20 token contract
├── script/
│   └── DeployOurToken.s.sol   # Deployment script for OurToken
├── test/
│   └── OurTokenTest.t.sol     # Unit tests for OurToken
├── forge.config.toml          # Foundry configuration file
└── README.md                  # Project documentation
```

---

## Contribution

Contributions are welcome! If you’d like to suggest improvements or report bugs, please open an issue or submit a pull request.

### How to Contribute

1. Fork the repository.
2. Create a new branch for your feature or bugfix.
3. Write clear commit messages describing your changes.
4. Submit a pull request for review.

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT). Feel free to use, modify, and distribute this code as per the license terms.

---

## Contact

For any questions or feedback, feel free to reach out:

- **GitHub**: [sanvaad](https://github.com/sanvaad)
- **Email**: houseofsanvaad@gmail.com
