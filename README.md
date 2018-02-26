# workshop-blockchain
Exercises workshop Blockchain

## Ethereum:

  ### Exercise 0: Ethereum Virtual Machine
  - Install ethereum (Ubuntu)
```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```
  - See https://github.com/ballesterosbr/evm_meetup#demo
  - Follow demos 1 to 3
  
  *Answer the question: how to return the result as a 32-byte word? Hint: https://github.com/CoinCulture/evm-tools/blob/master/analysis/guide.md#execution*
  

  ### Create a ledger
  *Note: deploy infraestructure with Terraform on AWS. Use anwbis and AWS_PROFILE*
  - Install terraform
  - Configure aws-cli
  - Configure anwbis
  - export AWS_PROFILE
  - Generate ssh key and import to AWS account, as described [here](https://gist.github.com/beeva-mariorodriguez/e1bedb4aa28e1ce97d16646950f1e9a6)
  - git clone https://github.com/beeva-mariorodriguez/lab-workshop-blockchain-2017
  - Follow steps 1 to 4
  - Save BOOTNODE_ADDRESS in a text file

  ### Configure a local miner
  *Note: Ethereum data are located in `~/Library/Ethereum` on **MacOS**, and in `~/.ethereum` in **Linux***
  - Download and install [Go/Golang](https://golang.org/dl/)
  - Download [Geth & Tools](https://ethereum.github.io/go-ethereum/downloads/)
  - Download and install [Ethereum Wallet and Mist](https://github.com/ethereum/mist/releases)
  - git clone https://github.com/beeva-mariorodriguez/lab-workshop-blockchain-2017
  - Copy (ask for) the `genesis.json` of the previously created private Ethereum ledger.
  - Copy (ask for) the BOOTNODE_ADDRESS of the previously created private Ethereum ledger.
  - Follow step 5
  
  ### Exercise 1: Transactions and gas
  - Create a new account from Mist
  - Transfer Ether from your miner account to your new empty account
  
  *Answer the question: What fee is required for the transaction to be validated?*
  
  ### Exercise 2: Upload a contract
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
