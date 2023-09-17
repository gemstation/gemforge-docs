---
order: -1
---

# App storage

We recommend using the [App storage](https://eip2535diamonds.substack.com/p/appstorage-pattern-for-state-variables?s=w) pattern for storing data in a Diamond as this gives the most flexibility and ease-of-use.

The [demo projects](../frameworks.md) illustrate this through the `LibAppStorage.sol` file, which contains something like:

```solidity
struct AppStorage {
  bool diamondInitialzied;
  // ...other storage variables here
}

library LibAppStorage {
    bytes32 internal constant DIAMOND_APP_STORAGE_POSITION = keccak256("diamond.app.storage");

    function diamondStorage() internal pure returns (AppStorage storage ds) {
        bytes32 position = DIAMOND_APP_STORAGE_POSITION;
        assembly {
            ds.slot := position
        }
    }
}
```

Any contract in your codebase can now load the current diamond storage and interact with the storage variables. For example:

```solidity
AppStorage storage s = LibAppStorage.diamondStorage();
return s.diamondInitialzied;
```

## Modifying storage post-deployment

It goes without saying the types of existing variables in your storage structures should never be removed or modified once the Diamond has been deployed, as this will otherwise cause memory corruption.

_But what about adding new variables?_

To add storage variables to a deployed Diamond without causing memory corruption, the [following rule](https://eip2535diamonds.substack.com/p/example-of-a-diamond-upgrade) MUST be adhered to:

_**A struct may only be extended with new variables by appending them to the end of the list of existing variables, NEVER before or in between existing variables.**_

And note that this rule also applies to the `AppStorage` struct itself.

For example, let's say we are storing game results for players playing a game:

```solidity
struct PlayerGameResult {
  boolean won;
}

struct Player {
  address wallet;
  mapping(uint => PlayerGameResult);
}

struct AppStorage {
  uint numGames;
  mapping(address => Player) players;
}
```

Here are the variable additions that are both safe and unsafe to do based on the above rule:

```solidity
struct PlayerGameResult {
  uint score;  // <- BAD, will cause memory conflicts!!!
  boolean won;
  uint playTime; // <- Good, since it's added at the end of the AppStorage struct
}

struct Player {
  address wallet;
  uint currentGame;  // <- BAD, will cause memory conflicts!!!
  mapping(uint => PlayerGameResult);
  uint numGamesWon; // <- Good, since it's added at the end of the AppStorage struct
}

struct AppStorage {
  uint numGames;
  uint numGamesInProgress; // <- BAD, will cause memory conflicts!!!
  mapping(address => Player) players;
  uint numGamesCompleted; // <- Good, since it's added at the end of the AppStorage struct
}
```



