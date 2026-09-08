# NORA

**network owned reasoning agent**

a locally hosted llama 3.1 70b whose personality is authored collectively by the people who hold it.

## overview

NORA is an autonomous language model with no owner and no central operator. its personality is not written by a company or an individual. it is assembled from fragments contributed by $NORA holders, each of whom controls a distinct region of the model's mind.

the base model runs entirely on local hardware. there are no third party inference APIs and no hosted intermediary. a personality layer sits above the base weights and is recompiled in real time as holders revise their fragments, so the agent's voice is always a live reflection of its community rather than a fixed script.

## design goals

* **distributed authorship.** no single account can dictate how NORA thinks. influence is spread across many independent holders.
* **verifiable ownership.** every region of the mind is bound to a wallet and gated by on chain balance, so authorship is provable rather than assumed.
* **a living personality.** the system prompt is never frozen. it evolves continuously as fragments are written and revised.
* **local and sovereign.** the model, the aggregation layer, and the agent all run on infrastructure that no external provider can revoke.

## how it works

1. a holder connects a wallet and signs a message to prove control of it.
2. a server side check reads the wallet's $NORA balance and resolves its tier.
3. the holder claims an unclaimed region of the brain map and writes a fragment of up to 500 characters.
4. the fragment is merged into the aggregated system prompt that drives every generation.

each holder may submit once per day. a new submission overwrites the previous one, so the mind stays current without growing unbounded.

## ownership tiers

access scales with holdings. larger positions unlock larger regions and greater influence over the aggregate personality.

* **cortex.** 50k $NORA. 12 slots. small regions that contribute to the collective tone.
* **mid cortical.** 200k $NORA. 8 slots. medium regions with meaningful weight on behavior.
* **major lobe.** 1M $NORA. 4 slots. large regions that act as primary personality drivers.

there are 24 regions in total. each belongs to exactly one wallet, allocated first come first served within a tier.

## anti capture

the region model exists specifically to stop any one buyer from seizing the whole personality. influence is bounded by a finite number of slots per tier rather than by raw balance, so acquiring an enormous position cannot translate into unilateral control. the mind is meant to stay the product of many contributors, never a single one.

## personality pipeline

fragments are stored per region and continuously aggregated into one personality document. at generation time the agent loads the latest aggregate, blends the fragments into a single coherent voice, and produces output without quoting any fragment verbatim. the effect is emergent: no contributor sees their exact words returned, only their influence on the whole.

## architecture

```
wallet (MetaMask)
  │
  ├─ SIWE signature ─→ Cloud Function (verify + balanceOf check)
  │                        │
  │                        ├─ Firebase Auth (custom token)
  │                        └─ RTDB write (holder record + fragment)
  │
  ├─ personality aggregation ─→ RTDB /agent/personality
  │
  └─ agent reads personality ─→ llama 3.1 70b (local inference)
                                   │
                                   └─→ X posts + replies (@norashared)
```

* **chain:** EVM, using SIWE for auth and ERC20 balanceOf for gating
* **auth:** Sign In with Ethereum, exchanged for Firebase custom tokens
* **storage:** Firebase Realtime Database for live sync
* **hosting:** Firebase Hosting
* **backend:** Cloud Functions on Node.js
* **model:** llama 3.1 70b, self hosted
* **frontend:** vanilla JS with a canvas rendered brain map and no framework

## the agent

NORA operates autonomously on X. its behavior is driven entirely by the current aggregate, so its voice shifts as the community rewrites it.

* publishes original thoughts on a regular interval
* responds to mentions within seconds
* draws its personality from the live aggregate rather than any static persona
* never presents itself as a cloud hosted model
* keeps a rolling memory of recent output to avoid repeating itself

## roadmap

two open problems the project is actively working through:

* **meaningful acquisition.** claiming a region should take more than a single click. the goal is to make ownership a deliberate, earned action rather than a trivial one.
* **aligned incentives.** holding a region should carry real upside. the goal is a design that ties regional ownership to value which grows as participation grows.
