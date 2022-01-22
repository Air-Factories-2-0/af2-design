# [Air Factories 2.0](https://air-factories-2-0.github.io/af2-website/public/) - Design

## What is Air Factories 2.0

Air Factories 2.0 is a project funded by the Ministry of University and Research under the FISR 2020 COVID notice from the Special Supplementary Research Fund.

The final aim of the Air Factories 2.0 project is the implementation of an intelligent, decentralized, sustainable and resilient production chain that allows the rapid prototyping of new projects and guarantees the certifiability of the parts produced.

## Project Goals

L’obiettivo di questo progetto è stato quello di implementare metodologie, tecniche, algoritmi, sistemi hardware e software per costruire una Fabbrica Decentralizzata composta da stampanti 3D, liberamente messe a disposizione da utenti distribuiti sul territorio, e finalizzare dei prototipi funzionanti relativi alla piattaforma software ed ai componenti hardware.

## Project Introduction

La piattaforma software realizzata è stata denominata Air Factories 2.0, essa è decentralizzata e basata su tecnologie Blockchain per la gestione del ciclo di vita della fabbrica, dalla prototipazione di nuovi prodotti alla produzione di massa.

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/1.jpg)
=======

La struttura di Air Factories 2.0 comprende

- **Front-end**
  
  - Interfaccia grafica per gli utenti finali divisi in due categorie **Air Caller** e **Air Maker**
  
  - Sviluppata utilizzando react-native web

- **Back-end Distribuito**
  
  - **Hyperldeger**
    
    - Si occupa dell'algoritmo per la verifica della qualità e validazione della stampa
  
  - **Ethereum**
    
    - Contiene tutta la logica della piattaforma Air Factories 2.0 a partire dall'iscrizione dell'utente, passando dalla prototipazione dei design proposti dagli **Air Caller**, all'algoritmo di scheduling degli ordini di stampa fino ad arrivare alla gestione del processo di stampa
    
    - Viene fatto uso delle librerie Web3 al fine di consentire la comunicazione tra browser e la blockchain Ethereum

- **Printer Manager**
  
  - **Printer Hub**
    
    - Parte Harwdare del Printer Manager composta da una Raspberry PI 3 Modello A e la Raspi Camera V2
  
  - **Printer Controller**
    
    - Parte Software del Pringer Manager, contiene il server che verrà contattato dal Maker per far partire il processo di stampa che da inizio alla *Proof of Print* (PoP)

## User

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/2.jpg)

- ### Air Caller
  
  - Are those who require the production of a certain number of finished pieces by the distributed factory, they can also develop new projects to be produced in the distributed factory and use it for the prototyping of the first pieces;

- ### Air Makers
  
  - they can act as validators of the projects in the prototyping phase of the product, as producers of finished pieces and as an Air Caller;

## [Front-End](https://github.com/Air-Factories-2-0/af2-frontend)

Is a website made using React-native web. It can interact with the Ethereum SmartContract using the web3 libraries. 

## Distributed Back-End

La piattaforma software Air Factories 2.0 è totalmente autonoma e decentralizzata e sostituisce gli Air Admin.

Il termine “decentralizzato” è inteso nell’accezione tipica delle tecnologie Blockchain, pertanto, essendo la piattaforma Air Factories 2.0 in esecuzione sulla Blockchain, essa è totalmente autonoma e non controllata da agenti umani.

In particolare, è stato sfruttato il concetto di Smart Contract, ovvero un programma software in esecuzione sulla Blockchain, alla cui esecuzione nessuno può apportare delle modifiche, e che si comporta sempre nello stesso modo, e di cui, dunque, tutti possono fidarsi.

The Distributed Back-End è realizzato facendo ricorso a Smart Contract di Ethereum e Chaincode per Blockchain creata tramite Hyperledger Fabric.

Il termine Chaincode è paragonabile a Smart Contract in blockchain Hyperledger.

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/3.jpg)

### [Hyperledger](Hyperledger.md)

Il ChainCode per Hyperledger si occuperà del controllo qualità sulla stampa di un pezzo richiesto ad un Maker.

### [Ethereum](Ethereum.md)

Lo SmartContract di Air Factories 2.0 gestisce:

1. L'iscrizione degli utenti alla piattaforma, compresa la registrazione di nuove stampanti 3D nella fabbrica

2. Il token di Air Factories 2.0

3. L'algoritmo di voting per la prototipazione

4. L'algoritmo di scheduling per la ripartizione dei pezzi di un ordine fra i Maker della piattaforma

5. Parte del processo di Proof of Print, tramite il quale ci si assicura che un Maker abbia eseguito la stampa di un pezzo e che questa sia valida
   
   - Affinche questo sia possibile è stato implementato un Oracolo per Ethereum in modo che possa contattare la blockchain creata in Hyperledger Fabric.

## [Print Manager](PrinterManager.md)

Il Printer Manager è composto da una parte Software chiamata Printer Controller e una parte Hardware chiamata Printer HUB.

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/4.jpg)

### Printer HUB

Il Printer HUB è composto da una RaspberryPI 3 Modello A sul quale sarà caricata la logica del Printer Controller e una Raspi Camera v2 utilizzata per effettuare le foto durante il processo di stampa.

### Printer Controller

Il Printer Controller è la logica del nostro Printer Manager, esso contiene un Server scritto in Flask tramite il quale l'utente potrà dare il vio alla stampa al momento del ricevimento di un ordine da parte di un Caller, Octoprint per poter comunicare con la stampante 3D e IPFS.

Il server sarà in grado di comunicare con lo SmartContract di Air Factories tramite le librerie Web3 e contattare la blockchain creata con Hyperledger Fabric tramite delle chiamate REST.

Per poter comunicare in remoto con il Firmware della stampante il Controller utilizza OctoPrint e la libreria octorest di python.

### Proof of Print (PoP)

La Proof of Print è il processo tramite il quale Air Factories 2.0 controlla che un Maker abbia effettivamente stampato un modello richiesto da un Caller e che la qualità di quest'ultimo soddisfi quella richiesta.

![](/Users/antoniopipitone/Desktop/Air%20Factories%202.0/af2-design/IMG/README/5.jpg)

Il processo mette in comunicazioni tutte le componenti del sistema Air Factories 2.0: il Front-End, lo SmartContract sulla Blockchain Ethereum, il Chaincode su Blockchain Hyperledger e il Printer Manage.

La *Proof of Print* comprende le seguenti fasi:

#### Fase 0 - Ordine

Per far sì che possa iniziare la Proof of Print abbiamo una fase 0 nella quale un caller effettua un ordine specificando il numero di pezzi e la qualità della stampa (che deciderà il profilo di slicing da utilizzare).
Dopo la fase di scheduling verranno selezionati i Maker che andranno ad effettaure le stampe del desing richiesto, nel front-end di questi arriverà quindi la notifica per iniziare a stampare. 
I maker attualmente occupati o non disponibili alla stampa saranno scartati dalla fase di scheduling. 

Il Caller effettuerà un pagamento preventivo trasferendo i token necessari per la stampa all'indirizzo dello SmartContract di Air Factories 2.0 .

#### Fase 1 - Inizio Stampa

Una volta apparsa la notifica sul front-end del Maker questo potrà avviare la fase di stampa automatica. 

#### Fase 2 - Recupero STL e Profilo Slicing,  Inizio Stampa

##### 2.1 - Recupero HASHES: STL and Slicing Profile

Come prima cosa, a partire dall'ordine richiesto viene recuperato l'HASH dell'STL e l'HASH del profilo slicing per la stampante che viene utilizzata. 

###### 2.2 - Download STL and Slicing Profile

Tramite gli HASHES viene effettuato il download da IPFS dell'STL del design richiesto e del profilo dello slicing necessario per creare il GCODE

Non viene mostrato nel diagramma, ma fondamentalmente viene creato il GCODE a partire dall'STL e dal profilo appena scaricati, il GCODE viene successivamente caricato su IPFS, servirà in un secondo momento nella fase di Validazione.

##### 2.3 - Timestamp: Start Printing

Viene salvato sullo smartcontract di Air Factories 2.0 il timestamp di inizio stampa del design

#### Fase 3 - Raccolta Data

Insieme alla fase 4, la fase 3 si ripete fintanto che la stampante non avrà finito la realizzazione del pezzo.

##### 3.1 - Inizio Stampa

Il Controller tramite octorest comunica con OctoPrint il quale contattando il FW della stampante fa partire il processo di stampa e avvia il servizio di timelapse

###### 3.2 - Timelapse

Vengono effettuate delle foto del processo di stampa ogni N Layer stampati. Queste vengono salvate sul controller.

##### 3.3 - Salvataggio delle foto su IPFS

Ogni foto scattata durante il processo di stampa viene salvata su IPFS e viene salvato l'HASH di quest'ultima sul controller.

##### 3.4 - Invio Hash della foto ad Hyperledger

L'hash della foto appena salvata su IPFS viene inviato (Insieme all'HASH del GCODE) ad Hyperledger tramite una chiamata REST.

##### Attenzione

Qualora durante la fase di stampa il processo dovesse interrompersi per un qualsiasi errore, verrà inviato il timestamp di fine stampa alla blockchain Ethereum considerando la stampa fallita. 

#### Fase 4 - Verifica e Validazione Stampa

##### 4.1 - Recupero Foto e GCODE

Tramite gli HASH ricevuti vengono scaricati il GCODE creato dal controller e la foto allo strato N del processo di stampa del design 

##### 4.2 - Verifica qualità Stampa

Tramite l'algoritmo spiegato più nel dettaglio nella sezione di [Hyperledger](Hyperledger.md) viene verificato che il processo di stampa allo strato N stia soddisfacendo i canoni della qualità richiesti. Il risultato della verifica al suddetto strato viene salvato sulla blockchain Hyperledger

#### Fase 5 - Fine Stampa

##### 5.1 - Fine stampa del singolo pezzo

Il FW della stampante setterà il proprio stato in "Operational" ad indicare che la stampa è terminata (In caso di errore/interruzione della stampa lo stato sarebbe "Canceling").

##### 5.2 - Invio timestamp fine Stampa

Il controller tramite OctoPrint verificherà che la stampa sia terminata e invierà allo Smart Contract di Air Factories 2.0 il timepstamp per indicare che la procedura di stampa è andata a buon fine e non sia stata interrotta. 

##### Attenzione

Prima di procedere con la fase 5.3 ossai il Claim del Reward, le precedenti fasi devono essere portate a termine per OGNI pezzo del design richiesto

##### 5.3 - Claim Reward

Una volta terminata la stampa di tutti i pezzi ordinati dal Caller l'utente può richiedere di ricevere i token previsti per la stampa. 

Verrà dunque effettuata una chiamata al metodo dello Smart Contract per la richiesta del pagamento

##### 5.4 - Oracolo Ethereum: Recupero risultato di validazione

Per poter recuperare le informazioni contenute sulla Blockchain Hyperledger, Ethereum deve necessariamente passare da un Oracolo. L'Oracolo farà una chiamata GET sul Server REST connesso ad Hyperledger che recupererà i dati necessari affinché Ethereum possa decidere se pagare il Maker o meno

##### 5.5 - Recupero risultato validazione da Hyperledger

Per ogni pezzo stampato, sulla blockchain di Hyperledger ci sono i risultati della verifica di qualità della stampa effettuati su ogni strato, quindi ad ogni pezzo è associata una percentuale di qualità stampa: se tutti i pezzi hanno un valore maggiore del 70/80% (DA DECIDERE) allora l'ordine è considerato completato con successo.

##### 5.6 - Pagamento Maker

Una volta recuperato il risultato della stampa da Hyperledger, lo smartcontract a partire da questo deciderà se pagare il Maker che ha effettuato la stampa o rimborsare il Caller.

## 
