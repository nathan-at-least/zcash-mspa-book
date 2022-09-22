# Introduction

The Zcash Modular Scalable Payments Architecture (MSPA) is a theoretical new blockchain architecture specialized for Zcash shielded payments and storage. The intent behind the name:

- _Modular_: Different required functionality is provided by distinct types of infrastructure for consensus, double-spend prevention, data availability, and network privacy.
- _Scalable_: The goal of the architecture is to support a high level of usage. A target metric for the scalability goal is for typical transaction fees to remain low while maintaining usability guarantees of safety, alacrity, and availability.
- _Payments_: The architecture is specialized to payments. This makes it distinct from blockchain architectures aimed at scaling stateful smart contracts.

## What is Safety?

We place substantial significance behind the term "safety", and we rely on that word to anchor technical feature goals behind a human-centric notion:

  Safety means any user can rely on the system to perform what they intend without unnecessarily placing them in harms way.

In practice, safety requires many of the more well-known cryptocurrency principles:

**Privacy by Default:** We believe people should not have to have esoteric knowledge to know which details of their behavior are exposed to which ambient third parties based on technical trivia. So, the technology must protect, as best as possible, the privacy of user behavior so that only the information they intend to disclose is available and only to the parties they intend to disclose it to.

**Permissionlessness:** If a person comes to rely on a technology only to later discover they are vulnerable to gate-keeping or censorship by a third-party without their consent, that technology fails to provide safety. So the technology must provide permissionless access so that any person is free to use it at any time at their discretion.

**Reliability:** The technology needs to function reliably at any time so that people can justifiably depend on it.

**Resilience:** If the network of participants fails to maintain and improve the technology over time, people who rely on it may one day find it no longer works. The technology must remain resilient through time under changing conditions and a variety of adversarial environments.

We posit that other commonly touted blockchain principles such as decentralization, capture resistance, availability, resilient governance, and platform neutrality all stem from these core deeper principles underlying our use of the word safety.

Of particular note is _economic sustainability_ which we frame as a requirement implied by reliability, and resilience, and which permissionlessness facilitates.

## Usability

The usability model for Zcash MSPA is as follows:

- Users may install and utilize a variety of _wallets_. More options are generally better.
- A user can freely create as many _addresses_ as they need or desire at no cost.
- Each address controls a distinct set of `ZEC` funds.
- Each address is controlled entirely by a _spend authority_: anyone with knowledge of this _spend authority_ can observe an address's entire history and spend any funds.
- Each address has an associated _viewing key_ which allows anyone with knowledge thereof to view the entire history and future interactions of the address. However, a viewing key _cannot_ spend any funds held by an address or effect any other changes. Anyone with knowledge of a spend authority can derive the associated viewing key. It is not possible to determine the spend authority given a viewing key.
- Given a destination address, a wallet holding a spend authority can initiate a transfer of funds controlled by that spend authority to be received by the destination address. When a sending wallet initiates a transfer, it generates all necessary information locally _non-interacitvely_[^1], then it interacts with the Zcash MSPA to effect the transfer.
- When such a transfer is initiated, it will either become _finalized_ by the protocol, or it will _fail to finalize_ in a known limited amount of time.
- The protocol futher requires various _fees_ to enable reliability and resilience which are requirements of safety. [**TODO:** Flesh out how fees relate to MSPA better.]

[^1]: The requirement for sending to be non-interactive is tenuous, because this may be a fruitful requirement to relax to better achieve a balance of overall goals.

**Wallet Data Availability:** In a crucial departure from modern Zcash and many cryptocurrencies, the Zcash MSPA _does not_ directly support data availability for wallets!

This means that wallets _bear the sole responsibility_ for ensuring:

- received funds are available to be spent
- history is preserved
- restoring from a backup is successful

Wallets may rely on one or more _wallet data availability providers_ to achieve this goal, as described in the [High Level Architecture Overview](#high-level-architecture-overview).

## Insights

### Unifying Privacy, Availability, and Privacy

A key insight behind Zcash MSPA is that scalability, data availability, and privacy are all deeply connected:

- Data must be _available_ to those who need it.
- Privacy requires that data must _not be available_ to anyone except the intended participants.
- _Scalability_ requires that data must propagate as efficiently as possible to only those who need it.

These goals are mutually supportive and form three pillars supporting the overall safety goals.

### Roles and Their Needs

Stepping back to review the needs of a safe payments system, consider which participants need to know what information:

| Role | Need |
| ---- | ---- |
| Holder | To see their balance and the history that lead to it. |
| Spender | To know a recipient did or did not receive a transfer in a timely fashion. |
| Recipient | To know they've received a transfer, how much, any message from the sender associated with the payment, and that the payment is legitimately reliable. |
| Spenders and Receipients | To know their activity has strong privacy so that only parties they intend to disclose information to are privy to it. |
| All Participants | To know every transfer conserves the scarcity of `ZEC`, ie there are no double-spends or counterfeiting exploits. |
| All Infrastructure Providers | To know that their contribution is economically sustainable over long enough time periods. |

## High Level Architecture Overview

The high level MSPA architecture follows directly from meeting the needs of the participants in a modular, scalable fashion while preserving the fundamental safety guarantees.

### Consensus Layer Providers

Consensus layer providers (aka CLPs) are responsible for protecting the scarcity of `ZEC` and implementing economic constraints to ensure reliability and resilience which are necessary for safety.

In the Zcash MSPA, the _only_ interaction between users and consensus is these two mechanisms:

- A recipient's _Reception Provider_ (see [Reception Providers](#reception-providers) below) must be able to submit a _finalization payload_ to the consensus layer.
- Sender and recipients must learn when transfers they are parties to become finalized.

**TODO / BUG:** How do senders and recipients learn that their transfer has been finalized?

To achieve the goal of Privacy by Default _only_ the senders and recipients which are parties to a transfer are able to learn when the relevant transfer becomes finalized.

### Wallet Data Availability Providers

Because only the sender and recipient need to know about a given transfer, this implies that a modular data availability solution can be specialized to senders and recipients, rather than global.

In Zcash MSPA this function is decoupled in a modular fashion from the rest of the architecture and provided by _Wallet Data Availability Providers_ (aka WDAPs). Each wallet is responsible for ensure data availability for wallet-specific data (which also happens to be private to the wallet user).

#### WDAP Requirements

A WDAP must:

- provide the full history of an address to a wallet on demand,
- persistently store locally generated modifications of wallet state, especially sender-initiated transfer information,
- be highly available in order to receive transfers at any time (see [Wallet Reception Providers](#wallet-reception-providers).
- persistently store remotely generated modifications of wallet state, especially incoming transfers including the `ZEC` transferred and associated messages,
- and finally, enable users to restore this data in the event that their wallet device is destroyed or otherwise needs to be restored.

### Wallet Reception Providers

Recall that only senders and recipients need and should know most details of a transfer. The only exception to this is that all participants must know that no transfer between any parties violates `ZEC` scarcity by double spending or other counterfeiting exploits.

Because transfers are non-interactively (see [^1]) generated by senders, sending wallets must directly use their WDAP to persist that information locally. However, they must later learn that a transfer was finalized.

However, recipients must come to learn relevant details of a transfer, including:

- a _payload_ including:
  - the amount of `ZEC` and associated messages for the transfer, and
  - any cryptographic information necessary to control the received `ZEC` with their spend authority,
- and that the transfer occurred and is finalized.

In traditional Zcash shielded transfers, this data is all encrypted and posted globally to the blockchain which provides global data availability to all wallets. This violates the principle of unifying scalability, data availability, and privacy by sacrificing the first for the latter two.

In Zcash MSPA, by contrast, a sending wallet transmits the payload directly to the recipient's _Wallet Reception Provider_ (aka WRP), which enables scalability by leaving all data specific to sender and recipient out of global state tracking.

#### WRP Requirements

A WRP must:

- be highly available to receive transfers at any time,
- persist an incoming transfer in the receiving wallets WDAP,
- submit any necessary data to the _consensus layer_ to ensure it is finalized,
- then (and only then) notify the receiving wallet at the earliest possible time.

This design is aimed at achieving two essential goals:

- First, by being highly available, a WRP enables transfers to any recipient at any time, regardless of if the recipient's wallet UI is active.
- Second, by persisting the payload in the recipient's WDAP prior to finalizing a transfer, the WRP & WDAP ensure _consistency_ of the payment protocol by avoiding the case where consensus finalizes a transfer that cannot be retrieved by a receiving wallet.

### Network Privacy Providers

As defined in [What is Safety?](#what-is-safety), the system must provide Privacy by Default. The Zcash shielded payment protocols provide best-in-class privacy for ledger history. However, those protocols do not encompass network privacy. Without network privacy, various metadata is exposed to arbitrary third parties outside the awareness of senders or receivers which violates the goal of Privacy by Default.

Therefore, the Zcash MSPA requires strong network privacy which comes from _Network Privacy Providers_ (aka NPPs).

#### NPP Requirements

A NPP must:

- be highly available to facilitate transfers and finalization at any time,
- enable a sender to deliver a transfer payload to a recipient's WRP,
- protect both sender and recipient network location from all parties, including each other or any other party,
- protect, as well as possible, timing metadata so that no party may distinguish some senders from others based on timing information at the consensus layer.

**Implications:**

Because NPP's must enable a sender to deliver to a specific recipient's WRP at any time, the recipient's address must encode sufficient information to enable the sender's wallet and the NPP to achieve this.

## Architecture Characteristics

### Modularity

Because the Zcash MSPA distinguishes infrastructure into specialized provider roles, each role can be developed in a decoupled fashion from the others, constrained only by their respective _interfaces_.

In addition to the protocol for a given role evolving in a decoupled fashion, alternative designs for a given role can be simultaneously deployed. For example, a Zcash MSPA blockchain might be in operation with two or more distinct Network Privacy Provider protocols, any number of Wallet Data Availability or Wallet Reception Providers, and so on.

It is even the case that a Zcash MSPA blockchain _may_ have multiple Consensus Layer Provider protocols provided there is appropriate mechanisms to protect overall consensus of `ZEC` scarcity and economic constraints!

### Sustainability

In order for a Zcash MSPA blockchain to remain reliable and resilient, all of the provider roles must be economically sustainable. The specifics of how each provider role can be sustainable may depend on the particular provider protocol designs. Here we offer two broad classes of mechanism that may facilitate reliability and resilience through economic sustainability:

- Pay for Use: in this model, participants pay providers "directly" or "in-band" to ensure their economic sustainability. An example of this would be a payment protocol that ensures NPPs, WRPs, and WDAPs are paid by a sender only when a recipient receives a transfer.
- Consensus Payments: in this model, the consensus rules issue payments to various service providers as part of its issuance (or _Posterity Fund disbursements_, see [Long-term sustainability with the Zcash Posterity Fund](https://electriccoin.co/blog/long-term-sustainability-with-the-zcash-posterity-fund/)).

Each approach presents significant challenges. Two common challenges to both approaches is ensuring the payments are sufficient to economically sustain the providers while also being low enough to encourage adoption and usage by end users.

Some challenges for Pay for Use include ensuring that the payment protocol:

- pays only the correct providers, or pays the most productive providers on average,
- does not violate Privacy by Default safety,

A key challenge for Consensus Payments is the constraint that the consensus protocol must make payments contingent on its ability to verify a provider is correctly providing a service.
