# road-to-web3 
Code with me: https://alchemy.com/?r=9453f902fe1eda25 
This is a basic ERC721 smart contract of 10,000 NFT using Open Zeppelin Smart Conttracts.

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.4;

    import "@openzeppelin/contracts@4.8.0/token/ERC721/ERC721.sol";
    import "@openzeppelin/contracts@4.8.0/token/ERC721/extensions/ERC721Enumerable.sol";
    import "@openzeppelin/contracts@4.8.0/token/ERC721/extensions/ERC721URIStorage.sol";
    import "@openzeppelin/contracts@4.8.0/access/Ownable.sol";
    import "@openzeppelin/contracts@4.8.0/utils/Counters.sol";

    contract WitCode is ERC721, ERC721Enumerable, ERC721URIStorage, Ownable {
    using Counters for Counters.Counter;
    
    Counters.Counter private _tokenIdCounter;
    // private for security so that only the contract reads itself & not any other users or other type of smart contracts
    uint256 MAX_SUPPLY = 10000

    constructor() ERC721("WitCode", "WTC") {}

    function safeMint(address to, string memory uri) public onlyOwner {    
    // public onlyOwner so that any other user can intereact with our smart contract
       
        uint256 tokenId = _tokenIdCounter.current();
        require(tokenId <= MAX_SUPPLY, "Ah Snap, All NFTs have been minted");
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }

    // The following functions are overrides required by Solidity.

    function _beforeTokenTransfer(address from, address to, uint256 tokenId, uint256 batchSize)
        internal
        override(ERC721, ERC721Enumerable)
    {
        super._beforeTokenTransfer(from, to, tokenId, batchSize);
    }

    function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721Enumerable)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
