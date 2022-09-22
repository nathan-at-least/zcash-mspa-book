# Wallet Data Availability Providers

Because only the sender and recipient need to know about a given transfer, this implies that a modular data availability solution can be specialized to senders and recipients, rather than global.

In Zcash MSPA this function is decoupled in a modular fashion from the rest of the architecture and provided by _Wallet Data Availability Providers_ (aka WDAPs). Each wallet is responsible for ensure data availability for wallet-specific data (which also happens to be private to the wallet user).

## WDAP Requirements

A WDAP must:

- provide the full history of an address to a wallet on demand,
- persistently store locally generated modifications of wallet state, especially sender-initiated transfer information,
- be highly available in order to receive transfers at any time (see [Wallet Reception](./wallet-reception.md)).
- persistently store remotely generated modifications of wallet state, especially incoming transfers including the `ZEC` transferred and associated messages,
- and finally, enable users to restore this data in the event that their wallet device is destroyed or otherwise needs to be restored.
