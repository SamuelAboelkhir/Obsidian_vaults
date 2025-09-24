---
tags:
- WR
- VA
MOC: Work
---
[[_0000 Home|Home]] | [[_0002 Work MOC|Back to Work MOC]] | [[WR VA Diagnostics index|Back to index]]
# Steps:

- Order arrives via phone or IDA
- Creation of the BOL
    - A legal document stating the action of delivering cargo from point A to point B
- Creation of the probill (transport document) PBDFF in the DB
    - A probill is a collection of BOLs (relation: many to one)
    - Can be created manually(Phone) or automatically(IDA)
- The probill is passed to operations
- The merch is then picked up by a delivery person
- Probills are charged at the end
- The probill is then added to  a delivery sheet
- Delivery sheet = 1 or more probills = 1 or more BOLs, goes into a container
- The sheet is taken by a driver to make the delivery and the probill is updated to delivery in progress
    - The delivery is sequential; you deliver each client's order in order of who you reach first in the delivery route
    - Delivery by order of location takes precedence over urgency of delivery
- The probill status is updated based on data received from ISSAC, which can be done manually(Phone) or automatically(IDA)
- The driver must confirm the delivery
- Everything is approximated, regarding the weight and volume
- They're unable to confirm the exact details of the order before it reaches them, meaning the driver arrives to pick up the merch with approximate data, which means the probill would need to be updated
- Even when the delivery sheet is updated with delivered probills, that may not be true due to varying factors
- To compensate, there is an operational document that indicates what has actually been delivered
- The driver gives his delivery sheet to the operators who then confirm the deliveries
- All the documents are enumerated in librex manually
- The employees must confirm the deliveries so that they correspond with the probill
- Once the delivery sheet goes from the operational stage and into the administrative stage, it's completed
- The delivery sheet in the operational stage has the status:
    - Pending delivery
    - Delivered
- In the administrative stage:
    - Delivery confirmed
    - Delivery not confirmed
- Bills are generated when the order is received, but it's corrected after the probill is fully updated
- Probill elements: what will be delivered, quantity, volume
- A container's volume is 2800 cubic feet
- An order can't exceed the size of a container
- If an order exceeds the container size, it will counts as 2 different orders spread over 2 probills

# Tables

They have a constants system With a constants table that explains the existing tables and their use
GECTF01
select * from vaqacdta/gectf01 to select this table
Not all modules are functional
Each function is a module
First 2 letters are always a prefix indicating the module
AC: accessoires chauffeur camion remorquecontroller les cartes de credits par exemple
AF: Frais AccessoireRelated to the probill module
AG: agence - chauffeurs independantsExternally hired drivers
AN: Action table - prise de commandeprise de telephone etc
cueillette chez un client
nest pas directement lie au connaisssement
AR: pas utilise
BG: pas utilise
BL: BOL (connaisssement), the most important tableDFF01: Principal table
The other tables are annex tablesThe hold extra metadata regarding the main table
01: The leader or entry to the table group
Others represent the other POs
The other numbers have connections to the main table
The B/L number refers the principal tableThey refer to a group of notes that's related to a single BOL
BN: banque et tarif client
CS: Client
DR: Driver
DT: transmission de donnees - transfert info aux systemes fournisseur
DL / DV : ca fonctionne pasHas the data, but with an ancient structure
ED: EDI
EM: Employes
FI : fonctionne pas
FL: Filtre dans les fichiers - en haut en droite
FM: Fleetmind - Isaac
GE: General
IN: facturation - programmation pas de base de donnees.
IS: Isaac (plus de 30 tables a modifier)
PB: Probillpblsts: probill status
TK: Truck : camion
TR: remorque
TS: Task
GE: General - For general functions
VA is the main DBEnvironments
qac: Quality DB
dev: Development DB
prd: production DB
Q[Prefix]: is a file that shows a bird's eye view of the existing table group of a certain table typeexample: QPB
Table des constantes GECTF01
EXT tables are empty tables that act as structure tables, not actual data tables (blueprints)
There are tables that are normalized, and others that are not normalized

vaqac ne contient pas de tables
RDI: Rationnel Des base de donner IBM


select * from [tableName] where [tableKey=key]


# Tables

- They have a constants system
    - With a constants table that explains the existing tables and their use
    - GECTF01
    - select * from vaqacdta/gectf01 to select this table
- Not all modules are functional
- Each function is a module
- First 2 letters are always a prefix indicating the module
- AC: accessoires chauffeur camion remorque
    - controller les cartes de credits par exemple
- AF: Frais Accessoire
    - Related to the probill module
- AG: agence - chauffeurs independants
    - Externally hired drivers
- AN: Action table - prise de commande
    - prise de telephone etc
    - cueillette chez un client
    - nest pas directement lie au connaisssement
- AR: pas utilise
- BG: pas utilise
- BL: BOL (connaisssement), the most important table
    - DFF01: Principal table
    - The other tables are annex tables
        - The hold extra metadata regarding the main table
    - 01: The leader or entry to the table group
    - Others represent the other POs
    - The other numbers have connections to the main table
    - The B/L number refers the principal table
        - They refer to a group of notes that's related to a single BOL
- BN: banque et tarif client
- CS: Client
- DR: Driver
- DT: transmission de donnees - transfert info aux systemes fournisseur
- DL / DV : ca fonctionne pas
    - Has the data, but with an ancient structure
- ED: EDI
- EM: Employes
- FI : fonctionne pas
- FL: Filtre dans les fichiers - en haut en droite
- FM: Fleetmind - Isaac
- GE: General
- IN: facturation - programmation pas de base de donnees.
- IS: Isaac (plus de 30 tables a modifier)
- PB: Probill
    - pblsts: probill status
- TK: Truck : camion
- TR: remorque
- TS: Task
- GE: General - For general functions
- VA is the main DB
- Environments
    - qac: Quality DB
    - dev: Development DB
    - prd: production DB

- Q[Prefix]: is a file that shows a bird's eye view of the existing table group of a certain table type
    - example: QPB
- Table des constantes GECTF01
- EXT tables are empty tables that act as structure tables, not actual data tables (blueprints)
- There are tables that are normalized, and others that are not normalized

vaqac ne contient pas de tables

RDI: Rationnel Des base de donner IBM