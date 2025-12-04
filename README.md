# MyToken (MTK) — ERC-20 Cryptocurrency Token

## 📌 Overview
MyToken (MTK) is a simple and fully functional ERC-20 token built on the Ethereum blockchain using Solidity and Remix IDE.  
This project demonstrates the fundamentals of cryptocurrency development and token standards used by thousands of tokens such as USDT, LINK, and UNI.

This token allows:
- Holding & transferring digital assets
- Checking token balances
- Approving third-party addresses to spend tokens
- Executing transfers on behalf of the owner (allowance mechanism)

---

## 🎯 Project Objective
To create and deploy the first cryptocurrency token using the ERC-20 standard while learning the basics of smart contracts, blockchain interactions, and token economics.

---

## 🔧 Technology Used
| Component | Details |
|----------|---------|
| Programming Language | Solidity (v0.8.x) |
| IDE | Remix IDE |
| Blockchain | Ethereum (JavaScript VM for testing) |
| Standard | ERC-20 Token Standard |

---

## 📍 Token Details

| Property | Value |
|---------|------|
| **Name** | MyToken |
| **Symbol** | MTK |
| **Decimals** | 18 |
| **Total Supply** | 1,000,000 MTK (Minted on deployment to owner) |

---

## 🚀 Features Implemented (ERC-20 Core)

| Feature | Status |
|--------|:-----:|
| Token Metadata | ✔️ |
| balanceOf | ✔️ |
| transfer | ✔️ |
| approve | ✔️ |
| allowance | ✔️ |
| transferFrom | ✔️ |
| Transfer Event | ✔️ |
| Approval Event | ✔️ |
| Balance & allowance validation | ✔️ |

---

## 🧠 Smart Contract Code (Solidity)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyToken {
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply;
        balanceOf[msg.sender] = _initialSupply;
        emit Transfer(address(0), msg.sender, _initialSupply);
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Cannot transfer to zero address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        require(_spender != address(0), "Cannot approve zero address");

        allowance[msg.sender][_spender] = _value;

        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Cannot transfer to zero address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Insufficient allowance");

        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, _value);
        return true;
    }
}
