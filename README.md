# **Deploying a Smart Contract on Local Anvil Testnet**

## **1️⃣ Setting Up Foundry & Anvil**
Before deploying a smart contract, ensure you have Foundry installed. If not, install it :
```sh
curl -L https://foundry.paradigm.xyz | bash
foundryup
```
Start the Anvil local testnet :
```sh
anvil
```
Anvil will provide RPC URL (e.g., `http://127.0.0.1:8545`) & test accounts

---

## **2️⃣ Building the Smart Contract**
Navigate to your Foundry project folder & build the contract :
```sh
forge build
```
Fix any errors if they appear

---

## **3️⃣ Testing the Smart Contract**
Run the tests to ensure your contract works as expected :
```sh
forge test
```

---

## **4️⃣ Deploying the Smart Contract**
On local anvil testnet :
```sh
forge script script/YourContract.sol --private-key <PRIVATE_KEY> --sender <senderAdress> --broadcast 
```
You will see a deployment success message with the **contract address**

---

## **5️⃣ Verifying Deployment**
To verify that the contract is deployed, use :
```sh
cast call <CONTRACT_ADDRESS> "yourGetterFunction()"
```
If the function returns valid data(hax formatted data), the contract is deployed correctly

---
---
---
---

# **Interacting with the Contract from the Frontend**

## **1️⃣ Install Ethers.js**
In your frontend project, install Ethers.js :
```sh
npm install ethers
```

---

## **2️⃣ Set Up the Contract in Frontend**
Declares configuration for contracts like ABI, Contract Address, Provider

```javascript
import { ethers } from "ethers";

const contractAddress = "0xYourDeployedContractAddress"; 

const contractABI = [ /* Paste ABI here */ ];

const provider = new ethers.JsonRpcProvider("http://127.0.0.1:8545");

const signer = await provider.getSigner();
```
Make two separate instance of contract using provider & signer
```javascript
const contractProvider = new ethers.Contract(contractAddress, contractABI, provider); // Use for read only functions

const contractSigner = new ethers.Contract(contractAddress, contractABI, signer); // Use for tx functions
```

---

## **3️⃣ Calling Read Functions (Getter Methods)**
For reading contract data :
```javascript
async function getNumber() {
    const number = await contractProvider.getNumber();
    console.log("Stored Number:", number.toString());
}
getNumber();
```

---

## **4️⃣ Sending Transactions (Write Functions)**
For updating contract data :
```javascript
async function setNumber(value) {
    const tx = await contractSigner.setNumber(value);
    await tx.wait(); // Wait for transaction confirmation
    console.log("Number updated successfully!");
}
setNumber(42);
```

---

## **5️⃣ Listening for Events**
If your contract emits events, listen for them :
```javascript
contract.on("NumberUpdated", (newValue) => {
    console.log("Number updated event received:", newValue.toString());
});
```

