# Sustainability

In order for a Zcash MSPA blockchain to remain reliable and resilient, all of the provider roles must be economically sustainable. The specifics of how each provider role can be sustainable may depend on the particular provider protocol designs. Here we offer two broad classes of mechanism that may facilitate reliability and resilience through economic sustainability:

- Pay for Use: in this model, participants pay providers "directly" or "in-band" to ensure their economic sustainability. An example of this would be a payment protocol that ensures NPPs, WRPs, and WDAPs are paid by a sender only when a recipient receives a transfer.
- Consensus Payments: in this model, the consensus rules issue payments to various service providers as part of its issuance (or _Posterity Fund disbursements_, see [Long-term sustainability with the Zcash Posterity Fund 🡕](https://electriccoin.co/blog/long-term-sustainability-with-the-zcash-posterity-fund/)).

Each approach presents significant challenges. Two common challenges to both approaches is ensuring the payments are sufficient to economically sustain the providers while also being low enough to encourage adoption and usage by end users.

Some challenges for Pay for Use include ensuring that the payment protocol:

- pays only the correct providers, or pays the most productive providers on average,
- does not violate Privacy by Default safety,

A key challenge for Consensus Payments is the constraint that the consensus protocol must make payments contingent on its ability to verify a provider is correctly providing a service.
