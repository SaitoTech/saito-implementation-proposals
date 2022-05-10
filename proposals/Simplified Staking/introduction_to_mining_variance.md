**Description**:

The expected time for Bitcoin miners to find a single block is 10 minutes. This is just an average: sometimes the network will find several blocks in quick succession, and sometimes it will take a much longer time before miners stumble onto a single valid block. The algorithm that lets us calculate what percentage of Bitcoin blocks will be found after an arbitrary time_in_minutes is:

    exp( ( -1 * time_in_minutes ) / blocktime_in_minutes )

This lets us calculate that roughly 13 percent of Bitcoin blocks are produced more than 20 minutes *after* the previous solution is found, while 0.24 percent take an hour to be found. But regardless of how much time and energy has already been spent searching for the next block, in Bitcoin our expected time to find a solution remains 10 minutes at every single point in time. If the network has already spent 50 minutes in fruitless hashing, we still expect to find the next block somewhere around the 60 minute mark.

The important thing to note here is that the difficulty of hashing in Bitcoin provides an expected average time for block production which remains constant no matter how much hashing has been done in the past. It does not provide any non-probabilistic guarantee that any particular block will be produced within any timeframe.

Saito’s golden ticket mechanism has similar properties because it also uses a hashing process. We can set a target difficulty that guarantees an average of N golden tickets per M blocks, but we cannot guarantee that any arbitrary chain of M blocks will contain N golden tickets. It is not a problem for Saito if we produce too many golden tickets for any block as only one may be included in the blockchain. But the natural variance at which golden tickets are produced means that some blocks will always go unsolved. Saito’s staking mechanism is intended to address the problems this creates.