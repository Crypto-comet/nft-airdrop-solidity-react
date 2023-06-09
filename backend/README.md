# Digital Critters NFT Collection Backend

## Execution Instructions

### `ENV` File Setup

1. Create a `.env` file at the root level
2. Add all of these variables inside it.

```bash
GOERLI_RPC_URL=https://eth-goerli.g.alchemy.com/v2/...
PRIVATE_KEY=...
ETHERSCAN_API_KEY=...
PINATA_API_KEY="..."
PINATA_API_SECRET="..."
UPDATE_FRONTEND=false
```

Get the GOERLI RPC URL from [Alchemy Dashboard](https://www.alchemy.com/)  
Get the PRIVATE KEY from your Metamask Wallet.  
Get the ETHERSCAN API KEY from [Etherscan Dashboard](https://etherscan.io/)  
Get the PINATA API KEY & SECRECT from [Pinata Cloud](https://www.pinata.cloud/)  
Read the below instructions to see their use.

### Deploy Files Setup

There are two files inside deploy folder which will run when you use the `npx hardhat deploy` command.

1. `01-deploy-nft-airdrop.ts` file will deploy the contract on the network of your choice (Make sure to set the settings for your prefered network in side `hardhat.config.ts` file).
2. `99-update-frontend.ts` file will automatically update the contractAddresses and latest ABI of your smart contract for your frontend.

#### 1. `01-deploy-nft-airdrop.ts` file setup

1. For minting the NFTs, its metadata files will be required which will contain the information about your NFTs and image hashes (for generating see the **Scripts Files Setup Section**) for that specify the hashes of metadata files at this line `const metadataIpfsHashes: string[] = [];`
2. Update the name of your NFT at this line `const nftName: string = "Digital Critters Collection";`
3. Update the symbol of your NFT at this line `const nftSymbol: string = "DCC";`

#### 2. `99-update-frontend.ts` file setup

1. You can turn on `UPDATE_FRONTEND` variable inside `.env` file to `true` when you want to update the contractAddresses and ABI on the frontend side.
2. By default the frontend folder in place inside the same folder of backend, but if you change the location of frontend folder in your pc then update these two variables.

```js
const FRONTEND_CONTRACT_ADDRESSES_FILE_PATH: string =
    "../frontend/src/constants/contractAddresses.json";
const FRONTEND_ABI_FILE_PATH: string = "../frontend/src/constants/abi.json";
```

### Scripts Files Setup

There are four scripts inside this folder;

1. `generate-merkle-tree.ts` which will generate the Merkle Tree for provided list of addresses and return the `tree` instance of type `Merkle Tree`.
2. `upload-image-to-pinata.ts` which upload you NFT image on pinata.
3. `upload-metadata-pinata.ts` which upload your NFT metadata on pintata.
4. `mint-nft.ts` which mint the nft.

#### 1. `generate-merkle-tree.ts` file setup

1. Specify the list of addresses inside the `allowList`.
2. `generateMerkleTree` function will be called while deploying the contract and `const root = tree.getHexRoot();` this line will generate the root hash of Merkle Tree of given addresses in the `allowList`.

**Note: Once you deployed the contract with the root hash of Merkle Tree of given allowList addresses you will not be able to modify it again (No even the Owner, you can change this feature if you want).**

#### 2. `upload-image-to-pinata.ts` file setup

1. Make sure you add the `PINATA_API_KEY` and `PINATA_API_SECRET` inside `.env` file.
2. Add your NFT images inside the **images/** folder.
3. Update the image file path at this line `const IMAGE_FILE_PATH: string = "images/xyz.png";`
4. Update the image file name for pinata server at this line `const response = await pinata.pinFileToIPFS(readableStreamForFile, {
            pinataMetadata: { name: "nft-name.png" },
        });`
5. You need to run this script for each individual image in order to upload it to pinata or you can create an script to upload all at once.

#### 2. `upload-metadata-pinata.ts` file setup

1. Make sure you add the `PINATA_API_KEY` and `PINATA_API_SECRET` inside `.env` file.
2. Metadata template has some predefined attributes for NFT which you can change according to your NFT. For more info read here [Opensea Metadata Standard](https://docs.opensea.io/docs/metadata-standards).
3. Update the name of you NFT at this line `nftMetadata.name = "";`
4. Update the description of your NFT at this line ` nftMetadata.description = ""`
5. Specify the hash of your NFT (generated by above script) at this line `const ipfsHash: string = "";`
6. Update the metadata file name for your pinata server (if you want) at this line `const response = await pinata.pinJSONToIPFS(nftMetadata, {
            // * specify the name of the metadata file for pinata dashboard.
            pinataMetadata: { name: "" },
        });`
7. You need to run this script for each image hash.

### Install dependencies

1. Install the dependencies `npm install --force`