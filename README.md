# Air Factories 2.0 - Design

## What is Air Factories 2.0

Air Factories 2.0 is a project funded by the Ministry of University and Research under the FISR 2020 COVID notice from the Special Supplementary Research Fund.

The final aim of the Air Factories 2.0 project is the implementation of an intelligent, decentralized, sustainable and resilient production chain that allows the rapid prototyping of new projects and guarantees the certifiability of the parts produced.

## Project Introduction

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/1.jpg)

## User

- Air Caller: Are those who require the production of a certain number of finished pieces by the distributed factory, they can also develop new projects to be produced in the distributed factory and use it for the prototyping of the first pieces;

- Air Makers: they can act as validators of the projects in the prototyping phase of the product, as producers of finished pieces and as an Air Caller;

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/2.jpg)

## Front-End

Is a website made using React. It can interact with the Ethereum SmartContract using the web3 libraries. 

## Distributed Back-End

The Distributed Back-End è realizzato facendo ricorso a smartcontract di Ethereum e Chaincode per blockchain creata tramite Hyperledger Fabric

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/3.jpg)

### [Hyperledger](Hyperledger.md)

Il ChainCode per Hyperledger si occuperà del controllo qualità sulla stampa di un pezzo richiesto ad un Maker.

### [Ethereum](Ethereum.md)

Lo SmartContract di Air Factories 2.0 gestisce:

1. L'iscrizione degli utenti alla piattaforma, compresa la registrazione di nuove stampanti 3D nella fabbrica

2. Il token di Air Factories 2.0

3. Il sistema di voting dei prototipi

4. L'algoritmo di scheduling per la ripartizione dei pezzi di un ordine fra i Maker della piattaforma

5. Parte del processo di Proof of Print, tramite il quale ci si assicura che un Maker abbia eseguito la stampa di un pezzo e che questa sia valida
   
   - Affinche questo sia possibile è stato implementato un Oracolo per Ethereum in modo che possa contattare la blockchain creata in Hyperledger Fabric.

## [Print Manager](Printer Manager.md)

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/4.jpg)

Il Printer Manager è composto da una parte Software chiamata Printer Controller e una parte Hardware chiamata Printer HUB.

### Printer HUB

Il Printer HUB è composto da una RaspberryPI 3 Modello A sul quale sarà caricata la logica del Printer Controller e una Raspi Camera v2 utilizzata per effettuare le foto durante il processo di stampa.

### Printer Controller

Il Printer Controller è la logica del nostro Printer Manager, esso contiene un Server scritto in Flask tramite il quale l'utente potrà dare il vio alla stampa al momento del ricevimento di un ordine da parte di un Caller, Octoprint per poter comunicare con la stampante 3D e IPFS.

Il server sarà in grado di comunicare con lo SmartContract di Air Factories tramite le librerie Web3 e contattare la blockchain creata con Hyperledger Fabric tramite delle chiamate REST.

### Proof of Print (PoP)

La Proof of Print è il processo tramite il quale Air Factories 2.0 controlla che un Maker abbia effettivamente stampato un modello richiesto da un Caller e che la qualità di quest'ultimo soddisfi quella richiesta.

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/5.jpg)

Il processo mette in comunicazioni tutte le componenti del sistema Air Factories 2.0, il Front-End, lo SmartContract sulla Blockchain Ethereum, il ChainCode su blockchain Hyperledger e il Printer Manager e comprende le seguenti fasi:

#### Fase 0 - Ordine

Per far sì che possa iniziare 

## 
