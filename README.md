# workshop-blockchain
Exercises workshop Blockchain

## Ethereum:

  ### Exercise 0: Ethereum Virtual Machine
  - Install Ethereum Virtual Machine:
    
###### Mac

Install via Homebrew.
```
brew update
brew upgrade
brew tap ethereum/ethereum
brew install ethereum
```
###### Linux

Install via `apt-get`.
```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```
###### Windows
Download lastest binary [here](https://geth.ethereum.org/downloads/), extract it and open a command terminal and type:
```
chdir <path to extracted binary>
open geth.exe
```

  - Test installation executing `evm` 
  - See https://github.com/ballesterosbr/evm_meetup#demo
  - Follow demos 1 to 3
  

  ### Exercise 1a: Configure a local miner
  - Download [Geth](https://geth.ethereum.org/downloads/) _(only Linux and Mac)_
  - Download and install [Ethereum Wallet](https://github.com/ethereum/mist/releases)
  - Download [genesis.json](https://raw.githubusercontent.com/beeva-mariorodriguez/lab-workshop-blockchain-2017/master/files/genesis.json)
  
  - Initialize blockchain using genesis.json:
  
  ```bash
  mkdir chaindata
  geth init genesis.json --datadir chaindata
  # --datadir chaindata to avoid conflicts. E.g. error: database already contains an incompatible genesis block
  ```
  
  - create ethereum account to store mining profits:
  
  ```bash
  geth account new --datadir chaindata
  ```
  - Copy (ask for) the BOOTNODE_ADDRESS of the previously created private Ethereum ledger.
  
  - Run miner: Ask us for *BOOTNODE_ADDRESS*.
  
  ##### Linux and Mac
  On Mac, Ethereum data are located in `~/Library/Ethereum`. So replace `~/.ethereum/keystore` by `~/Library/Ethereum/keystore`
  ```bash
  geth -networkid $(jq .config.chainId < genesis.json) \
             -bootnodes $BOOTNODE_ADDRESS \
             -mine -minerthreads=1 \
             -etherbase=0x$(jq -r .address < chaindata/keystore/UTC*) \
             -rpc -datadir=chaindata
  ```

  ##### Windows
  Check *`chainId`* value on `genesis.json` (`config > chainId`) and get your *`walletId`* from `<ethereum_data_path>/keystore`, into file prefixed with `UTC--*`, in its `address` attribute (starts with `0x`). Then, execute:
  ```
geth.exe -networkid <chainId> -bootnodes <bootnode_address> -mine -minerthreads=1 -etherbase=<walletIid> -rpc -datadir=chaindata
  ```
  
  - Launch ethereumwallet:
  ```
  ethereumwallet --datadir=chaindata --rpc chaindata/geth.ipc
  ```
  
  - (If required) attach to console: `geth attach --datadir=chaindata`
  
  ### Exercise 1b: Transactions and gas
  - Create a new account from Ethereum Wallet
  - Transfer Ether from your miner account to your new empty account
  
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

### Exercise 1d: Execute a contract
- Select contract *My Token* and execute function *Transfer*
- See transaction details of contract execution

