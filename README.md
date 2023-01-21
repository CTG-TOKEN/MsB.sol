pragma solidity ^0.8.0;

contract MultiSigWallet {
    address[] public owners;
    mapping(address => bool) public isOwner;
    address public owner;
    address public spender;
    address public recipient;
    uint public value;

    constructor(address[] memory _owners) public {
        owners = _owners;
        for (uint i = 0; i < owners.length; i++) {
            isOwner[owners[i]] = true;
        }
    }

    function execute(address _spender, address _recipient, uint _value) public {
        require(isOwner[msg.sender], "Sender is not an owner of the contract");
        require(isOwner[spender], "Spender is not an owner of the contract");
        spender = _spender;
        recipient = _recipient;
        value = _value;
    }

    function confirm(address _spender, address _recipient, uint _value) public {
        require(spender == _spender, "Spender does not match the transaction");
        require(recipient == _recipient, "Recipient does not match the transaction");
        require(value == _value, "Value does not match the transaction");
        require(isOwner[msg.sender], "Sender is not an owner of the contract");
        spender.transfer(_value);
    }
}
