# Avalanche Subnet  - Metacrafters Project

## 📖 Overview

The project uses the Avalanche network to build a decentralized game where players can collect, build, and earn rewards. By leveraging custom subnets and Solidity smart contracts, this project demonstrates how to create an engaging decentralized game with in-game assets and a native currency.

---

## 🚀 Features

1. **Custom EVM Subnet**: Configure your own blockchain on the Avalanche network for optimized performance.
2. **Native Currency**: Define and use your unique token as the in-game currency.
3. **MetaMask Integration**: Seamlessly connect your subnet to MetaMask for accessibility.
4. **Smart Contracts**:
   - **ERC20**: Implement tokens for your in-game currency.
   - **Vault**: Manage deposits and withdrawals for player assets.

---

## 🛠️ Setup Instructions

### 1. **Set Up Your EVM Subnet**
   Follow the guide in the [Avalanche Documentation](https://docs.avax.network/) to create a custom EVM subnet. This subnet will host your decentralized game.

### 2. **Define Your Native Currency**
   Deploy the provided ERC20 contract to create your native currency, which will serve as the in-game token for transactions and rewards.

   **Code Example**:
   ```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "Solidity by Example";
    string public symbol = "SOLBYEX";
    uint8 public decimals = 18;

		event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}

   ```

### 3. **Connect to MetaMask**
   Add your custom subnet to MetaMask by:
   - Retrieving your subnet RPC URL.
   - Adding a custom network in MetaMask with the RPC URL, chain ID, and currency details.
   Refer to our [Guide to Connecting MetaMask](#) for detailed instructions.

### 4. **Deploy Basic Smart Contracts**
   Use Solidity and Remix to deploy the following contracts:
   - **ERC20 Token**: Implement a fungible token to be used as in-game currency.
   - **Vault Contract**: Manage token deposits and withdrawals securely.

---

## 📂 Contract Descriptions

### **ERC20 Contract**

This contract defines your in-game currency. It allows minting, burning, transferring tokens, and setting allowances for accounts.

**Key Functions**:
- `mint(uint amount)`: Mint new tokens.
- `burn(uint amount)`: Burn existing tokens.
- `transfer(address recipient, uint amount)`: Transfer tokens to another account.

---

### **Vault Contract**

The Vault contract manages deposits and withdrawals, allowing users to securely lock tokens for gameplay activities.

**Key Features**:
- Proportional share allocation for deposits.
- Allows withdrawal of shares with calculated token amounts.

**Code Example**:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;

    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        /*
        a = amount
        B = balance of token before deposit
        T = total supply
        s = shares to mint

        (T + s) / T = (a + B) / B 

        s = aT / B
        */
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        /*
        a = amount
        B = balance of token before withdraw
        T = total supply
        s = shares to burn

        (T - s) / T = (B - a) / B 

        a = sB / T
        */
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
```

---

## 🧑‍💻 Tools and Technologies

- **Blockchain**: Avalanche Subnet
- **Smart Contract Language**: Solidity
- **IDE**: Remix
- **Wallet**: MetaMask
- **Programming Language**: JavaScript (for frontend, if applicable)

---

## 🌐 Resources

- [Avalanche Documentation](https://docs.avax.network/)
- [MetaMask Integration Guide](https://metamask.io/)
- [Solidity Documentation](https://soliditylang.org/)

---

## 🏁 Getting Started

1. Clone this repository.
2. Follow the subnet setup guide.
3. Deploy the provided contracts.
4. Connect your subnet to MetaMask.
5. Start building your DeFi Kingdom clone.

---
