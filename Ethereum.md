# Ethereum

## [What is a Smart Contract?](https://ethereum.org/en/developers/docs/smart-contracts/)

> A "Smart Contract" is simply a program that runs on the Ethereum Blockchain. It's a collection of code (its functions) and data (its state) that resides at a specific address on the Ethereum blockchain.

---

## [Air Factories 2.0 - Smart Contract](https://github.com/Air-Factories-2-0/af2-smart-contract)

This section contains the description of the main functionalities of the Air Factories 2.0 Smart Contract.

<img title="" src="/IMG/ETHEREUM/1.jpg" alt="" data-align="center">

---

### User Registration & On-boarding

<img src="/IMG/ETHEREUM/2.jpg" title="" alt="" data-align="left">

#### Registration

In order to register on Air Factories 2.0, the user simply have to visit the website and register himself into it; to do this, the user must use MetaMask and has to be connected with his Ethereum Account.


In the registration page, the user will specify the type of the account he would like to create between **Air Maker** and **Air Caller**; these data, together with the account address and other information, will be saved in the Blockchain.


Every time the user starts a transaction that will write data in the Ethereum Blockchain, he must pay the GAS that is the execution cost in the network; in this case, for instance, the user will pay the registration in Air Factories 2.0 with Ethereum. 

The user to login into the website just needs to go in the homepage while he is connected in MetaMask with the address he used for the registration. The system will automaticaly detect if the account address exists on the smart contract and will show him the main page. 


#### Printer On-boarding

If the type of the account is **Air Maker** the use will be able to add new printers into the Air Factories 2.0 environment. 


The user have to go into the specific page and inserts the information about his printer like: type, brand, available nozzles, available materials, food safety and so on.

Just like the registration procedure, even in this case, data will be saved in the Blockchain and associated with the user address; so the user must pay the GAS. 

---

### Proposing Desing

In this section is described how a new desing is added into the Air Factories 2.0 environment in order to be accepted for the ordering phase.

In the ordering phase, if the desing is accepted, the users could request the **Air Makers** to print a certain number of this desing.  

In order to propose a new desing the process goes through 3 different steps:

- Proposal

- Voting System - Phase 1 

- Voting System - Phase 2

![](/IMG/ETHEREUM/3.jpg)

#### Proposal

In this step, a user insert into the platform an STL of the desing he would like to propose to the Air Factories 2.0 community.

![](/IMG/ETHEREUM/4.jpg)

##### 1 - Insert the STL in the Proposal Page

First of all, the **Air Caller** (or **Air Maker**) goes into the Proposal Page where he can upload the STL file of the design and specify other informations about it (e.g., a description of the model and what it should be used for).

##### 2 & 3 - Load STL into IPFS

At this point, the front-end will upload the STL file into IPFS and will retrive its HASH.

##### 4 - Send HASH and STL Info

Then, the front-end will sent the HASH and the informations, mentioned above, to the Blockchain in which these will be stored.

Now, every user that want to see the design could simply get the hash in order to download it.

### Voting System

In the following step the **Air Makers** are the only ones that can vote.

The voting system is based on the **Makers**' reputation.

#### Voting System - Phase 1

In the first voting phase, the **Air Makers** will express their opinion about ethical and utilities criteria, for instance:

- **Ethical Criteria:** 
  
  - Air Makers will vote the STL design considering its ethical aspects.

- **Utilities Criteria:** 
  
  - Air Makers will vote the STL design considering its utility. Some designs, such as the calibration cube, are less likely to be accepted because no one is interested in      purchasing a batch of this type of item.

#### Voting System - Phase 2

In the second voting phase, the **Air Makers** will test the STL technical criteria.

In order to vote the design, the **Air Maker** *have to print at least one time the STL that he want to vote*

- **Feasibility Criteria:** 
  
  - Air Maker will vote if the STL design can be printed without any obvious feasibility problems.

- **Quality Criteria:**
  
  - Air Maker will vote if the STL design, once printed, actually has the features that the Air Maker expected from that specific design.

#### Voting System - Algorithm

---

### Scheduling

In this section is showed the policy followed by the Scheduling Algorithm implemented in Air Factories 2.0. 

Before going into the actual phase of the algorithm lets take a look at the data that are used to select the Air Makers that will print the design order, as shown in the following figure: 

![](/IMG/ETHEREUM/5.jpg)

In the same image are specified the Filters hierarchy that are used in the first scheduling phase.

##### Phase 1 - Filtering Makers

The first filters that are used simply check if a criteria is accomplished or not: if one of them is not satisfied then the **Air Maker's Printer** is rejected by the algorithm.

Then subsequent filter will select the **Air Printer** according to the available nozzles and the distance from the **Air Caller** that made the order. 

In particular, thanks to these filter, it's possible to check:

- If the quality related to the nozzle diameter satisfy the requested one from the **Air Caller**:
  
  - If the **Air Caller** accepts to have an higher quality, and a consequent higher cost, then are selected **Air Printers** that have smaller diamater nozzles as well.

- If there are **Air Printers** nearby the **Air Caller** that made the order:
  
  - If not, then the range of the **Air Printer**' geographic location is increased until it reaches a limit. 
  
  - After 5 increments no more **Air Printers** are selected.

![](/IMG/ETHEREUM/6.jpg)

##### Phase 2 - Prioritizing Makers

In this phase the algorithm will create a priority list of the **Air Printers** selected in the previously phase.

The algorithm will give a vote on each **Air Printer** based on the following assumptions: 

![](/IMG/ETHEREUM/7.jpg)

**Air Makers**' reputation is important on the creation of the priority list because, in this way, there is an higher probability that the choosen **Air Makers** will be more reliable.

As is wanted an high efficiency an higher score is given to a not busy **Air Printer**; the same concept is applied regard the ecosustainability: **Air Printer** closer to the user that made the order will have a score value between 0 and 2 given by the following formula.

> Let's consider a Map containing the Air Maker as a key and the distance from the Air Caller as a value.
> 
> Using the following formula, a value between 0 and 1 is assigned to each distance:
> 
> > **X' = ( x - min(X) ) / ( max(X) - min(X) )**
> 
> *0 will be associated with the furthest maker*
> 
> Now the values are considered inversely proportional to the distance:
> 
> > **X'' = 1 - x'**
> 
> All makers will now have an associated value between 0 and 1 where:
> 
> **1 = Very close to the Air Caller**
> 
> **0 = Far away from the Air Caller**
> 
> This value will be multiplied by 2 in order to have a greater impact on the priority.

##### Pieces Distribution

In relation to the previously created priority list, in the last phase there will be the pieces distribution, considering:

- N 
  
  - The number of selected **Air Makers** 

- M
  
  - The number of pieces requested with the order 

- X
  
  - The maximum number of pieces that each **Air Maker** can print in a given time 

<img src="/IMG/ETHEREUM/8.jpg" title="" alt="" data-align="inline">

##### Scheduling Algorithm

```python
def piecesDistribution(printers, npieces, maxPrintablePieces):
    '''
        @input printers = dict -> { printer : priority }
        @input npieces = int -> number of pieces to be printed
        @input maxPrintablePieces = int -> max number of pieces that can be printed by
                                           each printer in a given time
        @return distribution = dict -> { maker : pieces to  }
    '''
    distribution = {}
    #If there is only 1 piece to be printed then we assing it to the first 
    #printer in the list
  if npieces==1:
   firstPrinter = max(printers, key=printers.get)
   return { firstPrinter : 1 }

    #If the number of pieces is less then the printers in the list then we use
    #norm min-max in order to remove the printers with less priority
    if npieces < len(printers):
        minValue = min(list(printers.values()))
        for key, value in printers.items():
            printers[key] = (value - minValue ) / (10 - minValue)

    #Simple direct distribution
    sumPriority = sum(list(printers.values()))
    np = len(printers) #number of printers
    remainingPieces = npieces 
    for key, value in printers.items():
        assingPieces = round((npieces * value) / sumPriority)
  #If assingPieces > maxPrintablePieces then assingPieces = maxPrintablePieces
        if assingPieces > maxPrintablePieces:
           assingPieces = maxPrintablePieces
  #if remainingPieces = 0 or assingPieces > remainingPieces then assing to the printer
  #only the remainingPieces
        if remainingPieces <= 0 or assingPieces > remainingPieces:
            distribution[key] = remainingPieces
            break
        distribution[key] = assingPieces
        remainingPieces -= assingPieces

    #If after this process there are still remainingPieces these will be assing to the 
    #first printer in the priority list
    if remainingPieces > 0:
        firstPrinter = max(printers, key=printers.get)
        distribution[firstPrinter] += remainingPieces
    return distribution
```

---

### Token
In general, everything running on the Blockchain is paid for; in particular, there are tokens running on the Blockchain, which are currencies, and which are used to interact with the corresponding applications.
In the Air Factories 2.0 project, a coin (represented by the ERC-20 token) was implemented, which was called the Air Factories Token (AFT), with which users can interact with the platform.
Users pay to buy tokens, and thanks to these they can interact with the infrastructure: users who want to join the platform must buy a certain number of tokens and pay on-boarding for each printer they intend to connect.
The Air Caller, when making an Air Call, must pay a certain number of AFT based on what it orders, and then those tokens are turned from the platform to the Air Makers who participated in the production.
The Air Makers are paid considering the time spent, the electricity spent in the printing of the product, the material used and the wear of the printer.
Even to submit a new product you have to pay, with a certain number of tokens, and then, the Air Makers who participate in the vote, and that will be valid that product, are paid in the form of those tokens.

---

### Oracle

Blockchain oracles are third-party services that provide information external to smart contracts. They act as bridges between blockchains and the outside world. Blockchains and smart contracts cannot access off-chain data. A Blockchain oracle is not the data source itself, but rather the level that requires, verifies, and authenticates external data sources to then transmit the information. To request data from the outside world, the smart contract must be called up and must be spent network resources. Some oracles have the ability to not only transmit information to smart contracts, but also to send it back to external sources.

Software oracles interact with online information sources and transmit data to the Blockchain; this information can come from databases, servers, online websites - essentially, any data source on the Web. A centralized oracle is controlled by a single entity and is the sole provider of information for a smart contract. Using only one source of information can be risky - the effectiveness of the contract depends entirely on the entity controlling the oracle. The fundamental problem of centralized oracles is the existence of a single point of error, which makes contracts less resistant to vulnerabilities and attacks.
