1) run this command under main folder
    > npx create-next-app@latest --typescript --example with-tailwindcss
    app to be named as : <give a app name>  (creates next.js folder wwith tailwind inilitized)

2) check next.js app
    > cd dapp
    > npm install ethers
    > npm run dev

3) create folder contract under main folder
    run this under contract folder
    > npm init -y   (creates packgae .json file)

4) installing hardhat under contract folder
    > npm install --save-dev hardhat
    > npx hardhat
    > select "creata a javascript project"

5) installing hardhat plugins
    > npm install --save-dev "hardhat@^2.10.1" "@nomicfoundation/hardhat-toolbox@^1.0.1"
  
6) compile the contract to check
    > npx hardhat compile

7) Test the contract 
    > npx hardhat test

8) TO run local hardhat blockchain
    >npx hardhat node

9)(optional) importing account om metamask
    > copy a private key from hardhat local blockchain
    > login to metamask & change network to Localhost 8545
    > go to import account and paste the private key
    > account with 10000 ETH is imported

10) > npm install dotenv
     create a file named .env under contract folder & paste the below stuff
        NEXT_PUBLIC_RPC_URL=https://polygon-mumbai.infura.io/v3/c2e1daa89201497cafd93f1dd04e3387
        NEXT_PUBLIC_PRIVATE_KEY=892c29f33b38801949ea2e967e5ba6f9f6735f1d81a1c3497c897853dd3183c7
        NEXT_PUBLIC_ADDRESS=0x85D8662523CB62464794Ea681d312bEBe2b1691b  

11) open hardhat.config.js  & paste the following

        // library for writing and testing smart contracts
        require("@nomicfoundation/hardhat-toolbox");

        // dotenv Loads environment variables from .env file
        require('dotenv').config({path:'./.env'});


        task("accounts","Prints list of accounts",async(taskArgs,hre)=>{
        //return list of account addresses
        //if defaultNetwork=polygon,then accounts defined under polygon are returned
        const [signerAccounts]=await hre.ethers.getSigners(); 
        console.log("Account Address:",signerAccounts.address);//returns address of the account
        })
        // getting environment variables from .env.local file
        const privateKey=process.env.NEXT_PUBLIC_PRIVATE_KEY // Private key of metamask acount
        const RPC_URL=process.env.NEXT_PUBLIC_RPC_URL //Endpoint URL from Infura (polygon mumbai) helps connect dapps to blockchain

        module.exports = {
        solidity: "0.8.9",
        defaultNetwork:"polygon",//defaultnet can be hardhat or polygon
        networks:{
            hardhat:{},
            polygon:{
            url:RPC_URL,
            accounts:[privateKey]
            }
        }
        };

12) write the Smart Contract using Remix IDE & paste under contract/contracts folder
    > run npx hardhat compile
    
13) go to scripts/deploy. js and paste the following
    -change SC name to the one which u want to deploy
    const hre=require('hardhat');

async function main(){
            //return list of account addresses
            
        //if defaultNetwork=polygon,then accounts defined under polygon are returned
            const [deployer] = await ethers.getSigners(); 

            console.log("Deploying contracts with the account:", deployer.address);
        
            console.log("Account balance:", (await deployer.getBalance()).toString());

            // .getContractFactory("<contract name>") function of ethers.js, used to access contracts in the artifacts folder
            const User_Details= await hre.ethers.getContractFactory("User_Details")
            const user_Details= await User_Details.deploy();

            await user_Details.deployed();

            console.log("Deployed Contract Address: ",user_Details.address);
        }

        main()
            .then(() => process.exit(0) )
            .catch((error) =>{
                console.log(error)
                process.exit(1);
            })

14)run > npx hardhat run scripts/deploy.js
// check deployed contract using
//https://mumbai.polygonscan.com/address/0xc5d009C7F6e6256d2294E0Ad3F208046aB96a036

15) copy deployed contract address to .env 