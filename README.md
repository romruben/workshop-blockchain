# workshop-blockchain
Exercises workshop Blockchain

## Ethereum:

  ### Exercise 0: Ethereum Virtual Machine
  - Option 1 (Linux, Windows and Mac): Download and install [Geth & Tools 1.8.1](https://ethereum.github.io/go-ethereum/downloads/)
  - Option 2 (Ubuntu only)
```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```
  - Test installation executing `evm`
  - See https://github.com/ballesterosbr/evm_meetup#demo
  - Follow demos 1 to 3
  
  *Answer the question: how to return the result as a 32-byte word? Hint: https://github.com/CoinCulture/evm-tools/blob/master/analysis/guide.md#execution*
  

  ### Exercise 1a: Configure a local miner
  *Note: Ethereum data are located in `~/Library/Ethereum` on **MacOS**, and in `~/.ethereum` in **Linux*** 
  - Download and install [Ethereum Wallet and Mist](https://github.com/ethereum/mist/releases)
  - git clone https://github.com/beeva-mariorodriguez/lab-workshop-blockchain-2017
  - Copy (ask for) the `genesis.json` of the previously created private Ethereum ledger.
  
  - initialize blockchain using genesis.json:
  
  ```bash
  geth init files/genesis.json
  ```
  - create ethereum account to store mining profits:
  
  ```bash
  geth account new
  ```
  - Copy (ask for) the BOOTNODE_ADDRESS of the previously created private Ethereum ledger.
  
  - run miner:
  
  ```bash
  geth -networkid $(jq .config.chainId < files/genesis.json) \
             -bootnodes $BOOTNODE_ADDRESS \
             -mine -minerthreads=1 \
             -etherbase=0x$(jq -r .address < ~/.ethereum/keystore/UTC*) \
             -rpc
  ```
  
  - attach to console: `geth attach`
  
  ### Exercise 1b: Transactions and gas
  - Create a new account from Mist
  - Transfer Ether from your miner account to your new empty account
  
  *Answer the question: What fee is required for the transaction to be validated?*
  
  ### Exercise 1c: Upload a contract
  - Copy into Solidity Contract Source Code:
```
pragma solidity ^0.4.18;
/* Based on https://ethereum.org/token */

contract MyToken {
    /* This creates an array with all balances */
    mapping (address => uint256) public balanceOf;
    
    /* This creates an event with transfer data */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /* Initializes contract with initial supply tokens to the creator of the contract */
    function MyToken(uint256 initialSupply) public {
        balanceOf[msg.sender] = initialSupply;              // Give the creator all initial tokens
    }

    /* Send coins */
    function transfer(address _to, uint256 _value) public {
        require(balanceOf[msg.sender] >= _value);           // Check if the sender has enough
        require(balanceOf[_to] + _value >= balanceOf[_to]); // Check for overflows
        balanceOf[msg.sender] -= _value;                    // Subtract from the sender
        balanceOf[_to] += _value;                           // Add the same to the recipient
        
        /* Notify anyone listening that this transfer took place */
        Transfer(msg.sender, _to, _value);
    }
}

```
- Deploy contract
- Watch contract
