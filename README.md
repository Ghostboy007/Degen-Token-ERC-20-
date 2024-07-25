# Metacrafters- ETH + AVAX PROOF: Intermediate EVM Course - Module 4
## Description

This course teaches how to build decentralized applications (Dapps) on the Ethereum blockchain and Avalanche using the Solidity programming language.
The course teaches how to create smart contracts, connect them to wallets, build a user interface, and deploy your Dapps. This is the detailed review of the fourth module of the same course. 

## Code Explanation

```Solidity

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

Importing the OpenZeppelin ERC20 Contract and inheriting it to my contract using `is` keyword.

Using the constructor initlializing the owner state variable and minting some token for the owner.

```Solidity
  constructor(uint _tokenToMint) ERC20("Degen","DGN"){
        owner = msg.sender;
        gameAsset = new GameAsset();
        _mint(msg.sender, _tokenToMint); // very small amount because it takes high gas fees 
    } 
```

Minting the reward Token for another account ( its like giving the reward in the form of Tokens)

- I am not minning but transfering tokens because to save the transaction cost.

```Solidity

 ///@notice to reward a certain user by _amount amount only callable by the owner.

    function mintTokenReward(address _address,uint _amount) external onlyOwner{
        _mint(_address,_amount);
    }
```

Through this checkingBalance function anyone can check his/her token balance.
In degen game it will help to show/display/check the token balance of the current account.

```Solidity

  ///@notice for checking the balance of token of caller account.


    function checkingBalance() external view returns(uint){
        return balanceOf(msg.sender);
    }

```

Through transferTokens transfering some amount of tokens from the caller account to the recipient account by some required check i.e user must have equal to greater than that tokens.
In degen Game it will help to share game resources with other players.

```Solidity
function tranferTokens(address _recepient, uint _amount) external{
        require(balanceOf(msg.sender) >= _amount);
       transfer(_recepient, _amount);

    }
```

Redeeming one tokens for one NFT (Game Asset NFT).

```Solidity

   ///@notice redeeming one token for a NFT 
    function redeemTokens() external{
        require(balanceOf(msg.sender) >= 1);
        _transfer(msg.sender, address(this), 1);
        gameAsset.safeMint(msg.sender);
    }

```

Burning the game assets or Degen Game tokens.

```Solidity


 ///@notice burn the _tokenAmount amount of token

    function burnToken(uint _tokenAmount) external {
        require( balanceOf(msg.sender)>=_tokenAmount);
        _burn(msg.sender, _tokenAmount);
    }

```

In case of emergency the owner the withdraw all the tokens from the contract through withdraw function.

```Solidity
///@notice to withdraw all tokens
    function withdraw() external onlyOwner{
          _transfer(address(this), owner, balanceOf(address(this)));
    }
```

Now the code of GameAsset Contract

```Solidity

// SPDX-License-Identifier: MIT
// Compatible with OpenZeppelin Contracts ^5.0.0
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";


contract GameAsset is ERC721 {
    uint256 private _nextTokenId;

    constructor()
        ERC721("GameAsset", "GAT")
    {}

    function _baseURI() internal pure override returns (string memory) {
        return "https://ipfs.io/ipfs/QmUKHAfQqRsNDdyAtgKkQtkk5atn7t9nuGSpaZ4wh4vcdh";
    }

    function safeMint(address to) public {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
    }
}
```

In this contract only two functions are there 
- _baseURI : This Function sets the NFT's URI for minting.
- safeMint : This Function safely mints (address!=address(0)) the NFT to the ```to``` addresss.



The author of this file is Utkarsh Vinayak.
email id : utkarshvinayak7@gmail.com
