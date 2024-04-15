Author: Tim Bansemer
Status: Draft
Date: 15.04.2024

Context: When the Guardian service is started for the first time, it needs to self-configure to enable its functionality as a firewall like service to protect services behind it, for example the PKC.

Goal: Generate own wallet address, establish trust with the PKC during setup.

Input: Guardian Docker Container with dependencies including libraries required to generate and manage a wallet, the connection to the PKC and a verify which is extended to support and execute [AQUA Contracts]

Output: A trusted and secure connection between the Guardian and the PKC.

Process: The guardian must register itself to the PKC. The wallet address must be passed to the mediawiki and automatically elevated to admin level. The guardian must perform a certificate generation process by writing a page to the PKC which includes all relevant service information (Wallet address, service name, service identifier (should not be same as wallet address but could be a substring e.g. "guardian-4f3109" which is set as the hostname of the container / dns name etc.) and is signed by the wallet of the guardian to protect its integrity and to proof origin. The signature is required, as this certificate will serve as an association proof. This association proof will be used to establish trust in guardian-to-guardian communication.

Boundary conditions: This process is currently designed to map only 1:1 guardian to pkc relations. In the future one guardian could represent multiple services.

