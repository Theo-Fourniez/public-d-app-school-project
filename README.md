# Public - A web3 AI secure investment platform - {ReactJs, Solidity, Flask}
Groupwork of : SALON Tom, FOURNIEZ Théo, GILLOT Quentin  
Demo video at the end of this file.

## Project description
We have developed a decentralized crowdfunding application enabling users of legal age (who pass the AI automatic identity verification) to have their projects financed by selling shares in their companies in the form of tokens.
The application contains 2 main functionalities: 
- Creation of a project and its token
- The possibility of investing in the projects of other creators via the purchase of tokens.

## Work distribution
We've divided up the work, each acting on one of the project's main building blocks : 
* SALON Tom : Frontend
* FOURNIEZ Théo : Smart Contract
* GILLOT Quentin : AI Server for ID recognition

### Frontend
Tom took care of the frontend, designing all the mock-ups beforehand. 
He then built the entire user interface and made the link with the smart contract, calling the various functions enabling the application to function:  
Connecting to the application by calling Metamask: 
![image](https://github.com/SalonTom/public-d-app/assets/119957865/13c840c4-8cec-4f26-9524-31af86458059)

Identity verification page :

![Access request](image-3.png)

Page listing all available projects:

![Feed page](image.png)

Project creation page : 

![Project Creation](image-1.png)

Project investment : 

![Investment](image-2.png)


### Smart Contract
I (Théo) was in charge of the entire design and implementation of the smart contracts: 
- ProjectToken: this smart contract inheriting from ERC20 allows the mint of a token representing the company's shares. The total amount of existing tokens is always 100 * 10¹⁸ to represent 100% of the shares. 
- ProjectTokenFactory: this smart contract is used to create projects and the related token. It is also used to list all projects, and is called by the identity verification brick (Quentin's part) to whitelist users according to their age.
- ProjectTokenMarket: allows tokens to be added to the marketplace. In order to list a token on the market, the user must first authorize this contract to spend the right number of ProjectToken on his behalf (thanks to the ERC20 mechanism of [allowance](https://docs.openzeppelin.com/contracts/5.x/api/token/erc20#IERC20-allowance-address-address-)). The buyer then uses the purchaseToken function to buy a certain number of listed tokens.

### AI server for ID recognition
Quentin designed the python server used to validate the majority of users:
- a REST endpoint is exposed on the server, which receives an image as input
- the image is passed to an AI, which uses several mechanisms (rotation, greyscale, etc.) to retrieve all the text recognized on the image
- a set of rules (regex) are defined to retrieve only the data we're interested in here, i.e. date of birth and identity card number - the server acts in the background, thanks to thread parallelization, enabling several users to call the endpoint at the same time, without blocking the user's action on the frontend.
- the identity card is completely deleted by the server after AI processing.

## Demo video link
* [Vidéo de démonstration](https://drive.google.com/file/d/1IOIS0xdT2hrUI0VBE3dRWXQVFubkjSxl/view?usp=drive_link)
