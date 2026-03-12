---
# Blockchain Practical Experiments (BKC)

This repository contains practical implementations for **Blockchain concepts using Python, Ethereum, Ganache, MetaMask, Truffle, Solidity, and Web3.js**.
---

# Practical 1 – Python RSA (Public & Private Key)

Generate **public and private keys using Python RSA** and test encryption and decryption.

## Install dependency

```bash
pip install pycryptodome
```

## pract1.py

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

---

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

---

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

Find cryptocurrency exchange rate.

## Steps

1. Open

```
https://coinmarketcap.com
```

2. Search **Bitcoin (BTC)**

3. Note:

```
Day 1 Price = P1
Today Price = Pn
```

## Formula

```
Final Change (%) = (Pn − P1) / P1 × 100
```

### Example

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

R = ln(66,223.22 / 91,399.17)
R = −0.322
```

Conclusion:

```
Bitcoin decreased by 27.55%
```

---

# Practical 3 – Calculate Number of Bitcoins

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

### Example

```
Investment = 1000
BTC Price = 50000

BTC = 1000 / 50000
BTC = 0.02
```

---

# Practical 4 – Transaction Class (Python)

Create a class to simulate **deposit, withdraw, and enquiry operations**.

```python
class bank:

    def __init__(self):
        self.balance = 0
        print("Account created with balance 0")

    def deposit(self):
        amount = float(input("Enter amount to deposit: "))
        self.balance = self.balance + amount
        print("Deposit successful. Balance:", self.balance)

    def withdraw(self):
        amount = float(input("Enter amount to withdraw: "))
        if self.balance >= amount:
            self.balance = self.balance - amount
            print("Withdrawal successful. Balance:", self.balance)
        else:
            print("Insufficient balance")

    def enquiry(self):
        print("Current Balance:", self.balance)

account = bank()

account.deposit()
account.withdraw()
account.enquiry()
```

---

# Practical 5 – MetaMask + Ganache Transaction

## Steps

1. Open **Ganache**

Copy RPC server

```
http://127.0.0.1:7545
```

2. Open **MetaMask**

```
Menu → Networks → Add Custom Network
```

Configuration:

```
Network Name: Localhost 8545
RPC URL: http://127.0.0.1:7545
Chain ID: 1337
Currency: ETH
```

3. Import account

```
Account → Add Wallet → Import Account
```

4. From **Ganache**

```
Click key icon
Copy private key
```

5. Paste in MetaMask.

Account will show **100 ETH**.

6. Send transaction

```
Click Send
Paste receiver address
Enter amount
Confirm
```

Transaction appears in **Ganache**.

---

# Practical 6 – Remix Smart Contract

Open

```
https://remix.ethereum.org
```

Create file

```
tytokens.sol
```

## Solidity Code

```solidity
pragma solidity ^0.8.31;

contract tytoken {

    string public name = "tytoken";
    string public symbol = "TYIT";

    uint256 public totalsupply;

    mapping(address => uint256) public balanceOf;

    constructor(uint256 initialsupply) {

        totalsupply = initialsupply;
        balanceOf[msg.sender] = initialsupply;

    }

    function transfer(address toWhom, uint256 value)
    public returns(bool success){

        require(balanceOf[msg.sender] >= value, "insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[toWhom] += value;

        return true;
    }
}
```

## Deployment

1. Compile contract
2. Deploy using **Deploy & Run**
3. Enter initial supply
4. Test transfer between accounts

---

# Practical 8 – Real World Smart Contracts

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

---

## Donation Contract

```solidity
pragma solidity ^0.8.9;

contract Donation {

    address public owner;

    mapping(address => uint) public balances;

    event Donated(address indexed from, uint256 amount);

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

---

# Practical 9 – MetaMask + Ganache + Truffle

Create project folder

```
bkc node
```

Run commands

```bash
npm init -y
npm install -g truffle
npm install truffle lite-server
npx truffle init
```

Create contract

```
contracts/Add.sol
```

```solidity
pragma solidity ^0.8.0;

contract Add {

    function add(uint a, uint b)
    public pure returns(uint){

        return a + b;

    }
}
```

Migration file

```
migrations/1_deploy.js
```

```javascript
const Add = artifacts.require("Add");

module.exports = function (deployer) {
  deployer.deploy(Add);
};
```

Compile and deploy

```bash
npx truffle compile --all
npx truffle migrate --reset
```

---

# Practical 10 – DApp Frontend (Web3.js)

Install dependency

```bash
npm install web3
```

Create

```
index.html
```

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

            { internalType: "uint256", name: "b", type: "uint256" },
          ],

          name: "add",

          outputs: [{ internalType: "uint256", name: "", type: "uint256" }],

          stateMutability: "pure",

          type: "function",
        },
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

Replace

```
PASTE_DEPLOYED_CONTRACT_ADDRESS
```

with the address from:

```
truffle migrate --reset
```

Run:

```
lite-server
```

Open:

```
http://localhost:3000
```

---
