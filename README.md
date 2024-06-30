# Functions And Errors Metacrafters - DIGITAL IDENTITY

This program is to test out statements such as `require`, `assert`, and `revert` of the solidity programming language. The purpose of the program was to demonstrate real-life innovation such as the use of digital identity with analyzing gas fees.

## Description

-  The program is a smart contract on the Ethereum blockchain written in Solidity. It returns a contract with various functions such as `createIdentity`, `deleteIdentity`, `unsafeDeleteIdentity`, `updateIdentity`, `getIdentity`, and `validateIdentity`. Some of these functions must implement `require()`, `assert()`, and `revert()` statements.

-  Basically, a digital identity smart contract represents a decentralized, secure construct on a blockchain to manage and verify identities. By using smart contracts, the project ensures that identity data remains tamper-proof and verifiable, enhancing trust and transparency in digital interactions.

## Getting Started

### Installing and Executing the program

To run this program, one can utilize the provided IDE for Solidity which is Remix. To access the website, please direct to this site [Remix IDE](https://remix.ethereum.org/)

Once you are on the website, create a new file on the "+" icon in the left-handed sidebar. Save the file with a `.sol` extension (e.g., Digital_Identity.sol). Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/*
    Smart Contract Project
        For this project, write a smart contract that implements the 
            require(), 
            assert() and 
            revert() statements.
*/

contract Digital_Identity {
    struct Identity {
        string name;
        uint256 age;
        uint256 id;
    }

    mapping(address => Identity) private identities;
    uint256 private idCounter;

    event IdentityCreated(address indexed user, uint256 id);
    event IdentityUpdated(address indexed user, uint256 id);
    event IdentityDeleted(address indexed user, uint256 id);

    constructor() {
        idCounter = 1; // Start ID counter at 1
    }

    
    function createIdentity(string memory _name, uint256 _age) public {
        require(bytes(_name).length > 0, "Name cannot be empty");
        require(_age > 0, "Age must be greater than zero");
        require(identities[msg.sender].id == 0, "Identity already exists");

        //ensure name and age are valid and that the identity does not already exist.
        identities[msg.sender] = Identity(_name, _age, idCounter);
        emit IdentityCreated(msg.sender, idCounter);
        idCounter++;
    }

    function getIdentity() public view returns (string memory name, uint256 age, uint256 id) {
        Identity memory identity = identities[msg.sender];
        require(identity.id != 0, "Identity does not exist"); //to ensure the identity exists.
        return (identity.name, identity.age, identity.id);
    }

    function updateIdentity(string memory _name, uint256 _age) public {
        Identity storage identity = identities[msg.sender];
        require(identity.id != 0, "Identity does not exist"); //to ensure the identity exists.

        //to ensure the new name and age are valid.
        require(bytes(_name).length > 0, "Name cannot be empty");
        require(_age > 0, "Age must be greater than zero");

        identity.name = _name;
        identity.age = _age;
        emit IdentityUpdated(msg.sender, identity.id);
    }

    function deleteIdentity() public {
        Identity memory identity = identities[msg.sender];
        require(identity.id != 0, "Identity does not exist");  //to ensure the identity exists.

        delete identities[msg.sender];
        emit IdentityDeleted(msg.sender, identity.id);
    }

    /*
        Test validateIdentity:
        1. Call the validateIdentity function with the address of the account that created an identity (Account 1).
        2. Observe the behavior. The function should return true.
        3. Switch to another account (Account 2) in the "Deploy & Run Transactions" tab.
        4. Call the validateIdentity function with the address of Account 2 (which has not created an identity).
        5. Observe the behavior. The function should trigger an assertion failure.
    */

    //to ensure that the identity exists for the given address.
    function validateIdentity(address _user) public view returns (bool) {
        Identity memory identity = identities[_user];
        assert(identity.id != 0); // Assert that identity exists
        return true;
    }

    /*
        Test unsafeDeleteIdentity:
        1. Switch to another account (Account 2) in the "Deploy & Run Transactions" tab.
        2. Attempt to delete the identity of Account 1 by calling the unsafeDeleteIdentity function with Account 1’s address.
        3. Observe the behavior. The function should revert since the caller is not the owner of the identity.
    */

    //to revert the transaction if the identity does not exist for the given address.
    function unsafeDeleteIdentity(address _user) public {
        Identity memory identity = identities[_user];
        if (identity.id == 0) {
            revert("Identity does not exist for this user"); // Revert if identity does not exist
        }

        delete identities[_user];
        emit IdentityDeleted(_user, identity.id);
    }
}
```

To compile, click on the `Solidity Compiler` tab in the left-handed sidebar. Make sure the Compiler option is set to `0.8.18` (or another compatible version), and then click on the `Compile Digital_Identity.sol` button.

Once the compiler does not return any errors, you can deploy the contract by clicking on the `Deploy & Run Transactions` tab in the left-hand sidebar. Select any account address that contains 100 ether. Select the `Digital_Identity` contract from the dropdown menu, and then click on the `Deploy` button. 

### Sample Demonstration of the Digital Identity

Once the deployment is successful, you can proceed for a sample demonstration.

#### Function `createIdentity`

#### Function `getIdentity`

#### Function `updateIdentity`

#### Function `validateIdentity`

#### Function `deleteIdentity`

#### Function `unsafeDeleteIdentity`

* How to run the program
* Step-by-step bullets
```
code blocks for commands
```

## Help

If it somehow happens that the functions for `validateIdentity` or `unsafeDeleteIdentity` returns an error stating that `address does not exist` , please review the Ethereum address and make it sure it is `valid`.

## Authors

Vladimir Al Guerrero
[@Linkedin](https://www.linkedin.com/in/vladimir-al-guerrero-178b6a24b/)

## License

This project is licensed under the MIT License - see the `LICENSE.md` file for details
