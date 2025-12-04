My First ERC-20 Token (MTK)
This repository contains a complete implementation of a basic ERC-20 token written in Solidity and deployed using Remix IDE.
The purpose of this project is to understand the fundamentals of how Ethereum tokens work under the ERC-20 standard, including balances, transfers, approvals, and allowances.

🪙 What This Project Is
This project implements a standard ERC-20 token named MyToken (MTK) with:

A fixed total supply minted during deployment
18 decimal precision
Support for transfer, approve, and transferFrom
ERC-20 required events (Transfer and Approval)
Basic internal checks such as preventing zero-address transfers and enforcing balance/allowance limits
This token follows the official ERC-20 interface and can be interacted with using any Ethereum-compatible wallet or tool.

⚙️ How It Works
The ERC-20 token standard defines a common set of functions that allow:

Tracking balances
Transferring tokens between accounts
Allowing another address to spend tokens on your behalf
Emitting events that external tools can read (wallets, explorers, dApps)
Internally, the smart contract manages:

A ledger stored using mapping(address => uint256)
An allowance table stored using mapping(address => mapping(address => uint256))
A total supply that is assigned to the deployer
Basic safety checks through require statements
When a function like transfer() or transferFrom() is executed, the contract updates the stored balances and emits events so external systems can react to those changes.

📜 Solidity Contract
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyToken {
    // Token Metadata
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;

    uint256 public totalSupply;

    // Balances for each user
    mapping(address => uint256) public balances;

    // Allowances: owner => (spender => amount)
    mapping(address => mapping(address => uint256)) public allowance;

    // Events (required by ERC-20)
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply;
        balances[msg.sender] = _initialSupply;
    }

    function balanceOf(address _owner) public view returns (uint256) {
        return balances[_owner];
    }

    function transfer(address _to, uint256 _value) public returns (bool) {
        require(_to != address(0), "Invalid address");
        require(balances[msg.sender] >= _value, "Insufficient balance");

        balances[msg.sender] -= _value;
        balances[_to] += _value;

        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool) {
        require(_spender != address(0), "Invalid spender");

        allowance[msg.sender][_spender] = _value;

        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool) {
        require(_to != address(0), "Invalid address");
        require(balances[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Allowance too low");

        balances[_from] -= __value;
        balances[_to] += __value;
        allowance[_from][msg.sender] -= __value;

        emit Transfer(_from, _to, _value);
        return true;
    }
}
🚀 How To Deploy and Test (Remix)
Open Remix IDE: https://remix.ethereum.org

Create a file named MyToken.sol

Paste the contract code

Compile with Solidity 0.8.x

Go to Deploy & Run Transactions

Choose Remix VM (Prague)

Enter the initial supply (for 1,000,000 tokens with 18 decimals):

1000000000000000000000000
Click Deploy

Interact with the functions:

name, symbol, decimals, totalSupply
balanceOf
transfer
approve
transferFrom
🧪 Screenshots Included
Screenshots are placed inside the /screenshots folder:

Compilation success
Contract deployment
Token metadata checks
Transfer transaction + event log
Approval + transferFrom logs
Edge-case failures (zero-address and over-balance transfers)
📘 What I Learned
While building this token, I learned:

How the ERC-20 standard is structured
How balances and allowances are stored using mappings
How events make smart contracts observable from the outside
How Remix VM simulates a blockchain for local testing
Why transfers fail for invalid operations (zero address, insufficient balance, low allowance)
How each function modifies the internal state of a smart contract
This project helped me understand the foundational mechanics behind most Ethereum-based tokens.

📧 Contact
Feel free to explore or fork the project.
