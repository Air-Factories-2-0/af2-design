# Ethereum

## [What is a Smart Contract?](https://ethereum.org/en/developers/docs/smart-contracts/)

> A "smart contract" is simply a program that runs on the Ethereum blockchain. It's a collection of code (its functions) and data (its state) that resides at a specific address on the Ethereum blockchain.

---

## [Air Factories 2.0 - Smart Contract](https://github.com/Air-Factories-2-0/af2-smart-contract)

This section contain the description of the main functionality of the Air Factories 2.0 Smart Contract.

<img title="" src="/IMG/ETHEREUM/1.jpg" alt="" data-align="center">

---

### User Registration & On-boarding

<img src="/IMG/ETHEREUM/2.jpg" title="" alt="" data-align="left">

#### Registration

In order to register on Air Factories 2.0, the user simply have to visit the website and register himself into it. To do this, the user must use Metamask and has to be connected with his Ethereum Account.

[SCREENSHOT HOMEPAGE ESEMPIO COLLEGAMENTO CON METAMASK]()

In the registration page, the user will specify the type of the account he would like to create between **Air Maker** and **Air Caller**. These data, together with the account address and other information, will be saved in the blockchain.

[SCREENSHOT REGISTRATION PAGE]()

Every time the user starts a transaction that will write data in the Ethereum blockchain, he must pay the GAS that is the execution cost in the network; in this case, for instance, the user will pay the registration in Air Factories 2.0 with Ethereum. 

The user to login into the website just needs to go in the homepage while he is connected in MetaMask with the address he used for the registration. The system will automaticaly detect if the account address exists on the smart contract and will show him the main page. 

[SCREENSHOT MAIN PAGE]()

#### Printer On-boarding

If the type of the account is **Air Maker** the use will be able to add new printers into the Air Factories 2.0 environment. 

[SCREENSHOT ONBOARDING PAGE]()

The user have to go into the specific page and inserts the information about his printer like: type, brand, available nozzles, available materials, food safety and so on.

Just like the registration procedure, even in this case these data will be saved in the blockchain and associated with the user address; so the user must pay the GAS. 

---

### Proposing Desing

In this section is described how a new desing is added into the Air Factories 2.0 environment in order to be accepted for the ordering phase.

In the ordering phase, if the desing is accepted, the users could request the **Air Makers** to print a certain number of this desing.  

In order to propose a new desing the process  go through 3 different steps:

- Proposal

- Voting System - Phase 1 

- Voting System - Phase 2

![](/IMG/ETHEREUM/3.jpg)

#### Proposal

In this step, a user insert into the platform an STL of the desing he would like to propose to the community.

![](/IMG/ETHEREUM/4.jpg)

##### 1 - Insert the STL in the Proposal Page

The **Air Caller** (or **Air Maker**) go into the Proposal Page where he can upload the STL file of the design and specify other informations about it (e.g., a description of the model and what it should be used for).

##### 2 & 3 - Load STL into IPFS

At this point the front-end will upload the STL file into IPFS and will retrive his HASH.

##### 4 - Send HASH and STL Info

The front-end will sent the HASH and the informations mentioned above to the blockchain in which these will be stored.

Now every user that want to see the design could simply get the hash in order to download it.

### Voting System

In the following step the **Air Makers** are the only ones that can vote.

The voting system is based on the reputation of the **Makers**.

#### Voting System - Phase 1

In the first phase of the voting system the **Air Makers** will express their opinion about  ethical and utilities criteria, for instance:

- **Ethical Criteria:** 
  
  - Makers will vote the STL design considering its ethical aspects.

- **Utilities Criteria:** 
  
  - Maker will vote the STL design considering its utility
  
  - Design like calibration cube are more less likely to be accepted because no one is interred on buying a batch of this kind of object.

#### Voting System - Phase 2

In the second phase the **Air Makers** will test the STL technical criteria.

In order to vote the design, the **Maker** *have to print at least one time the STL that he want to vote*

- **Feasibility Criteria:** 
  
  - Maker will vote if the STL design can be printed without any problems.

- **Quality Criteria:**
  
  - Maker will vote if the STL design once printed actually has the features that the Maker expected from that specific design.

#### Voting System - Algorithm

---

### Scheduling

In this section is showed the policy followed by the Scheduling Algorithm implemented in Air Factories 2.0. 

Before going into the actual phase of the algorithm lets take a look at the data that are used to select the Makers that will print the design order, showed in the following figure: 

![](/IMG/ETHEREUM/5.jpg)

In the same image are specified the Filters hierarchy that are used in the phase 1.

##### Phase 1 - Filtering Makers

The first filters that are used simply check if a criteria is accomplished or not, if one of them is not satisfied then the **Air Maker's Printer** is rejected by the algorithm.

Then subsequent filter will select the **Printer** according to the available nozzles and the distance from the **Air Caller** that made the order. 

In particular, thanks to these filter is possible to check:

- If the quality related to the nozzle diameter satisfy the requested once from the **Caller**
  
  - If the **Caller** accept to have an higher quality and consequent higher price then are selected **Printers** that have smaller diamater nozzles as well.

- If there are **Printers** nearby the **Caller** that made the order
  
  - If not, then the range of the geographic location is increased until it reaches a limit. 
  
  - After 5 increments no more **Printers** are selected.

![](/IMG/ETHEREUM/6.jpg)

##### Phase 2 - Prioritizing Makers

In this phase the algorithm will create a priority list of the **Printers** selected in the previously phase.

The algorithm will give a vote on each **Printers** based on the following assumptions: 

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/ETHEREUM/7.jpg)

Makers' reputation is important on the creation of the priority list because in this way there is an higher probability that the choosen **Makers** will be more reliable.

As is wanted an high efficiency an higher score is given to a not busy **Printer**; the same concept is applied regard the ecosustainability: **Printer** closer to the user that made the order will have a value between 0 and 2 given by the following formula: 

> Let's consider a Map containing the Maker as a key and the distance from the Caller as a value:
> 
> Using the following formula, a value between 0 and 1 is assigned to each distance
> 
> > **X' = ( x - min(X) ) / ( max(X) - min(X) )**
> 
> *0 will be associated with the furthest maker*
> 
> Now let's make our values inversely proportional to distance:
> 
> > **X'' = 1 - x'**
> 
> All makers will now have an associated value between 0 and 1 where:
> 
> **1 = Very close to the Caller**
> 
> **0 = Far away from the Caller**
> 
> This value will be multiplied by 2 in order to have greater impact on the priority

##### Pieces Distribution

In relation to the previously created priority list, in the last phase there will be the pieces distribution, considering:

- N 
  - The number of selected **Makers** 

- M
  
  - The number of pieces requested on the order 

- X
  
  - The maximum number of pieces that each **Maker** can print in a given time 

<img src="file:///Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/ETHEREUM/8.jpg" title="" alt="" data-align="inline">

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

---

### Oracle
