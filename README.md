
---

# Practical 1 – Python RSA (Public & Private Key)

## Aim

Generate public and private keys using Python RSA and test encryption and decryption.

## Installation

```bash
pip install pycryptodome
```

## Python Code

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

key = RSA.generate(2048)
private_key = key
public_key = key.publickey()

encryptor = PKCS1_OAEP.new(public_key)
decryptor = PKCS1_OAEP.new(private_key)

message = b'hello RSA'

encrypted = encryptor.encrypt(message)
print("Encrypted:", encrypted)

decrypted = decryptor.decrypt(encrypted)
print("Decrypted:", decrypted)
```

## Client Class Example

```python
import binascii
import Crypto
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5

class client:
    def __init__(self):
        random = Crypto.Random.new().read
        self._private_key = RSA.generate(1024, random)
        self._public_key = self._private_key.publickey()
        self._signer = PKCS1_v1_5.new(self._private_key)

    @property
    def identity(self):
        return binascii.hexlify(
        self._public_key.exportKey(format='DER')
        ).decode('ascii')

TYIT = client()
print("\npublic key:", TYIT.identity)
```

## Normal RSA Solving

```python
P = 3
Q = 11
N = P * Q
Phi = (P-1) * (Q-1)

E = 7
D = 3

def encryption(m):
    return (m**E) % N

def decryption(c):
    return (c**D) % N

message = 4

print("Original Message:", message)

encrypted = encryption(message)
print("Encrypted Message:", encrypted)

decrypted = decryption(encrypted)
print("Decrypted Message:", decrypted)
```

---

# Practical 2 – CoinMarketCap Exchange Rate

## Aim

Find cryptocurrency exchange rate change using CoinMarketCap.

## Steps

1. Open website

```
https://coinmarketcap.com
```

2. Search **Bitcoin (BTC)**

3. Note the values

```
Day 1 Price = P1
Today Price = Pn
```

## Formula

```
Final Change (%) = (Pn − P1) / P1 × 100
```

## Example

```
P1 = 91,399.17
Pn = 66,223.22
```

```
Final Change = (66,223.22 − 91,399.17) / 91,399.17 × 100
             = −27.55%
```

## Log Return

```
R = ln(Pn / P1)
```

```
R = ln(66,223.22 / 91,399.17)
R = −0.322
```

## Conclusion

Bitcoin price decreased by **27.55%**.

---

# Practical 3 – Calculate Number of Bitcoins

## Aim

Calculate the number of Bitcoins based on investment.

## Formula

```
BTC Amount = Investment / Price of 1 BTC
```

## Python Code

```python
investment = float(input("Enter investment amount: "))
btc_price = float(input("Enter price of BTC: "))

btc_amount = investment / btc_price

print("You will get", btc_amount, "BTC")
```

## Example

```
Investment = 1000
BTC Price = 50000
```

```
BTC = 1000 / 50000
BTC = 0.02
```

---

# Practical 4 – Transaction Class (Send and Receive Money)

## Python Code

```python
class bank:

    def __init__(self):
        self.balance = 0
        print("shelly\n")
        print("Account created with balance 0")

    def deposit(self):
        amount = float(input("Enter amount to deposit: "))
        self.balance = self.balance + amount
        print("The deposit is successful and the balance is %f" %self.balance)

    def withdraw(self):
        amount = float(input("Enter amount to withdraw: "))
        if self.balance >= amount:
            self.balance = self.balance - amount
            print("The withdrawal is successful and the balance is %f" %self.balance)
        else:
            print("Insufficient balance")

    def enquiry(self):
        print("The balance is %f" %self.balance)

account = bank()
account.deposit()
account.withdraw()
account.enquiry()
```

---

# Practical 5 – MetaMask + Ganache Transaction

## Steps

Open **Ganache**

Copy RPC Server address

```
http://127.0.0.1:7545
```

Open **MetaMask**

```
3 lines → Networks → Add Custom Network
```

Enter:

```
Network Name : Localhost 8545
RPC URL : http://127.0.0.1:7545
Chain ID : 1337
Currency Symbol : ETH
```

top left corner > account
add wallet > import an account

```
Ganache → Key icon → Copy private key
MetaMask → Import Account → Paste private key
```

Now account with **100 ETH** appears.

Send Transaction

```
Click Send → ETH ->Enter receiver address → Amount (2 ETH) → Confirm
```

Transaction appears in **Ganache**.

---

# Practical 6 – Remix IDE Smart Contract

Open

```
https://remix.ethereum.org
```

Create new file **tytokens.sol**

## Solidity Code

```solidity
pragma solidity ^0.8.31;

contract tytoken{

    string public name = "tytoken";
    string public symbol = "TYIT";
    uint256 public totalsupply;

    mapping (address => uint256) public balanceOf;

    constructor(uint256 initialsupply) {
        totalsupply = initialsupply;
        balanceOf[msg.sender] = initialsupply;
    }

    function transfer(address toWhom, uint256 value) public returns(bool success){

        require(balanceOf[msg.sender] >= value, "insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[toWhom] += value;

        return true;
    }
}
```

## Steps

1. Compile using **Solidity Compiler**
2. Go to **Deploy & Run Transactions**
3. Enter initial supply (example: 10000)
4. Click **Deploy**
5. click on deploy
6. copy address of 2nd account
7. scroll down 
8. go to deployed contracts:
9. expand transfer:
10. towhom: paste address of 2nd account
11. value: 5000
12. DONT FORGET TO CHOOSE THE 1ST ACCOUNT BEFORE TRANSACT
13. click on transact
14. balanceof: paste address of 1st account
15. click on balanceof

---

# Practical 7 – Basic DeFi (Decentralized Financial System)

## Aim

Create a basic **DeFi (Decentralized Financial System)** using **Solidity and Remix IDE**.

---

## Step 1: Open Remix IDE

Open the browser and go to:

```
https://remix.ethereum.org
```

---

## Step 2: Create a New File

Create a file named:

```
pract.sol
```

---

## Step 3: Write Solidity Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyToken is ERC20 {

    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, 1000000 * 10 ** decimals());
    }
}
```
compile the file
## Step 4: Deploy the Contract

1. Go to **Deploy & Run Transactions**.
2. Select **Remix VM**.
3. Select **1st account**.
4. Click **Deploy**.
5. After deployment:
6. Scroll to **Deployed Contracts**.
7.  Expand **MyToken**.
8. Use the **transfer** function.

Fill the values:

```
from: 1st account
to: 2nd account
value: 5000
```

Click:

```
transact
```

This transfers **5000 tokens** from the first account to the second account.

---

# Part 2 – Staking (DeFi Feature)

Now we create a **staking smart contract**.

---

## Step 1: Create a New File

Create another file named:

```
practice.sol
```

---

## Step 2: Write Staking Contract

```solidity
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract SimpleStaking {

    IERC20 public token;
    mapping(address => uint256) public stakes;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function stake(uint256 amount) external {
        require(amount > 0, "Zero amount");
        token.transferFrom(msg.sender, address(this), amount);
        stakes[msg.sender] += amount;
    }

    function unstake(uint256 amount) external {
        require(stakes[msg.sender] >= amount, "Not enough stake");
        stakes[msg.sender] -= amount;
        token.transfer(msg.sender, amount);
    }

}
```

## Step 3: Compile the Contract

Click:

```
Solidity Compiler → Compile practice.sol
```

---

## Step 4: Deploy the Contract

1. Go to **Deploy & Run Transactions**.
2. Select **1st account**.
3. Deploy the contract.

---

## Step 5: Interact with the Contract

After deployment:

1. Scroll to **Deployed Contracts**.
2. click stakes > paste 1st account address
3. then paste on "at address"
4. then click on at address
5. then go to deployed contracts open:
6. paste address on stakes > and click on stakes

---

# Practical 8 – Real World Smart Contract Cases

## Attendance Contract

```solidity
pragma solidity ^0.8.9;

contract Attendance {

    mapping(uint256 => bool) public isPresent;

    constructor(uint256[] memory rollno) {

        for (uint256 i = 0; i < rollno.length; i++) {
            isPresent[rollno[i]] = true;
        }
    }

    function attend(uint256 roll) public view returns (bool) {

        return isPresent[roll];
    }
}
```

Deploy Example

```
[1,2,3]
```

Check Attendance

```
attend(2) → true
attend(5) → false
```
## Steps

1. compile > deploy & run
2. scroll down to deploy
3. enter initialsupply (eg: [1,2,3]) 
4. Click **Deploy**
5. go to deployed contractes: 
6. attend: 2 (o/p: true)
7. ispresent: 5 (o/p: false)

---

## Donation Contract

```solidity
pragma solidity ^0.8.9;

contract Donation {

    address public owner;

    mapping(address => uint) public balances;

    event Donated (address indexed from, uint256 amount);

    constructor(){
        owner = msg.sender;
    }

    function donate() public payable {

        require(msg.value > 0, "Donation amount must be greater than zero");

        balances[msg.sender] += msg.value;

        emit Donated(msg.sender, msg.value);
    }

    function getDonationBalance() public view returns (uint256) {

        return address(this).balance;
    }
}
```
## Steps
1. compile > deploy & run:
2. directly click deploy 
3. then change the value to 1 in "value"
4. scroll down to deployed contracts:
5. click on deploy

---

# Practical 9 – MetaMask + Ganache + Truffle

## Steps

Open Ganache.
Copy RPC Server address
Example: http://127.0.0.1:7545

Open MetaMask

```
click on 3 lines > go to network > custom network
Network Name : Loaclhost 8545
RPC URL : http://127.0.0.1:7545
Chain ID : 1337
Currency : ETH
```
top left corner > account
add wallet > import an account

Go to Ganache
Click key icon
Copy private key

In MetaMask

```
Import Account → Paste private key
```

Create project folder

```
bkc node
```
Open CMD in folder

```
npm init -y
npm install -g truffle
npm install truffle lite-server
npx truffle init
```

Create smart contract

```
open bkc folder on vs code:
in contract folder > 
add add.sol:
```

```solidity
pragma solidity ^0.8.0;

contract Add{

    function add(uint a, uint b)
    public pure returns(uint){

        return a + b;
    }
}
```

Create migration file

```
migrations/1_deploy.js
```

```javascript
const Add = artifacts.require("Add");

module.exports = function (deployer) {
  deployer.deploy(Add);
};
```
in truffle.config file > search for "development"
remove comments for development
**port: 7545**

search for compilers: (change version to):
**version: 0.8.0**

go to cmd again

```
npx truffle compile --all
npx truffle migrate --reset
```
go to metamask > token 
see if deducted

---

# Practical 10 – DApp Frontend (Web3 + Smart Contract)

go to metamask > token 
see if deducted
then go to cmd in proj folder:

```
npm install web3
```
go to vs code > Create **index.html**

```html
<!doctype html>
<html>

<head>
<title>DApp</title>
<script src="https://cdn.jsdelivr.net/npm/web3/dist/web3.min.js"></script>
</head>

<body>

<h2>Add Two Numbers</h2>

<input id="num1" placeholder="Number 1" />
<input id="num2" placeholder="Number 2" />

<button onclick="add()">Add</button>

<h3 id="result"></h3>

<script>

let web3;
let contract;

const contractAddress = "PASTE_DEPLOYED_CONTRACT_ADDRESS";

const abi = [
{
inputs: [
{ internalType: "uint256", name: "a", type: "uint256" },
{ internalType: "uint256", name: "b", type: "uint256" }
],
name: "add",
outputs: [{ internalType: "uint256", name: "", type: "uint256" }],
stateMutability: "pure",
type: "function"
}
];

window.onload = async () => {

if (window.ethereum) {

web3 = new Web3(window.ethereum);

await window.ethereum.request({ method: "eth_requestAccounts" });

contract = new web3.eth.Contract(abi, contractAddress);

}

};

async function add() {

let a = document.getElementById("num1").value;

let b = document.getElementById("num2").value;

let result = await contract.methods.add(a, b).call();

document.getElementById("result").innerHTML = "Result = " + result;

}

</script>

</body>
</html>
```

Paste deployed contract address from:

```
truffle migrate --reset
```

Open **index.html** to test the DApp.

---

