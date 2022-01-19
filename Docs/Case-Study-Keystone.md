# Keystone
### A #SmartCustody Case Study

The Keystone is an airgapped HD wallet/offline signer.

## Usage

The Keystone holds seeds and can interact with the Keystone Mobile App, which acts as a watch-only wallet. Transactions are prepared by the Mobile App, then sent to the Keystone via airgap for signing.

The normal process for using a Keystone is:

1. Create a wallet on Keystone or import using BIP-39 words or shards from Shamir's Secret Sharing.
2. Back up wallet by writing down BIP-39 words or shards from Shamir's Secret Sharing.
3. Bind Keystone to Mobile App by displaying QR code on Keystone and reading it on Mobile App. This presumably transfers an xpub to the Mobile App, encoded as `ur:bytes`.
4. Receive funds by reading address QR codes from Keystone or from Mobile App into remote wallet.
5. Send funds by creating a transaction on the Mobile App, then generating a QR for signing, which presumably is a PSBT, encoded as `ur:bytes`.
6. Read and sign the transaction on the Keystone, then send it back, now presumably as a fully signed PSBT, encoded as `ur:bytes`.

Additional wallets on Keystone may be created using the "Hidden Wallet" feature. Only one wallet is displayed at a time; switching to another requires a passphrase, with no indication given on the device that the additional wallet exists.

## Gordian Principles

As an airgapped wallet, the Keystone matches the general design of the [Gordian Architecture](https://github.com/BlockchainCommons/Gordian#overview-gordian-architectural-model).
It interacts with the more specific Gordian Principles as follows:

**Independence.**

Keystone's independenced focuses on the ability to transact funds without outside intervention.

* Keystone allows direct, personal control of all assets. The master seed resides on the Keystone; a watch-only wallet exists on the Mobile Device.
* Keystone does _not_ provide an options for choosing cryptocurrency servers to work with, potentially creating a path for censorship that might require removing the funds to another device.

**Privacy.**

The use of the Keystone and the Mobile Device together keep a user's information close.

* As a closely held device, Keystone maximizes the privacy possibilities for private keys.
* The Keystone is only accessible through QR codes or a micro-SD card.
* Again, Keystone does _not_ provide options for choosing cryptocurrency servers, and so it's possible that data honeypots could be created: the user doesn't know.

**Resilience.**

Keystone's resilience depends on users properly managing their backups, with the biggest advance being the inclusion of Shamir's Secret Sharing.

* The physical device is protect by password and/or fingerprint.
* A Secure Element stores the private keys.
* The device is designed to self-destruct in the case of physical tampering (which can reduce the danger of single-point-of-compromise but increase the danger of single-point-of-failure).
* Master seeds can be backed up using written BIP-39 words. 
* Keystone suggests backing up the BIP-39 words to the steel Keystone Tablet.
* Master seeds can alternatively be backed up using Shamir's Secret Sharing and writing down the shards.
* Keystones does _not_ provide any multisig capabilities, so BIP-39 words can act as a single-point-of-compromise.
* Because there are _not_ multisig capabilities, loss of the Keystone device along with the backed up words can act as a point-of-failure.
 
**Openness.**

Keystone provides open information on their software and on their hardware audit. Interoperability is more limited, but there is full ability to move seeds on or off device.

* Seeds can be transferred onto or off of the device using BIP-39 or Shamir's Secret Sharing.
* Interactivity is provided not just with the Mobile Device, but also Metamask and a few other specific online coordinators.
* Uniform Resources (URs) underlie QR codes, _but_ they are encoded as `ur:bytes` not more interoperable UR types such as `ur:hdkey` or `ur:psbt`
* Software is [open source](https://github.com/KeystoneHQ).
* Hardware audit is [publicly available](https://github.com/KeystoneHQ/Keystone-developer-hub/blob/main/audit-report/cobo_audit_report_2020_09_en_1_0.pdf).

## Adversaries

The Keystone offers specific defenses against the following [#Smartcustody](https://www.smartcustody.com/) adversaries:

**Bitrot.**

Provided that a sure correctly stores their Recovery Phrase, they can transfer their seeds to another device if either Keystone or the Mobile Device grows obsolete. A Recovery Phrase Check allows users to occasionally ensure that phrase remains valid. 

However, there is no way to regenerate the BIP-39 words or Shamir shards after initial creation, so if they are later lost, and the user does not move the funds to a new seed, the danger of Bitrot can resurface.

**Coercion.**

The Passphrase Wallet feature, which allows for the creation of additional wallets that are only visible if a user types in the correct passphrase, is an interesting methodology to try and prevent physical coercion. The idea of public wallets and secret wallets has long been a proposed solution to this adversary, and is fully enacted on the Keystone.

**Convenience.**

The Keystone provides sufficient convenience that users might be discouraged from using more "convenient", lower security methodologies, such as conducting transactions on a directly connected devices. The QR codes allow quick transfers across airgaps and the Keystone's high-quality touchscreen means that they don't have to fumble with the low-resolution LED screens and push buttons of the last generation of hardware wallets, which may have been sufficient to deter their usage.

**Disaster.**

Shamir's Secret Sharing allows a strong defense against disaster, if shards are in widely separated areas, but it requires both the use of Shamir's Secret Sharing, over the simpler BIP-39 methodology, and their separation.

**Institutional Theft.**

With private keys held on the Keystone, with all transaction creation done on the Mobile Device, and with source opened, the possibilities of institutional theft are minimized. (As always, the manufacturer must still be trusted.)

**Key Fragility.**

Key fragility is one of the biggest threats to individual cryptocurrency holders, and Shamir's Secret Sharing is a first step toward minimizing that.

**Network Attack.**

The use of an airgapped device to hold all seeds largely eliminates the possibility of network attack.

**Physical Theft.**

By depending on fingerprints and passwords, and by destroying information is there are too many bad password guesses or if the device is physically tampered with, Keystone makes physical theft largely irrelevent as a single-point-of-compromise. However, it still can be a single-point-of-failure if BIP-39 words or Shamir shards were not correctly held and protected.

**Supply-Chain Attack.**

A Keystone authenticates itself with the Keystone website on first usage. That, with its self-destruction in the case of physical tampering, minimizes the possibility of a supple-chain attack.

**Transaction Error.**

Like most modern wallets, the Mobile App takes care of most of the transaction details, such as scanning in addresses through QRs and determining fees. This greatly reduces the possibility of transaction errors.

_Death/Incapacitation remains one of the largest outstanding adversaries, though use of Shamir's Secret Sharing can resolve that. Though Keystone advertises a true random number generator, and backs that up by releasing its code, they also provide documentation on generating seeds with dice, which can remove the possibility of Systemic Key Compromise. Censorship and Correlation may or may not be possible depending on the design of the integration of cryptography servers._

## Specifications

Transfer of information between the Keystone and Mobile Device primarily occurs through QR codes that are encoded as `ur:bytes`. General interoperability with other devices is not possible because `ur:hdkey`, `ur:psbt`, `ur:request`, `ur:response`, and other specific UR types are not used.

## Hardware & Software

[todo]

## Final Notes

The first generation of #SmartCustody primarily depended on good procedures for storing recovery words, which Keystone supports through its BIP-39 output and its Keystone Tablet.

A second generation focuses on Shamir's Secret Sharing, which Keystone also supports.

There is some interoperability for Keystone, allowing its use with specific other devices, which provides increased independence to the user, but it's thus far limited, and Keystone does not support the third generation of #SmartCustody, where single-points-of-failure and compromise are further reduced with multisigs.