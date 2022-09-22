# Consensus Layer Providers

Consensus layer providers (aka CLPs) are responsible for protecting the scarcity of `ZEC` and implementing economic constraints to ensure reliability and resilience which are necessary for safety.

In the Zcash MSPA, the _only_ interaction between users and consensus is these two mechanisms:

- A recipient's _Wallet Reception Provider_ (see [Wallet Reception](./wallet-reception.md)) must be able to submit a _finalization payload_ to the consensus layer.
- Sender and recipients must learn when transfers they are parties to become finalized.

**[TODO / BUG]** How do senders and recipients learn that their transfer has been finalized?

To achieve the goal of Privacy by Default _only_ the senders and recipients which are parties to a transfer are able to learn when the relevant transfer becomes finalized.
