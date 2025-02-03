Asset Management System on Hyperledger Fabric
==============================================

Overview
--------
This system allows users to manage assets on a Hyperledger Fabric blockchain network. The operations supported are:

- **Add** an asset with a unique ID, owner name, and value.
- **Read** an asset's details by ID.
- **Update** an asset's owner or value.
- **Delete** an asset by ID.

Steps Implemented
-----------------

1. **Set Up Hyperledger Fabric Network**
   - Used the Fabric test network (`test-network` from Fabric samples).
   - Configured channels, peers, and certificate authorities (CA).
   - Created a channel ``mychannel`` and joined peers to it.

2. **Develop Chaincode (Smart Contract)**
   - Wrote chaincode in Go to handle asset CRUD operations.
   - Implemented functions:
     - ``AddAsset``: Stores asset data (ID, owner, value) on the ledger.
     - ``ReadAsset``: Retrieves asset details by ID.
     - ``UpdateAsset``: Modifies owner/value of an existing asset.
     - ``DeleteAsset``: Removes an asset from the ledger.
   - Used ``shim`` library to interact with the ledger state.

3. **Build Client Application**
   - Developed a Node.js client using Fabric SDK.
   - Implemented methods to invoke chaincode functions:
     - ``invokeAddAsset``, ``queryReadAsset``, ``invokeUpdateAsset``, ``invokeDeleteAsset``.
   - Configured wallet identities and connection profiles.

4. **Deploy and Test**
   - Installed/approved chaincode on the channel.
   - Tested all operations via the client app and verified ledger updates.

Setup & Run Instructions
------------------------

Prerequisites
^^^^^^^^^^^^^
- Docker and Docker Compose.
- Node.js 16+ and npm.
- Hyperledger Fabric v2.5+ binaries and samples.

1. **Clone Repository**
   .. code-block:: bash

      git clone https://github.com/hyperledger/fabric-samples.git

2. **Start Fabric Network**
   .. code-block:: bash

      cd fabric-samples/test-network
      ./network.sh up createChannel -c mychannel -ca
      ./network.sh deployCC -ccn asset-management -ccp ../chaincode/asset-management/go -ccl go

3. **Install Client Dependencies**
   .. code-block:: bash

      cd client
      npm install

4. **Run Client Application**
   .. code-block:: bash

      node app.js

Chaincode Details
-----------------
- **Language**: Go.
- **Key Functions**:
  - ``AddAsset``: Uses ``stub.PutState()`` to store asset data.
  - ``ReadAsset``: Uses ``stub.GetState()`` to fetch asset data.
  - ``UpdateAsset``: Validates existence before updating via ``PutState()``.
  - ``DeleteAsset``: Removes data with ``stub.DelState()``.

Cleanup
-------
.. code-block:: bash

   ./network.sh down  # Stops the Fabric network