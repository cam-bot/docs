---
description: >-
  How THORChain enables interest-bearing synthetic assets with no IL and with
  single asset exposure.
---

# Synthetic Asset Model

THORChain synthetic assets are primitives for both higher-order financial features, as well as fully-secured, fully-backed synthetic assets that can be sent anywhere in the Cosmos IBC ecosystem and they will always retain the guarantee they can be redeemed for the underlying.  

## Synthetic Assets 

THORChain synthetics are unique in that they are 50% backed by their own asset, with the other 50% backing being provided by RUNE. This is achieved by using pool ownership to collateralise the synth, which ensures always-on liquidity and pricing. 

### Minting

Synthetic Assets are created by adding Rune to a pool \(or swapping from an asset into RUNE, then adding that\) for a synthetic asset of that pool. This is known as Minting.

$$
synthAmount = (r * R * A)/(r + R)^2
$$

* r = rune deposited
* R = Rune Depth \(before\)
* A = Asset Depth \(before\)



The total Synth Supply is updated;



$$
synthSupply += synthAmount
$$

SynthUnits the rune collateral value that needs to be kept in the pool. They are computationally derived at any point, this ensures there is only enough at any time to represent the outstanding supply.

$$
runeValueOfSynths = S*(R/A)
$$

* S = Synth Supply
* R = Rune Depth
* A = Asset Depth

The Rune Collateral Value of Synths is made up 50% asset and 50% rune within the pool. Synth units represent the rune portion. 

$$
runeCollateralOfSynth = runeValueOfSynths/2 = ((S*R)/A)/2 = (S*R)/(2*A)
$$

SynthUnits are issued to cover the new liquidity, but held by the pool \(not owned by anyone\). PoolUnits are therefore the sum of [liquidityUnits ](continuous-liquidity-pools.md#calculating-pool-ownership)+ synthUnits.

Synthetic Assets Minting is capped to 33% of the assets in the pool, or about 16.5% of the pool depth. Minting assets increases the total RUNE pooled amount, which cannot be greater than the total bonded.

### Redeeming 

Synthetic Assets are burned by swapping the Synth for Rune. This is known as Burning or Redeeming. A Synthetic Asset can be redeemed to Rune at any time \(or swapped to Rune than to an asset\).

Synth Assets hold the value to normal assets and can be redeemed 1:1. Thus swapping 1 BTC for Rune then minting Synthetic BTC will give 1 Synthetic BTC. This can later be redeemed to Rune and swapped for 1 BTC, excluding fees.

$$
runeAmount = (s * A *  R)/(s + A)^2
$$

* s = Synths to Redeem
* R = Rune Depth \(before\)
* A = Asset Depth \(before\)

Pool Units, Synth Supply and Rune Pool Depth are correspondingly decremented. 

### Swapping

Synth Swaps can be done as follows:

* Layer 1 to Synth: L1 in, Rune moved over to the pool, synth MINTED
* Synth to Layer 1: Synth REDEEMED, RUNE moved to next pool, Layer 1 swapped out
* Synth to Synth: Synth REDEEMED, RUNE moved over to the pool, synth MINTED

To specify the destination asset is synth, simply provide a THOR address. If it is Layer 1, then provide a layer 1 address, e.g. a BTC address.

### Economic Reasoning

Synth holders do not experience any gain or any loss caused by price changes when minting / redeeming a synth. They do however, pay a slip-based fee on entry or exit and tx fees. Synth minting, swaping and redeeming pay half the normal slip-base fee. 

The dynamic synth unit accounting is to ensure that gain or loss caused by price divergence in the synths is quickly realised to LPs. Although since LPs have IL Protection, as long as they stay for longer than 100 days, the Protocol Reserve is taking on the price risk.

Due to the collateralisation method, THORChain Synthetic Assets are impervious to impermanent loss and offer single asset exposure.

Synth minting, swapping and redeeming will get a 50% reduction to the slip fee compared to swapping assets to incentivise the use of Synths. 

### Staged Pools

If an active pool that minted synths becomes staged, then swaps are disabled. However, synth holders can always redeem for RUNE, or the underlying asset, by specifying that on the way out.

## Synthetic Rune 

Synthetic Rune, known as iRune, can be minted from Rune and redeemed to Rune in the same way as normal Synthetic Assets. iRUNE also has the same features such as no IL and single asset exposure. 

iRune is not minted by adding to a specific pool, instead, it is minted by adding to ALL the pools at once as a form of "global collateral". This is done by giving each pool a dynamic RUNE depth, which depends on real RUNE balance, as well as the "virtual" RUNE balance, which is a global number, but pro-rata allocated to each pool.

Minting and Redeeming of iRune is done against all pools, not just one.

## Synthetic Vault

Gives yield to synth holders in the way of a savings account where synths can be locked to earn interest by "Savers". This interest is fixed and is paid by the protocol. 

Principle can be Locked to earn yield and Unlocked to access the interest. The yield is collected by the Saver and added to their deposit each time they Lock or Unlock.

