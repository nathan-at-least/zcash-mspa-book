# Modularity

Because the Zcash MSPA distinguishes infrastructure into specialized provider roles, each role can be developed in a decoupled fashion from the others, constrained only by their respective _interfaces_.

In addition to the protocol for a given role evolving in a decoupled fashion, alternative designs for a given role can be simultaneously deployed. For example, a Zcash MSPA blockchain might be in operation with two or more distinct Network Privacy Provider protocols, any number of Wallet Data Availability or Wallet Reception Providers, and so on.

It is even the case that a Zcash MSPA blockchain _may_ have multiple Consensus Layer Provider protocols provided there is appropriate mechanisms to protect overall consensus of `ZEC` scarcity and economic constraints!
