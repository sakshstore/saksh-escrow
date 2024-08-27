# Saksh Escrow



Saksh Escrow is a Node.js package that provides an escrow service for secure transactions between users. It allows users to initiate, release, cancel, and dispute transactions, ensuring a safe and reliable payment process.



## Features



- Initiate an escrow transaction

- Release funds from escrow

- Cancel an escrow transaction

- Raise and manage disputes

- Submit evidence for disputes

- Escalate disputes to mediation

- Resolve disputes and mediations



## Installation



To install the package, run:



```bash

npm install saksh-escrow

```



## Usage



### Prerequisites



- Node.js

- MongoDB (Make sure to have a MongoDB instance running)



### Example



Here's a basic example of how to use the Saksh Escrow package:



```javascript 



const mongoose = require('mongoose');

//const EscrowService = require('saksh-escrow'); // Adjust the path as necessary

//const WalletUser = require('../models/WalletUser'); // Assuming you have this file

const defaultConfig = require('../config/config'); // Adjust the path as necessary



 const { EscrowService, WalletUser, config } = require('saksh-escrow');



async function main() {

    // Connect to MongoDB

    await mongoose.connect('mostr', {

        useNewUrlParser: true,

        useUnifiedTopology: true,

    });







    const sender = new WalletUser({

        userId: 'sender123',

        balances: new Map([['USD', 1000]]),

    });



    const receiver = new WalletUser({

        userId: 'receiver123',

        balances: new Map([['USD', 1000]]),

    });



    const escrowAdmin = new WalletUser({

        userId: 'susheelhbti',

        balances: new Map([['USD', 1000]]),

    });







    //await WalletUser.deleteMany({});



    await sender.save();

    await receiver.save();

    await escrowAdmin.save();







    // here sender is a logged in user in the system 

    const escrowService = new EscrowService(sender, WalletUser);







    try {

        // Example: Initiate an escrow

        const escrow = await escrowService.initiateEscrow('sender123', 'receiver123', 100, 'USD', 'Payment for services');

        console.log('Escrow initiated:', escrow);



        // Example: Release funds from escrow

        const releasedEscrow = await escrowService.releaseEscrow(escrow._id);

        console.log('Escrow released:', releasedEscrow);



        // Example: Cancel an escrow

        const canceledEscrow = await escrowService.cancelEscrow(escrow._id);

        console.log('Escrow canceled:', canceledEscrow);



        // Example: Raise a dispute

        const disputedEscrow = await escrowService.raiseDispute(escrow._id, 'Service not delivered', 'Service Issue');

        console.log('Dispute raised:', disputedEscrow);



        // Example: Submit evidence for a dispute

        const evidenceEscrow = await escrowService.submitEvidence(escrow._id, 'http://example.com/evidence.jpg');

        console.log('Evidence submitted:', evidenceEscrow);



        // Example: Escalate a dispute

        const escalatedEscrow = await escrowService.escalateDispute(escrow._id, 'Level 2');

        console.log('Dispute escalated:', escalatedEscrow);



        // Example: Escalate to mediation

        const mediationEscrow = await escrowService.escalateToMediation(escrow._id, 'mediator123');

        console.log('Escalated to mediation:', mediationEscrow);



        // Example: Resolve mediation

        const resolvedMediation = await escrowService.resolveMediation(escrow._id, 'RELEASE', 'mediator123');

        console.log('Mediation resolved:', resolvedMediation);



        // Example: Resolve a dispute

        const resolvedDispute = await escrowService.resolveDispute(escrow._id, 'RELEASE');

        console.log('Dispute resolved:', resolvedDispute);

    } catch (error) {

        console.error('Error:', error);

    } finally {

        // Close the MongoDB connection

        await mongoose.connection.close();

    }

}



main();

```



### API



#### `EscrowService(sender, WalletUser)`



- **Parameters:**

  - `sender`: The user initiating the escrow.

  - `WalletUser`: The WalletUser model for database operations.



#### `initiateEscrow(senderId, receiverId, amount, currency, description)`



- **Parameters:**

  - `senderId`: ID of the sender.

  - `receiverId`: ID of the receiver.

  - `amount`: Amount to be held in escrow.

  - `currency`: Currency type (e.g., 'USD').

  - `description`: Description of the transaction.

- **Returns:** The created escrow object.



#### `releaseEscrow(escrowId)`



- **Parameters:**

  - `escrowId`: ID of the escrow to be released.

- **Returns:** The released escrow object.



#### `cancelEscrow(escrowId)`



- **Parameters:**

  - `escrowId`: ID of the escrow to be canceled.

- **Returns:** The canceled escrow object.



#### `raiseDispute(escrowId, reason, type)`



- **Parameters:**

  - `escrowId`: ID of the escrow.

  - `reason`: Reason for raising the dispute.

  - `type`: Type of the dispute.

- **Returns:** The disputed escrow object.



#### `submitEvidence(escrowId, evidenceUrl)`



- **Parameters:**

  - `escrowId`: ID of the escrow.

  - `evidenceUrl`: URL of the evidence.

- **Returns:** The escrow object with submitted evidence.



#### `escalateDispute(escrowId, level)`



- **Parameters:**

  - `escrowId`: ID of the escrow.

  - `level`: Level to escalate the dispute.

- **Returns:** The escalated dispute object.



#### `escalateToMediation(escrowId, mediatorId)`



- **Parameters:**

  - `escrowId`: ID of the escrow.

  - `mediatorId`: ID of the mediator.

- **Returns:** The escrow object escalated to mediation.



#### `resolveMediation(escrowId, action, mediatorId)`



- **Parameters:**

  - `escrowId`: ID of the escrow.

  - `action`: Action to take (e.g., 'RELEASE').

  - `mediatorId`: ID of the mediator.

- **Returns:** The resolved mediation object.



#### `resolveDispute(escrowId, action)`



- **Parameters:**

  - `escrowId`: ID of the escrow.

  - `action`: Action to take (e.g., 'RELEASE').

- **Returns:** The resolved dispute object.



## License



This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.



## Contributing



Contributions are welcome! Please open an issue or submit a pull request.



## Contact



For any inquiries, please contact susheel2339 @ gmail.com.
