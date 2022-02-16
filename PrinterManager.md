# Printer Manage

This document contains the description of the Printer Manager.

## Printer HUB

The printer HUB is composed of a Raspberry PI 3 Model A+ in which are connected 2 camera, the first one is a Raspi Camera v2 and the other is a webcam.

The Raspberry is connected to the 3d Printer via USB in order to comunicate with it.

This setup allow to use OctoPrint to send command to the 3d Printer Firmware, enable the printing process and the camera recording. 

## [Printer Controller](https://github.com/Air-Factories-2-0/af2-printer-controller)

The Printer Controller allow the user to interact with the printer during the PoP.

It expose a Server that will be contacted every time an order is sent to the **Maker**, in order to start the data collection, and printing validation process; to achieve this goal the controller must be able to interact with IPFS, Octoprint, Ethereum and Hyperledger.

### [IPFS](https://ipfs.io/)

> A peer-to-peer hypermedia protocol designed to preserve and grow humanity's knowledge by making the web upgradeable, resilient, and more open.

IPFS is the InterPlanetary File System in which the files are stored based on their  HASHES.

### [OctoPrint](https://octoprint.org/)

> **OctoPrint** is an open source 3D printer controller application, which provides a web interface for the connected printers. It displays printers' status and key parameters and allows user to schedule prints and remotely control the printer

### Flask Server

Flask is used to create a simple REST server that will be contacted from the user to start the printing process and the PoP.

Following there is an example of one API implementation on Air Factories 2.0:

```python
from flask import Flask, request, jsonify, Response
from Controller import Controller, IPFS, Hyperledger, Octoprint
import json, threading

controller = Controller(
    Octoprint = Octoprint(),
    IPFS = IPFS(),
    Hyperledger = Hyperledger()
)

app=Flask(__name__)
@app.route('/start', methods = ["POST"])
def start():
  try:
    #! If I'm already printing stop the call
    if controller.getOcto().isPrinting():
      return Response(json.dumps({"status":"Bad Request","Error" : "Already Printing"}), 400, mimetype="application/json")

    #! Get the STL hash from the body request
    stl_hash=request.get_json().get('stl_hash',None)
    if not stl_hash:#! L'hash è settato nel body? Se no errore
      return Response(json.dumps({"status":"Bad Request", "Error": "Missing stl_hash key"}), 400, mimetype="application/json" )

     #! Get the STL hash from the body request
    pieces=request.get_json().get('pieces',None)
    if not stl_hash:
      return Response(json.dumps({"status":"Bad Request", "Error": "Invalid number of pieces to print"}), 400, mimetype="application/json" )

    #! Creating the response 
    #! The response will contain the status and acknowledge about the stl_hash sended
    x = threading.Thread(target=controller.start, kwargs=({"hash":stl_hash, "pieces":pieces}))
    x.start()      
    return Response(json.dumps({"status":"start printing","stl_hash" : stl_hash}), 200, mimetype="application/json")

  except Exception as e: #! If something should go wrong an error will be returned
    return Response(json.dumps({"status":"Internal Server Error", "Error": str(e)}), 500, mimetype="application/json" )
```

#### Web3

Web3 allow the communication with a Ethereum node in order to contact the methods defined in a certain Smart Contract.

To be able to call a method it is necessary to have the ABI of the Smart Contract.

This is an example of usage:

```python
from web3 import Web3

web3=Web3(Web3.HTTPProvider("url"))
smcAddress="SmartContract Address"
smcPath=str(os.path.abspath(("smartcontract/abi/path")))
abi = None
web3.eth.defaultAccount = self.__web3.eth.accounts[1]  
with open(smcPath)as f:
            json_data=json.load(f)
            abi=json_data["abi"]

contract=web3.eth.contract(address = smcAddress, abi = abi)
contract.functions.printingBegin(self.__printer,tstmp,design_bytes32).transact()
```

#### OctoRest

OctoRest is a python library that allow the comunication with OctoPrint via REST request.

It contains method that contact a specific API in order to perfom a specific action.

For instance, after the authentication to start a printing process it is possible to use the following method:

```python
import octorest

url="http://localhost:5000" 
api_key="YOURAPIKEY"

#Authentication via APIKEY
client=octorest.OctoRest(url=self.__url,apikey=self.__api_key)
#Select the default printer connected (If only 1 printer is connected that one is the default one)
client.connect(autoconnect = True)
#Start printing
client.select("GCODE/PATH/IN/OCTOPRINT",print=True)
```

#### requests library

The request library simply allow to perform HTTP request that will be used in order to communicate with the Server connected to Hyperledger Fabric Blockchain

After retriving the photo taken during the printing process these will be uploaded in IPFS and their hashes will be sended to Hyperledger Server.

```python
import requests
import json
class Hyperledger:

    address="http://192.168.1.53:10000/"

    def __init__(self, address = address):
        self.__address = address

    def send_hash(self, hash):
        r = requests.post(self.__address, json = {"hash":hash})
        r = json.loads(r.content.decode("utf-8"))
        return r
```
