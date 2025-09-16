# Create a DV Using the SDK

This is a walkthrough of using the [Obol-SDK](https://www.npmjs.com/package/@obolnetwork/obol-sdk) to propose a four-node distributed validator cluster for creation using the [DV Launchpad](https://docs.obol.org/next/learn/intro/launchpad).

### Pre-requisites[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#pre-requisites) <a href="#pre-requisites" id="pre-requisites"></a>

* You have [node.js](https://nodejs.org/en) installed.

### Install the package[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#install-the-package) <a href="#install-the-package" id="install-the-package"></a>

Install the Obol-SDK package into your development environment

{% tabs %}
{% tab title="NPM" %}
```sh
npm install --save @obolnetwork/obol-sdk
```
{% endtab %}

{% tab title="Yarn" %}
```sh
yarn add @obolnetwork/obol-sdk
```
{% endtab %}
{% endtabs %}

### Instantiate the client[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#instantiate-the-client) <a href="#instantiate-the-client" id="instantiate-the-client"></a>

The first thing you need to do is create an instance of the Obol SDK client. The client takes two constructor parameters:

* The `chainID` for the chain you intend to use.
* An ethers.js [signer](https://docs.ethers.org/v6/api/providers/#Signer-signTypedData) object.

```sh
import { Client } from "@obolnetwork/obol-sdk";
import { ethers } from "ethers";

// Create a dummy ethers signer object with a throwaway private key
const mnemonic = ethers.Wallet.createRandom().mnemonic?.phrase || "";
const privateKey = ethers.Wallet.fromPhrase(mnemonic).privateKey;
const wallet = new ethers.Wallet(privateKey);
const signer = wallet.connect(null);

// Instantiate the Obol Client for holesky
const obol = new Client({ chainId: 17000 }, signer);
```

### Propose the cluster[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#propose-the-cluster) <a href="#propose-the-cluster" id="propose-the-cluster"></a>

List the Ethereum addresses of participating operators, along with withdrawal and fee recipient address data for each validator you intend for the operators to create.

```sh
// A config hash is a deterministic hash of the proposed DV cluster configuration
const configHash = await obol.createClusterDefinition({
  name: "SDK Demo Cluster",
  operators: [
    { address: "0xC35CfCd67b9C27345a54EDEcC1033F2284148c81" },
    { address: "0x33807D6F1DCe44b9C599fFE03640762A6F08C496" },
    { address: "0xc6e76F72Ea672FAe05C357157CfC37720F0aF26f" },
    { address: "0x86B8145c98e5BD25BA722645b15eD65f024a87EC" },
  ],
  validators: [
    {
      fee_recipient_address: "0x3CD4958e76C317abcEA19faDd076348808424F99",
      withdrawal_address: "0xE0C5ceA4D3869F156717C66E188Ae81C80914a6e",
    },
  ],
});

console.log(
  `Direct the operators to https://holesky.launchpad.obol.org/dv?configHash=${configHash} to complete the key generation process`
);
```

### Invite the Operators to complete the DKG[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#invite-the-operators-to-complete-the-dkg) <a href="#invite-the-operators-to-complete-the-dkg" id="invite-the-operators-to-complete-the-dkg"></a>

Once the Obol-API returns a `configHash` string from the `createClusterDefinition` method, you can use this identifier to invite the operators to the [Launchpad](https://docs.obol.org/next/learn/intro/launchpad) to complete the process

1. Operators navigate to `https://<NETWORK_NAME_HERE>.launchpad.obol.org/dv?configHash=<CONFIG_HASH_HERE>` and complete the [run a DV with others](https://docs.obol.org/next/run/start/quickstart_group) flow.
2. Once the DKG is complete, and operators are using the `--publish` flag, the created cluster details will be posted to the Obol API.
3. The creator will be able to retrieve this data with `obol.getClusterLock(configHash)`, to use for activating the newly created validator.

### Retrieve the created Distributed Validators using the SDK[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#retrieve-the-created-distributed-validators-using-the-sdk) <a href="#retrieve-the-created-distributed-validators-using-the-sdk" id="retrieve-the-created-distributed-validators-using-the-sdk"></a>

Once the DKG is complete, the proposer of the cluster can retrieve key data such as the validator public keys and their associated deposit data messages.

```sh
const clusterLock = await obol.getClusterLock(configHash);
```

Reference lock files can be found [here](https://github.com/ObolNetwork/charon/tree/main/cluster/testdata).

### Activate the DVs using the deposit contract[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#activate-the-dvs-using-the-deposit-contract) <a href="#activate-the-dvs-using-the-deposit-contract" id="activate-the-dvs-using-the-deposit-contract"></a>

In order to activate the distributed validators, the cluster operator can retrieve the validators' associated deposit data from the lock file and use it to craft transactions to the `deposit()` method on the deposit contract.

```sh
const validatorDepositData =
  clusterLock.distributed_validators[validatorIndex].deposit_data;

const depositContract = new ethers.Contract(
  DEPOSIT_CONTRACT_ADDRESS, //0x0a502f846F6dc2e3D4d8C595B18b3AF44657B1bD  for Mainnet, 0xff50ed3d0ec03aC01D4C79aAd74928BFF48a7b2b for Goerli
  depositContractABI, // https://etherscan.io/address/0x0a502f846F6dc2e3D4d8C595B18b3AF44657B1bD#code for Mainnet, and replace the address for Goerli
  signer
);

const TX_VALUE = ethers.parseEther("32");

const tx = await depositContract.deposit(
  validatorDepositData.pubkey,
  validatorDepositData.withdrawal_credentials,
  validatorDepositData.signature,
  validatorDepositData.deposit_data_root,
  { value: TX_VALUE }
);

const txResult = await tx.wait();
```

### Usage Examples[​](https://docs.obol.org/next/adv/advanced/quickstart-sdk#usage-examples) <a href="#usage-examples" id="usage-examples"></a>

Examples of how our SDK can be used are found [here](https://github.com/ObolNetwork/obol-sdk-examples).
