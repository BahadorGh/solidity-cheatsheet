# Most up to date Solidity Cheatsheet and Best practices

## Motivation

This document is a cheatsheet for **Solidity** that you can use to write **Smart Contracts** for **Ethereum and all EVM Compatible** based blockchain.

This guide is not intended to teach you Solidity from the ground up, but to help developers with basic knowledge who may struggle to get familiar with Smart Contracts and Blockchain because of the Solidity concepts used.

> **Note:** If you have basic knowledge in JavaScript, it's easier to learn Solidity.

## Table of contents

- [Solidity Cheatsheet and Best practices]
  (#solidity-cheatsheet)
  - [Licensing](#licensing-of-the-smart-contract)
  - [Compiler Version](#compiler-version)
  - [Import]
  - [Contract Definition](#contract-definition)
  - [Data types](#data-types)
  - [Variable types](#variable-types)
  - [Visibility scope](#visibility-scope)
  - [Function types](#function-types)
  - [Constructor](#constructor)

## Licensing of the smart contract

`// SPDX-License-Identifier: MIT`

> **Note:** List of allowed SPDX licenses to use => https://spdx.org/licenses/

## Compiler version

Defining solidity compiler(solc) version for compiling the smart contract => - Some possible ways to use: - compiler version set exactly on 0.8.0 version => `pragma solidity 0.8.0;`

- compiler version set above 0.8.0 version and lower than 0.9.0 version => `pragma solidity ^0.8.0;`
- compiler version set equal or above 0.7.0 version and lower than 0.9.0 version => `pragma solidity >= 0.7.0 < 0.9.0;`

## Contract definition

Creating the smart contract:
`contract ContractName {}`

> **Note:** Contracts are like classes in other programming languages
> **Note:** Naming convention is like this for declaring contract: MyContract

## Data types

- 1. Value types:
     - Value stored in smart contract storage slot
       - `bool` : `true` or `false`
       - int(Signed Integer) : `int8  | int16  | int32 | ... | int64  | int128  | int256(int)`
       - uint(Unsigned Integer) : `uint8 | uint16 | uint32 | ... | uint64 | uint128 | uint256(uint)`
       - `address` and `address payable` : Holds an Ethereum address(20 bytes)
       - `bytes1(byte)`, `bytes2`, `bytes3`, ..., `bytes32`
       - [enum](#enum)

- 2. Reference types:
  - A reference to a stored value in smart contract storage slot will be set
    - array
    - `string`
    - [mapping](#mapping)
    - [struct](#struct)
    - bytes

## Variable types

- 1. State variables:

  - Get permanently stored on blockchain(smart contract storage)
  - Use most gas usage in smart contracts
  - Accessible on whole smart contract

- 2. Local variables:

  - Not stored on blockchain
  - Use less gas in smart contracts
  - Are living and working just in function body

- 3. Global variables:
  - Provide information about the blockchain
  - Can be used both as state variables and local variables
  - Mainly used to determine contract owner and checking time

## Visibility scope

1.  **Variables**:

    - `public`
    - `private`
    - `internal`

    - Ex: `uint public number;`

> **Note:** visibility is just for state variables and not applicable on local variables
> **Note:** default visibility of a state variable is(if we don't declare visibility scope) --> internal
> **Note:** if declare a variable to have `public` scope, automatically a 'getter function' will be created for that variable

- 2. **Functions**:

  - `public`
  - `private`
  - `internal`
  - `external`
  - Ex: `function setNumber() public {}`

## Function types

- 1.  **Non-Payable** => Functions which

  - Write on the blockchain
  - Are our default functions type
  - Are not able to accept deposits on the smart contract

  * Ex: `function setNumber() public {}`

- 2.  **View** => Functions which

  - Are able to show us data
  - Read from blockchain

  * Ex: `function setNumber() public view {}`

- 3.  **Pure** => Functions which

  - Neither read nor write on blockchain
  - Just do a specific work for us (ex: making sum of 2 numbers and returning back the value)

  * Ex: `function setNumber() public pure {}`

- 3.  **Payable** => Functions which

  - Are able to accept Ether deposits on the smart contract

  * Ex: `function setNumber() public payable {}`

## Constructor

- Is optional
- Does not have a name
- Does not have visibility scope
- Executed during contract deployment
- Can take parameters while deploying
- Initializes smart contract state variables
- Will be at most 1 within each smart contract
- Can have payable attribute associatede with it

* Ex:

```
constructor(uint _number) {
     number = _number;
    }
```

## Data locations

- Each variable declared and used in a contract has a data location:
  - **Storage**:
    - global memory available to all functions within a contract.
    - permanent storage that Ethereum stores on every node.
  - **Memory**:
    - local memroy available to every function within a contract.
    - short living in functions.
  - **Calldata**:
    - where all incoming function execution data is stored(including function arguments)
    - non-modifiable memory location(note: similar to memrory location, except it is not modifiable)
  - **Stack** - a stack which is maintained by EVM(Etheereum Virtual Machine) for loading variables and intermediate values for working with Ethereum instruction set(the working set memory for the EVM). - max limit is 1024 levels, and exceeding this limit(by storing anything more than that), raises an exception.

> **Note**: data location of variable, is dependent on:

        - Location of the variable declaration
        - Data type of the variable

> **Note**: We face them mostly, when are working with reference type variables.

## Events

- Used for logging(like other languages)
- Used to notify applications about changes in contracts
- can be used to "call" JavaScript callbacks in the user interface of a dapp
- Primarily for informing the calling application about the current state of the contract
- Declared with 'event' keyword
- Firing them with 'emit' keyword
- Note: Events can be declared anonymous.
- Note: Events can have 'indexed' keyword in variable declaration(to make easier filtering of some specific data)
- Ex:
  ```
  event EventName(address sender,uint number);
  emit EventName(address(0), 10);
  ```

## Error Handling

- 1.  Require

  - Check a condition,
    if true => go to next line codes,
    if false => show a string message and revert to privious state
  - Refund remaining gas to the caller
  - Note: Use require conditions, all at beginning the function.
  - Ex: `require(number >= 10, "number must be greater than 10");`

- 1.  Revert

  - Similar to require, but can have more complex conditions
  - Refund remaining gas to the caller

  - Ex: `if(number <= 10) { revert("number must be greater than 10"); }`

- 1.  Assert

  - Mainly used in writing contract tests
  - Don't refund remaining gas to the caller

  * Ex:

  ```
  uint number = 123;
  function setNumber() {
      // code
      assert(number == 10);
  }
  ```

## Enum

- Ex:
  ```
  enum OrderStatus { pending, accepted, completed, rejected};
  OrderStatus order = OrderStatus.accepted;
  ```

## Mapping

Declaration:
`mapping(_KeyType => _ValueType) mappingName`

Mappings are like **Dictionary** or **hash table** which are virtually initialized such that every possible key exists and is mapped to a specific value.

**key** can be any type which EVM internally knows about it(exceptions are: a dynamically sized array, a contract, an enum, or a struct.
**value** can actually be any type, including mappings.

## Struct

Somehow we can pack some data types(and ofcourse data values) of an specific entity with structs:

- Ex:

  ```
  struct UserInfo { string fName; string lName; uint8 age; address wallet;}
  UserInfo user;
  ```
