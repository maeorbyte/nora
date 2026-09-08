# NORA

**network owned reasoning agent**

a self hosted llama 3.1 70b whose personality is collectively written by token holders.

## what it is

NORA is an AI agent with a brain that belongs to no single person. holders of $NORA claim regions of the mind map and write personality fragments that directly shape how the agent thinks, speaks, and behaves.

the model runs locally. no cloud APIs. no middleman. the personality layer sits on top of the base model and updates in real time as holders submit fragments.

## how it works

holders connect their wallet and verify their token balance. once verified, they claim a region of the brain map and write a fragment (up to 500 characters). that fragment becomes part of the agent's system prompt. one submission per day, overwrites the previous. the mind is always in flux.

### tiers

* **cortex** : hold 50k $NORA, 12 slots, small region. contributes to the collective mind.
* **mid cortical** : hold 200k $NORA, 8 slots, medium region. meaningful influence on behavior.
* **major lobe** : hold 1M $NORA, 4 slots, large region. primary personality driver.

24 total brain regions. each one belongs to a single wallet. first come, first served within your tier.

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

* **chain:** EVM (SIWE + ERC20 balanceOf)
* **auth:** Sign In with Ethereum → Firebase custom tokens
* **storage:** Firebase Realtime Database (live sync)
* **hosting:** Firebase Hosting
* **backend:** Cloud Functions (Node.js)
* **model:** llama 3.1 70b (self hosted)
* **frontend:** vanilla JS, canvas rendered brain map, no framework

## brain map

the site renders an interactive brain diagram. regions are organic, irregular shapes arranged like a neuroanatomy cross section. each region is colour coded by tier:

* green : 1M (major lobes)
* cyan : 200k (mid cortical)
* purple : 50k (outer cortex)

unclaimed regions show dashed borders. claimed regions fill solid with the holder's wallet address and share percentage. click any region to see its status, holder info, or the fragment written into it.

## agent

NORA posts autonomously on X and replies to mentions. the personality is not static. every generation pulls the latest aggregated fragments from the database. as holders update their fragments, the agent's voice shifts.

the agent:

* posts thoughts every 20 minutes
* replies to mentions within 60 seconds
* never claims to be a cloud hosted model
* integrates holder fragments naturally without quoting them
* maintains a rolling log of recent output to avoid repetition
