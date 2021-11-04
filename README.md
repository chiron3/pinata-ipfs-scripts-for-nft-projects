# Pinata IPFS scripts for NFT projects

This page tells you how to get started uploading NFT images to Pinata, computing the sha256 hashes, and how to associate a TokenID to specific IPFS metadata.

<a href="https://www.buymeacoffee.com/coderrob" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-green.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

## Scripts

### Calculate File IPFS CIDs

`/src/calculate-cids.js`

The calculate file CIDs script will iterate the contents of a specified folder, and for each file will compute the IPFS hash CID mapped to the file name.

Once complete the script will output the file name and CID mappings to a file.

#### Settings

`var: outputPath` - The relative output file path. Defaulted to `./output/file-cids.json`.

`var: folderPath` - The relative folder path containing the files to be processed. Each file will have its name and CID mapped. Defaulted to the `files` folder.

#### Command

```bash
node ./src/calculate-cids.js
```

#### Output

`./output/file-cids.json`

#### Contents

```json
{
  "one.png": "QmZPnX4481toHABEtvKFoCWoVuzFFQRBiA5QR2Cij9pjon",
  "two.png": "QmazpAaWf3Bb4qhSW9PnQXfj2URbQwdNbZvDr77RbwH7xb"
}
```

### Calculate File sha256 Hashes

`/src/calculate-hashes.js`

The calculate file hashes script will iterate the contents of a specified folder, and for each file will compute the sha256 hash mapped to the file name.

Once complete the script will output the file name and sha256 hash mappings to a file.

#### Settings

`var: outputPath` - The relative output file path. Defaulted to `./output/file-hashes.json`.

`var: folderPath` - The relative folder path containing the files to be processed. Each file will have its name and CIDsha256 hash mapped. Defaulted to the `files` folder.

#### Command

```bash
node ./src/calculate-hashes.js
```

#### Output

`./output/file-hashes.json`

#### Contents

```json
{
  "one.png": "f8e50b5c45e6304b41f87686db539dd52138b873a3af98cc60f623d47a133df2",
  "two.png": "76d9c6f8dc113fff71a180195077526fce3d0279034a37f23860c1f519512e94"
}
```

### Download Pinata Pinned CIDs

`/src/download-cids.js`

The download file CIDs script will iterate all pinned files associated with the Pinata API Key. The script will map each row's file name and IPFS hash CID.

Once complete the script will output the file name and CID mappings to a file.

#### Settings

`var: outputPath` - The relative output file path. Defaulted to `./output/downloaded-cids.json`

`env: PINATA_API_KEY` - The Pinata API Key environment value

`evn: PINATA_API_SECRET` - The Pinata API Secret environment value

#### Command

```bash
node ./src/download-cids.js
```

#### Output

`./output/downloaded-cids.json`

#### Contents

```json
{
  "one.png": "QmZPnX4481toHABEtvKFoCWoVuzFFQRBiA5QR2Cij9pjon",
  "two.png": "QmazpAaWf3Bb4qhSW9PnQXfj2URbQwdNbZvDr77RbwH7xb"
}
```

### Upload Files

`/src/upload-files.js`

The upload files script will iterate the contents of a specified folder, and will upload and pin each _individual_ file to Pinata. After a successful upload the file name will be mapped to the IPFS hash CID from the response.

Once complete the script will output the file name and CID mappings to a file.

#### Settings

`var: pinataCIDs` - To prevent re-uploading already pinned files in Pinata. This variable is loaded with the json contents of the `./ouput/downloaded-cids.json` file if one exists. These CID mappings will help prevent re-uploading a file that has already been pinned in Pinata.

`var: outputPath` - The relative output file path. Defaulted to `./output/uploaded-cids.json`.

`var: folderPath` - The relative folder path to read and upload all local files to be pinned with Pinata.  Defaulted to the `files` folder.

`env: PINATA_API_KEY` - The Pinata API Key environment value

`env: PINATA_API_SECRET` - The Pinata API Secret environment value

#### Command

```bash
node ./src/upload-files.js
```

#### Output

`./output/uploaded-cids.json`

#### Contents

```json
{
  "one.png": "QmZPnX4481toHABEtvKFoCWoVuzFFQRBiA5QR2Cij9pjon",
  "two.png": "QmazpAaWf3Bb4qhSW9PnQXfj2URbQwdNbZvDr77RbwH7xb"
}
```

### Upload Metadata

`/src/upload-metadata.js`

The upload files script will iterate the contents of a specified folder, and will upload and pin each _individual_ file to Pinata. After a successful upload the file name will be mapped to the IPFS hash CID from the response.

Once complete the script will output the file name and CID mappings to a file.

> **Note** - To support `ipfs/<CID>/<TokenId>` such as `ipfs/QmR5m9zJDSmrLnYMawrySYu3wLgN5afo3yizevAaimjvmD/0` simple name the JSON files numerically and strip the file extensions. This will allow the files to be accessed by file name that can be mapped to the `TokenId`.

![Pinata pinned file list](https://github.com/Coderrob/nft-pinata-bulk-upload/tree/master/img/pinned-list.png)

![metadata folder container list](https://github.com/Coderrob/nft-pinata-bulk-upload/tree/master/img/metadata-list.png)

![File 0 metadata json](https://github.com/Coderrob/nft-pinata-bulk-upload/tree/master/img/metadata-0.png)

#### Settings

`var: outputPath` - The relative output file path. Defaulted to `./output/folder-cid.json`.

`var: folderName` - The folder name to use for the uploaded folder of json metadata. This can be changed to any name you'd like that identifies the collection of metadata files.  Defaulted to `metadata` as the folder name.

`var: folderPath` - The relative folder path to read and upload all local files to be pinned in Pinata as a folder container for the uploaded files. Defaulted to the `metadata` folder.

`env: PINATA_API_KEY` - The Pinata API Key environment value

`env: PINATA_API_SECRET` - The Pinata API Secret environment value

#### Command

```bash
node ./src/upload-metadata.js
```

#### Output

`./output/folder-cid.json`

#### Contents

```json
{ 
    "metadata": "QmR5m9zJDSmrLnYMawrySYu3wLgN5afo3yizevAaimjvmD" 
}
```