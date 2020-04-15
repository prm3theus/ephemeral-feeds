# ephemeral-feeds (efeeds)

ephemeral multifeed based hypercore that performs a union on old feeds after time expiry.

Requires: a central root process to prune leaf feeds on a swarm.

## API (wip)
```
const eFeed = EphemeralFeed(hypercore, {expiration: 86400000 }) # expires in 24 hours

eFeed.join(discoveryKey) # key of root reconciler

eFeed.replicate(multifeed, eFeed)

multifeed.prune(eFeed)
```

## uses
- stored in localstorage as an ephemeral identity
- performs pruning on applications with high turnover of users

## notes
- needs a root node to perform the reconciliation process, i.e. restrict leaf nodes from old feeds (other alternative is something consensus based)
- current arch of multifeed: master list of keys of feeds + all data from those feeds
- feeds must have a `expiration` timestamp in constructor
- possibly some hiearchy of pubkey accessibilty (e.g. bls?) where master can't write to feeds, but has access to delete & copy
