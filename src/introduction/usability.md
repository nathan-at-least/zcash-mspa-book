# Usability

The usability model for Zcash MSPA is as follows:

- Users may install and utilize a variety of _wallets_. More options are generally better.
- A user can freely create as many _addresses_ as they need or desire at no cost.
- Each address controls a distinct set of `ZEC` funds.
- Each address is controlled entirely by a _spend authority_: anyone with knowledge of this _spend authority_ can observe an address's entire history and spend any funds.
- Each address has an associated _viewing key_ which allows anyone with knowledge thereof to view the entire history and future interactions of the address. However, a viewing key _cannot_ spend any funds held by an address or effect any other changes. Anyone with knowledge of a spend authority can derive the associated viewing key. It is not possible to determine the spend authority given a viewing key.
- Given a destination address, a wallet holding a spend authority can initiate a transfer of funds controlled by that spend authority to be received by the destination address. When a sending wallet initiates a transfer, it generates all necessary information locally _non-interacitvely_[^1], then it interacts with the Zcash MSPA to effect the transfer.
- When such a transfer is initiated, it will either become _finalized_ by the protocol, or it will _fail to finalize_ in a known limited amount of time.
- The protocol futher requires various _fees_ to enable reliability and resilience which are requirements of safety. [**TODO:** Flesh out how fees relate to MSPA better.]

**Wallet Data Availability:** In a crucial departure from modern Zcash and many cryptocurrencies, the Zcash MSPA _does not_ directly support data availability for wallets!

This means that wallets _bear the sole responsibility_ for ensuring:

- received funds are available to be spent
- history is preserved
- restoring from a backup is successful

Wallets may rely on one or more _wallet data availability providers_ to achieve this goal, as described in the [Wallet Data Availability](../overview/wallet-data-availability.md) section.

[^1]: The requirement for sending to be non-interactive is tenuous, because this may be a fruitful requirement to relax to better achieve a balance of overall goals.
