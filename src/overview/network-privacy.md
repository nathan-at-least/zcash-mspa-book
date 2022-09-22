# Network Privacy Providers

As defined in [What is Safety](../introduction/what-is-safety.md), the system must provide Privacy by Default. The Zcash shielded payment protocols provide best-in-class privacy for ledger history. However, those protocols do not encompass network privacy. Without network privacy, various metadata is exposed to arbitrary third parties outside the awareness of senders or receivers which violates the goal of Privacy by Default.

Therefore, the Zcash MSPA requires strong network privacy which comes from _Network Privacy Providers_ (aka NPPs).

## NPP Requirements

A NPP must:

- be highly available to facilitate transfers and finalization at any time,
- enable a sender to deliver a transfer payload to a recipient's WRP,
- protect both sender and recipient network location from all parties, including each other or any other party,
- protect, as well as possible, timing metadata so that no party may distinguish some senders from others based on timing information at the consensus layer.

**Implications:**

Because NPP's must enable a sender to deliver to a specific recipient's WRP at any time, the recipient's address must encode sufficient information to enable the sender's wallet and the NPP to achieve this.
