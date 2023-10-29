# Weekend homework #1
Team 1 exercise in changing strings and transferring ownership using interfaces
## Tasks
• Form groups of 3 to 5 students\
• Interact with “HelloWorld.sol” within your group to change message strings and change owners\
• Write a report with each function execution and the transaction hash, if successful, or the revert reason, if failed
## Description
## Contract information
Contract address: 0xAab7E931c77b13BDBbefB7F9aAd04607a0fE7Ed9<br /><br />
Deployment transaction:<br />
https://sepolia.etherscan.io/tx/0x9039c03dd198a5d2a97ff745a8bd8561c18a0e23571f9c4862a0fdb89a0aed75<br /><br />
Contract code:
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract HelloWorld {

    string private text;

    address public owner;
    
    constructor() {
        text = "Hello World";
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require (msg.sender == owner, "Caller is not the owner");
        _;
    }

    function setText(string calldata newText) public onlyOwner {
        text = newText;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        owner = newOwner;
    }

    function helloWorld() public view returns (string memory) {
        return text;
    }
   
}   
```
## Mike's transactions
### Added natspec to the code, deployed and verified the contract
https://sepolia.etherscan.io/tx/0x9039c03dd198a5d2a97ff745a8bd8561c18a0e23571f9c4862a0fdb89a0aed75
### Interface I used to make calls 
``` solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

interface HelloWorldInterface {

    function helloWorld() external view returns (string memory);

    function setText(string calldata newText) external;
    
    function transferOwnership(address newOwner) external;

}
```
### Setting text to "Hello team 1!"
https://sepolia.etherscan.io/tx/0x7f8183fc95d560cb1035a8fe66697cf0283f80a45ef8f58dd0a03d5e7a169097
### Transferring contract ownership to Loren
https://sepolia.etherscan.io/tx/0x9294bc28d8d9e0f6bc7cfba45857c71bc153f196d1ac447a9ba3fb4bed9723a6

## Loren's transactions
### Setting text to "I'm get owned.. not the owner."
https://sepolia.etherscan.io/tx/0x3478026ed2ac0fa3c66230f40cfd6de1962901160551eb52813a2a1176b6055e
### Transferring contract ownership back to Mike
https://sepolia.etherscan.io/tx/0x4d03c66f16ed2e82988e421992a5d89a7824e439c3aa1a023415b247429931e1

## Mike's transactions
### Calling HelloWorld returned "I'm get owned.. not the owner."
### Transferred ownership to Tomo H
https://sepolia.etherscan.io/tx/0xd63546cca19b52ab553097c66b9fa6d2aa167f6d75a5fe2acee4b23691db386e

## Tomo H transations
### Set text to "rekt"
https://sepolia.etherscan.io/tx/0xb4913540df7479ee802bd0f87121619ce1ab4751bc9ef30419e8357c8c4f7dc0

### Transferring contract ownership to Mike
https://sepolia.etherscan.io/tx/0x55007aea66304f22d5fedb78e66d2f77053cfca8d13b7e9112445a9ec0a01133

### Tried setting text
"We were not able to estimate gas. There might be an error in the contract and this transaction may fail."
When trying to set text when not the owner. The tx would fail.

## Mike's transactions
### Calling HelloWorld returned "rekt"
### Transferring contract ownership to Kent
https://sepolia.etherscan.io/tx/0xa6616add6c7f89f28bee8066aefaf7fec59443163d9cb47b25c340dfe5b5c7f3

## Kent's transactions




