# [Air Factories 2.0](https://air-factories-2-0.github.io/af2-website/public/) - Design

## What is Air Factories 2.0

Air Factories 2.0 is a project funded by the Ministry of University and Research under the FISR 2020 COVID notice from the Special Supplementary Research Fund.

The final aim of the Air Factories 2.0 project is the implementation of an intelligent, decentralized, sustainable and resilient production chain that allows the rapid prototyping of new projects and guarantees the certifiability of the parts produced.

## Project Goals

The objective of this project was to implement methodologies, techniques, algorithms, hardware and software systems to build a decentralized factory composed of 3D printers, freely made available by users distributed throughout the territory, and to finalize working prototypes relating to the software platform and hardware components.

## Project Introduction

The software platform created was called Air Factories 2.0, it is decentralized and based on Blockchain technologies for the management of the factory life cycle, from the prototyping of new products to mass production.

![](/IMG/README/1.jpg)
=======

The structure of Air Factories 2.0 includes:

- **Front-end**
  
  - Graphical interface for end users divided into two categories **Air Caller** and **Air Maker**
  
  - Developed using React-Native Web

- **Distributed Back-end**
  
  - **Hyperldeger**
    
    - It deals with the algorithm for quality verification and validation of the print
  
  - **Ethereum**
    
    - It contains all the logic of the Air Factories 2.0 platform starting from the user's registration, passing from the prototyping of the designs proposed by the **Air Callers**, to the print order scheduling algorithm up to the management of the printing process
    
    - Web3 libraries are used in order to allow communication between the browser and the Ethereum blockchain

- **Printer Manager**
  
  - **Printer Hub**
    
    - Hardware part of the Printer Manager consisting of a Raspberry PI 3 Model A and the Raspi Camera V2
  
  - **Printer Controller**
    
    - Software part of the Pringer Manager, contains the server that will be contacted by the Maker to start the printing process that starts the *Proof of Print* (PoP)

## User

![](/IMG/README/2.jpg)

- ### Air Caller
  
  - Are those who require the production of a certain number of finished pieces by the distributed factory, they can also develop new projects to be produced in the distributed factory and use it for the prototyping of the first pieces;

- ### Air Makers
  
  - they can act as validators of the projects in the prototyping phase of the product, as producers of finished pieces and as an Air Caller;

## [Front-End](https://github.com/Air-Factories-2-0/af2-frontend)

Is a website made using React-native web. It can interact with the Ethereum SmartContract using the web3 libraries. 

## Distributed Back-End

The Air Factories 2.0 software platform is totally autonomous and decentralized and replaces the Air Admins.

The term "decentralized" is understood in the typical sense of Blockchain technologies, therefore, being the Air Factories 2.0 platform running on the Blockchain, it is totally autonomous and not controlled by human agents.

In particular, the concept of Smart Contract has been exploited, that is a software program running on the Blockchain, whose execution no one can make changes, and which always behaves in the same way, and which, therefore, everyone can trust. .

The Distributed Back-End is made using the Smart Contract of Ethereum and Chaincode for Blockchain created through Hyperledger Fabric.

The term Chaincode is comparable to Smart Contract in Hyperledger blockchain.

![](/IMG/README/3.jpg)

### [Hyperledger](Hyperledger.md)

The ChainCode for Hyperledger will take care of the quality control on the printing of a piece requested from a Maker.

### [Ethereum](Ethereum.md)

The Air Factories 2.0 Smart Contract manages:

1. The subscription of users to the platform, including the registration of new 3D printers in the factory

2. The Air Factories 2.0 token

3. The voting algorithm for prototyping

4. The scheduling algorithm for dividing the pieces of an order among the Makers of the platform

5. Part of the Proof of Print process, through which we make sure that a Maker has printed a piece and that it is valid
   
   - For this to be possible, an Oracle for Ethereum has been implemented so that it can contact the blockchain created in Hyperledger Fabric.

## [Print Manager](PrinterManager.md)

The Printer Manager is composed of a Software part called Printer Controller and a Hardware part called Printer HUB.

![](/IMG/README/4.jpg)

### Printer HUB

The Printer HUB is composed of a RaspberryPI 3 Model A on which the Printer Controller logic will be loaded and a Raspi Camera v2 used to take photos during the printing process.

### Printer Controller

The Printer Controller is the logic of our Printer Manager, it contains a Server written in Flask through which the user can give the green light to the print upon receipt of an order by a Caller, Octoprint to be able to communicate with the printer 3D and IPFS.

The server will be able to communicate with the Air Factories SmartContract through the Web3 libraries and contact the blockchain created with Hyperledger Fabric through REST calls.

In order to communicate remotely with the printer Firmware, the Controller uses OctoPrint and the Python octorest library.

### Proof of Print (PoP)

Proof of Print is the process by which Air Factories 2.0 checks that a Maker has actually printed a model requested by a Caller and that the quality of the latter meets that request.

![](/IMG/README/5.jpg)

The process connects all the components of the Air Factories 2.0 system: the Front-End, the SmartContract on the Ethereum Blockchain, the Chaincode on the Hyperledger Blockchain and the Printer Manage.

The *Proof of Print* includes the following steps:

#### Phase 0 - Order

In order for the Proof of Print to begin we have a phase 0 in which a caller places an order by specifying the number of pieces and the quality of the print (which will decide the slicing profile to use).
After the scheduling phase, the Makers who will print the requested design will be selected, and the notification to start printing will then arrive in the front-end of these.
Makers not currently available for printing will be discarded from the scheduling phase.

The Caller will make a preliminary payment by transferring the tokens required for printing to the Air Factories 2.0 SmartContract address.

#### Phase 1 - Start Printing

Once the notification has appeared on the front-end of the Maker, it will be able to start the automatic printing phase.
#### Phase 2 - Retrive STL and Slicing Profile, Start Printing

##### 2.1 - Retrive HASHES: STL and Slicing Profile

First of all, starting from the requested order, the HASH of the STL and the HASH of the slicing profile for the printer that is used are retrieved.

###### 2.2 - Download STL and Slicing Profile

Through the HASHES, the STL of the requested design and the profile of the slicing necessary to create the GCODE are downloaded from IPFS.

It is not shown in the diagram, but basically the GCODE is created starting from the STL and the profile just downloaded, the GCODE is subsequently loaded on IPFS, it will be used later in the Validation phase.

##### 2.3 - Timestamp: Start Printing

The print start timestamp of the design is saved on the Air Factories 2.0 smartcontract.

#### Fase 3 - Data Collection

Together with phase 4, phase 3 is repeated until the printer has finished making the piece.

##### 3.1 - Start Printing

The Controller through octorest communicates with OctoPrint which, by contacting the printer FW, starts the printing process and starts the timelapse service.

###### 3.2 - Timelapse

Photos of the printing process are made every N Layer printed. These are saved on the controller.

##### 3.3 - Upload snapshot on IPFS

Each photo taken during the printing process is saved on IPFS and the HASH of the latter is saved on the controller.

##### 3.4 - Sending snapshots' hashes to Hyperledger

The hash of the photo just saved on IPFS is sent (together with the GCODE HASH) to Hyperledger via a REST call.

##### Warning

If during the printing phase the process should be interrupted due to any error, the end of printing timestamp will be sent to the Ethereum blockchain considering the printing failed.

#### Phase 4 - Printing status check and validation

##### 4.1 - Retrive snapshot and GCODE

Through the received HASH, the GCODE created by the controller and the photo to the N layer of the design printing process are downloaded.
##### 4.2 - Printing quality validation

Through the algorithm explained in more detail in the section of [Hyperledger] (Hyperledger.md) it is verified that the printing process at the N layer is satisfying the required quality standards. The result of the verification at the aforementioned layer is saved on the Hyperledger blockchain.

#### Phase 5 - End Printing

##### 5.1 - End printing: Single piece

The printer FW will set its status to "Operational" to indicate that the printing is finished (In case of error / interruption of the printing the status would be "Canceling").

##### 5.2 - Send end printing timestamp

The controller via OctoPrint will verify that the printing has finished and will send the timepstamp to the Air Factories 2.0 Smart Contract to indicate that the printing procedure has been successful and has not been interrupted.

##### Warning

Before proceeding with phase 5.3 that is the Reward Claim, the previous phases must be completed for EVERY piece of the requested design.

##### 5.3 - Claim Reward

Once all the pieces ordered by the Caller have finished printing, the user can request to receive the tokens required for printing.

A call will therefore be made to the Smart Contract method to request payment.
##### 5.4 - Ethereum Oracle: Retrive validation result

In order to retrieve the information contained on the Hyperledger Blockchain, Ethereum must necessarily go through an Oracle. The Oracle will make a GET call on the REST Server connected to Hyperledger which will retrieve the data necessary for Ethereum to decide whether to pay the Maker or not.

##### 5.5 - Retrive validation result from Hyperledger

For each printed piece, on the Hyperledger blockchain there are the results of the print quality check carried out on each layer, therefore a percentage of print quality is associated with each piece: if all the pieces have a value greater than 70/80% (TO BE DECIDED) then the order is considered completed successfully.

##### 5.6 - Paying Maker

Once the printing result has been retrieved from Hyperledger, the Smart Contract will decide whether to pay the Maker who made the printing or reimburse the Caller.

## 
