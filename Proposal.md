# Infuze

## Summary

Infuze is a blockchain project that sets out to function as a second layer over-top one (or more) existing blockchains. In doing so, it can utilize the pre-existing security of its parent chain(s) to help secure its own data, as well as utilizing the parent chain's units as a form of gas to fuel transit of data for Infuze's chain.

One long term goal for this project is to provide a venue for cross-chain talk and interaction - like a bridge connecting two sides of a river, where one side wants to send data to the other over a verifiable and linkable path.

## Initial Implementation

Infuze should be seen as a reference implementation to the greater InfuzeCore backend, which will come slowly as Infuze is further fleshed out. In the time being, the initial implementation of Infuze will begin life as a proof-of-stake second layer over-top *one* parent chain, then expanded upon into the future.

# Proof of Stake

## Why PoS?

Taking into consideration the direction I want to go with this project, I believe proof-of-stake (PoS) to be the fairest solution for consensus, and believe it will provide the most forward momentum for the chain itself. PoS encourages and rewards consistency and makes it accessible to *more* people than mining alone does, as it requires less processing power, and instead, makes use of a node's punctuality in regards to having the correct data at the correct time.

## Block Rewards

As the Infuze network does not have fees of its own (instead relying on its parent network's units as a form of gas), block rewards are simplified, and determined based on the following:

```
(TotalUnits - AlreadyGeneratedUnits) >> EmissionFactor
```

From here, the reward is split between that block's producer and the valid stakers for that period.

## Who Produces Blocks?

One of the biggest hurdles in keeping a PoS system fair is deciding *who* produces the next block. After researching the topic for a while, I have come to the decision that an age-based system with a weighted authority is the most fair means of consensus. In this system, a target time and a drift period are both specified at blockchain genesis as constants. A block producer is chosen based on a pseudo-random selection algorithm from the oldest X stakers (whom have been staking for a minimum period of time) at the time leading up to the target block time. The oldest X stakers can each then attempt to produce a new block, and if the chosen producer produces a block within the drift period of the target block time, then that block is valid. If they miss this period and produce a block outside of the specified drift period, they are given a mark internally, and production is handed off to the next valid producer as chosen by the algorithm. When deciding a producer from an arbitrary set of stakers, the algorithm used weights these marks against them to determine the likelihood of being selected.

## What About Initial Distribution?

Because Infuze sits on top of one or more existing blockchains, we can use the pre-existing units on that chain to provide a number of Infuze units to a persion - this is done via the use of a special transaction type called a "kickback transaction." What is a kickback transaction? It's a transaction that contains no inputs or outputs, and specifies a custom fee amount. When Infuze sees a kickback transaction arrive, it takes into account the fees used to broadcast it on the parent network, and uses the following formula to reward the sender:

```
Floor((FeeUsed / ParentNetworkAlreadyGenerateUnits) * (TotalUnits - AlreadyGeneratedUnits))
```

# Transactions and Inputs

## Transaction Types

Within Infuze, there exist three types of transactions:

i. **Transfer** - The most common type of transaction, from one keyset to another; a direct re-mapping of input ownership.

ii. **Kickback** - This transaction type contains no inputs or outputs and exists solely to gain units via the kickback process.

iii. **Stake** - This transaction type asserts that a keyset is putting a set of one or more inputs that have ownership over up for stake. This transaction type specifies no outputs, and inputs are not re-mapped, and instead are locked for a period of time, then returned when this time has elapsed.

## Input Types

Similar to transactions, inputs come in different types, each of which having a different means of verification:

i. **Coinbase** - When a block is produced with an associated reward, this reward is sent to that block's producer and its stakers as a number of coinbase inputs, which can be verified against that block's information.

ii. **Kickback** - When a kickback transaction is received, the associated kickback amount is sent to the recipient as a single kickback input, which can be verified against the packet fees on the parent chain.

iii. **Transfer** - This input type is created when a coinbase or kickback input is sent in a standard transfer transaction, and can be verified against the transaction's information and historic information.

# Templates

The following are templates as they stand now, though they may change or be altered as development continues.

## Genesis Template

```
{
Block Hash (string),
Parent Hash (string),
Signature (string),
Producer (string),
Premine (uint64),
Total Units (uint64),
Decimal Places (int32),
Target (int32),
Target Variance (int32),
Emission Factor (int32)
}
```

## Block Template

```
{
Block Hash (string),
Parent Hash (string),
Signature (string),
Producer (string),
Reward (uint64)
}
```

## Transaction Template

```
{
Version (int32),
Type (int32),
Hash (string),
Public Key (string),
Transfers (int32),
Origin Hash (string),
Signature (string),
Inputs (serialized as string),
Outputs (serializes as string)
}
```

## Input Template

```
{
Type (int32),
Origin Hash (string),
Hash (string),
Index (int32)
}
```

## Output Template

```
{
Amount (uint64),
Index (int32),
Public Key (string)
}
```
