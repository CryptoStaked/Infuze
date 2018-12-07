# Infuze

Infuze is a blockchain project that I've been mulling over in my head for a while now, and although I plan to keep specifics private for the time being, I will be taking and posting personal notes here for future reference and critique. The reference implementation will use a PoS (Proof-of-Stake) consensus and reward system.

# Proof of Stake

## Why PoS?

With my current and future plans for this project, and the current technological movement I see taking place in the cryptocurrency world, in particular reference to mining, it's my opinion that a PoS system is the fairest and easiest to implement solution for consensus and reward in my reference implementation of Infuze.

## Block Rewards

I will preface this by stating that I am at the moment nowhere near a final decision on distribution of reward funds. I am currently leaning towards a reward and fee distribution structure, whereas the block reward will entirely, or majoritively, be granted to that block's producer, and the fees for transactions between block production and previous block production will be granted to stakers relative to age and amount. I would ultimately prefer a distribution that guarentees a minimum interest rate relative to time spent staking.

## Who Produces Blocks?

This is a topic I've spent a lot of time brainstorming over, as I believe it's the biggest hurdle to overcome in regards to fairness and security in a PoS system. There have been many solutions that I have studied in the past, and it's my opinion that an age based system with a weighted authority is best. In this system, a target time and a drift period are both specified at blockchain genesis as constants. A block producer is chosen based on a pseudo-random selection algorithm from the oldest X stakers (whom have been staking for a minimum period of time) at the time leading up to the target block time. The oldest X stakers can each then attempt to produce a new block, and if the chosen producer produces a block within the drift period of the target block time, then that block is valid. If they miss this period and produce a block outside of the specified drift period, they are given a mark internally, and production is handed off to the next valid producer as chosen by the algorithm. When deciding a producer from an arbitrary set of stakers, the algorithm used weights these marks against them to determine the likelihood of being selected.

## What About Initial Distribution?

Because of the way Infuze is set up to operate, distribution is already decentralized at the time of genesis.

# Transactions

## Transaction Types

Within Infuze, with te PoS structure specified about, there will exist three transaction types, which are:

i. **Transfer** - The most common type of transaction, from one keyset to another; a direct re-mapping of input ownership.

ii. **Coinbase** - This transaction type occurs when a block is produced and a reward value is split into a set of coinbase inputs.

iii. **Stake** - This transaction type asserts that a keyset is putting a set of one or more inputs that have ownership over up for stake. This transaction type specifies no outputs, and inuts are not re-mapped. An unlock time is specified at the time of staking, and these inputs are locked until this time is exceeded.

## Coinbase Transactions

When a block is prouced with an associated reward, this reward is split into a set of smaller units, each of which is marked as a coinbase input, and these inputs are then rewarded to the chosen staker(s). This transaction type is signed by the block itself with a one-time generated keyset.

## Inputs, Outputs, and Storage

An input is a verifiable mapping of value tracing back to a coinbase transaction signed by a block on the network. When a transfer transaction is made, the given outputs supply a destination and amount. Inputs owned by the sender are then added up until they are able to reach the value of each specified output, plus a network fee if applicable. These inputs, when verified, are then re-mapped to be owned by the destination key. Inputs are verifiable, outputs are roadmaps. In the future, I may implement ring signatures into the project, but would have to think about staking and authority in a different way to achieve that.

# Templates

## Genesis Template

```
(Coming Soon)
```

## Block Template

```
(Coming Soon)
```

## Transaction Template

```
(Coming Soon)
```

## Input Template

```
(Coming Soon)
```

## Output Template

```
(Coming Soon)
```
