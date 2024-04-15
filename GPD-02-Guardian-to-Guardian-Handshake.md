Author: Tim Bansemer
Status: Draft
Date: 15.04.2024

Context: Guardians do not communicate by default with any external party reducing their attack surface. To establish a communication between two guardians, there must be a handshake-process which establishes this trust based on the relationship of wallet-addresses, services and signatures.

Goal: Establish trust between two guardians, which are linked to two pkcs. When trust is established the two guardians start systems communication which is used to send messages back and forth to share data with each other.

Input: A aqua-chain which holds the relationship of <account 1><pkc 1><guardian 1> to <account 2><pkc 2><guardian 2>

Output: A state change in the guardian 1/2, leading to initialize systems communication with the other guardian.

Process: Verify the aqua-chain used to establish trust. Write an access policy. Use access policy to open services for inter-guardian communication. Successful and unsuccessful handshakes are logged by the guardian.

Step-by-Step (red and blue is to differenciate the two domains):
* 1: Issue Guardian Certificte: Guardian(blue) write self signed Guardian-Service-Certificate to PKC(blue).
* 2: Attest trust to Guardian: PKC(blue) owner-wallet signs Guardian-Service-Certificate(blue)
* 3: Create Request: PKC(blue) owner-wallet creates a Guardian-Trust-Request(blue) which includes the Guardian-Service-Certificate(blue) and the wallet-address of the remote PKC(red) owner-wallet.
* 4: Transport Request: PKC(blue) owner exports Guardian-Trust-Request(blue) and sends it to remote PKC(red) owner via independent communication channel.
* 5: Validate Request: Remote PKC(red) owner verifies the Guardian-Trust-Request(blue).
* 6: Import Request: importing it (or importing it via the Guardian?).
* 7: Confirm Request: Remote PKC(red) owner signs the Guardian-Trust-Request(blue).
* 8: Create Response: Remote PKC(red) detects valid Guardian-Trust-Request(blue), adds its own Guardian Service Certificate(red) and tries to establish a connection according to the transport services description within the Guardian-Service-Certificate(blue)
* 9: Transport Response: PKC(blue) receives remote signed Guardian-Trust-Response (Includes: Guardian-Service-Certificate(blue), Guardian-Trust-Request(blue), Guardian-Service-Certificate(red), Guardian-Trust-Response(red)).
* 10: Verify Response: Guardian(blue) Verify all assets. When valid stores the Guardian-Service-Certificate(red) in PKC(blue)
* 11: Finish Handshake: Send confirmation message (Guardian (blue) signed aqua-message to Guardian(red)) confirming successful handshake.

Boundary conditions:
* If the guardian has not been initilized it should reject any attempt to estalbish trust connections.
* If Step 9: transport fails, it should retry a defined number of times before creating a failure message. There should be a way to re-trigger step 9, to try to establish the connection again.
* Every action of the guardian should be logged into a seperate guardian log file. This file should be limited in its size or/and be limited by nuber of entries.
* Systems communication should be logged seperately for each guardian-to-guardian communication


