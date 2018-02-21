# workshop-blockchain
Exercises workshop Blockchain

## Ethereum:
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
  *Note: geth and mist browser will be installed. Go interpreter and Node.js 4.5 or higher are required*
```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install nodejs
``` 
  - [install geth](https://github.com/ethereum/go-ethereum/wiki/Installing-Geth#install-on-ubuntu-via-ppas)
  - git clone https://github.com/beeva-mariorodriguez/lab-workshop-blockchain-2017
  - Copy (ask for) the `genesis.json` of the previously created private Ethereum ledger.
  - Follow step 1
  - Copy (ask for) the BOOTNODE_ADDRESS of the previously created private Ethereum ledger.
  - Follow step 5
  
  ### Upload a contract
  - Copy into Solidity Contract Source Code 
```
pragma solidity ^0.4.18;
/* Based on https://ethereum.org/token */

contract MyToken {
    /* This creates an array with all balances */
    mapping (address => uint256) public balanceOf;

    /* Initializes contract with initial supply tokens to the creator of the contract */
    function MyToken(
        uint256 initialSupply
        ) public {
        balanceOf[msg.sender] = initialSupply;              // Give the creator all initial tokens
    }

    /* Send coins */
    function transfer(address _to, uint256 _value) public {
        require(balanceOf[msg.sender] >= _value);           // Check if the sender has enough
        require(balanceOf[_to] + _value >= balanceOf[_to]); // Check for overflows
        balanceOf[msg.sender] -= _value;                    // Subtract from the sender
        balanceOf[_to] += _value;                           // Add the same to the recipient
    }
}

```
- Deploy contract
