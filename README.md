# ephemeral-feeds (i.e. efeeds)

hypercore based ephemeral multifeeds that performs a union on old feeds after time expiry.

Requires: a central root process to prune leaf feeds in a swarm.

## api (thinking out loud)
```
const eFeed = EphemeralFeed(hypercore, {expiration: 86400000 }) # expires in 24 hours

eFeed.join(discoveryKey) # key of root reconciler

eFeed.replicate(multifeed, eFeed)

multifeed.prune(eFeed)
```
## tradeoffs
- point of centralization vs. distributed & resillient apps (can be smoothed with multiple root processes)
- opens up feed / swarm to spamming and clogging other important app processing
- longer lag time between 'up-to-date' version of feeds due to increased processing time to prune/replicate
- to boost load speed of application, consider max # of feeds vs. time for expiration

## uses
- eFeed stored in localstorage as an ephemeral identity
- performs pruning on applications with high turnover of users

## notes
- needs a root node to perform the reconciliation process, i.e. restrict leaf nodes from old feeds (other alternative is something consensus based)
- current arch of multifeed: master list of keys of feeds + all data from those feeds
- feeds must have a `expiration` timestamp in constructor
- possibly some hiearchy of pubkey accessibilty (e.g. bls?) where master can't write to feeds, but has access to delete & copy
- could have a feature to perform a reduce / map / filter duing reconcillation process, whereby data becomes truncated e.g. something like: https://github.com/kappa-db/kappa-view-kv
- could store feed of data on IPFS or seperate swarm, then have entry of where to find data
- if root reconciler is down, consider distributed alternatives / max capacity of feeds for optimal resiliency

## tests
- [ ] what does ddos look like? e.g. programmatic web crawling
- [ ] leaf node implementation (client side)
- [ ] root node implementation (server / reconciler)
- [ ] simple working leaf + root = ephemeral feed
- [ ] speed of root reconciler vs. # of feeds processed (explore capacity)

