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
### Transferring contract ownership to *

## * transactions


