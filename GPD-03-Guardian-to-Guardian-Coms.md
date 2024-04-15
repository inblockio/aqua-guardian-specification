Author: Tim Bansemer
Status: Draft
Date: 15.04.2024

Context: After guardians are connected via a secure transport layer and have completet their trust-handshake, they need to exchange information to exchange data in a robust and reliably manner. This is done through system message between Guardians.

Goal: Have a transport-layer independent system message specification enabling guardians to e.g. exchange and sync aqua-chains based on single revisions.For this Guardians effectivly exchange messages to announce the available AQUA-Chains as an offering for other Guardians to fetch.

Input: [AQUA Contracts] which are aqua-chains which contains specifically structured data which can be enforced by the Guardian. For example GPD-00,GPD-01 to establish trust between PKC-Guardian and Guardian-to-Guardian. Those Contracts need also to allow data governance and data access management. For this they enforce [AQUA Contracts] which are called [Data Access Agreements] and [Data Usage Agreements]which specifically definy access conditions and usage policies which can include e.g. automatic deletion after a pariod of time.

Output: Messages which control the data-exchange through messages between Guardians. After trust has been established, Guardians start to talk.


Process: This process describes how a data offering is compiled and how another guardian is fetching this data based on the offering.

Check aliveness, this should happen regularly in a randomized timewindow to distribute traffic and ensure a status check availability for message exchange.
Request Hello: Guardian(blue) initiates a conversation
Reply Hello: Guardian(red) confirms it is listening

Provide offering: Guardian(blue) I have the following public aqua-chains available for you:
<genesis hash><latest verification hash>
<genesis hash><latest verification hash>
<genesis hash><latest verification hash>

I have the following permissioned aqua-chains available for you:
<genesis hash><latest verification hash>
<genesis hash><latest verification hash>
<genesis hash><latest verification hash>

I have the following permissioned aqua-chains which require a signed [Data-Usage-Agreement] before exchange:
<genesis hash><latest verification hash>
<genesis hash><latest verification hash>
<genesis hash><latest verification hash>

Following the API-Specification of the AQUA-Implementation in Mediawiki a subset of the following API actions should be exposed for inter Guardian communication to sync aqua-chains:

* List all revisions which belong to a <genesis_hash>
* Give me revision <verification_hash>

See API reference implementation here https://github.com/inblockio/mediawiki-extension-Aqua/includes/API:
* ApiExportPage.php [expose me]
* GetPageLastRevHandler.php [expose me]
* ImportRevisionHandler.php [expose me]
* VerifyPageHandler.php [?]
* AuthorizedEntityHandler.php [?]
* GetRevisionHandler.php [expose me]
* RequestHashHandler.php [?]
* WriteStoreSignedTxHandler.php [expose me]
* DeleteRevisionsHandler.php [expose me]
* GetRevisionHashesHandler.php [?]
* RequestMerkleProofHandler.php [?]
* WriteStoreWitnessTxHandler.php [?]
* GetHashChainInfoHandler.php [?]
* GetServerInfoHandler.php [?]
* SquashRevisionsHandler.php [?]
* GetPageAllRevsHandler.php [expose me]
* GetWitnessDataHandler.php [expose me]
* TransclusionHashUpdater.php [?]


Boundary conditions: Those actions lead to a clear systems-message communication supporting AQUA-Chain handling. Furthermore elements for resialent communication need to be taken into consideration. Secure transport layer with TCP sessions and or other protocol specific error-handling should ensure fault-free transport. Guardians validate every data-set before writing it into the inbox of another PKC adding another layer to ensure integrity of the data. What needs handling is unavailability of Guardians and low bandwith communication.
