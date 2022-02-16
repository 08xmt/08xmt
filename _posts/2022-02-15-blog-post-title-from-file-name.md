## Hidden Paths and Arbitrage
Inspired by [@derked\_eth's recent article](https://twitter.com/derked_eth/status/1488330855114289152) on his adventure into the world of long tail MEV, I decided to write about my own story. I want to show that opportunities still exist for searchers, who put an emphasis on understanding protocols, not just searchers with the best black magic compute optimizations.  


This article will have a quick introduction to arbitrage, explain why most arbitrage is highly competitive, and then showcase less competitive arbitrage opportunities, based on a real world example. 

---  

### What is Arbitrage?  

*"Arbitrage is the simultaneous purchase and sale of the same asset in different markets in order to profit from tiny differences in the asset's listed price. It exploits short-lived variations in the price of identical or similar financial instruments in different markets or in different forms."* - Investopedia  

Arbitrage abound in DeFi and the broader crypto ecosystem. There exists thousands of trading pairs between different tokens, and often multiple identical pairs on different DEXes and CEXes.  

To model the problem, you can imagine each token being a node in a graph, and each trading pair a weighted edge.  

![Arbitrage Graph](https://github.com/08xmt/08xmt/blob/main/docs/assets/images/simple_arb_graph.png?raw=true)  
*A simplification of the graph. In reality, each edge would be weighted by a function taking multiple variables, such as trade size, liquidity, price curve etc.*

Big arbitrage bots look for imbalances in the grand web of trading pairs. Since tokens can have multiple pairs with other tokens, which in turn have pairs with many of the same tokens. You can imagine how many routes there are to take, and all you have to do is find a route that lands you back, at where you started, with more than you set out with.  

Now these CEX trading pairs are some of the most competitive hunting grounds for the biggest monsters of the [Dark Forest](https://www.paradigm.xyz/2020/08/ethereum-is-a-dark-forest). They tune their arbitrage smart contracts to be as gas effective as possible, and pay a majority of their profits to miners through Flashbot auctions. To be competitive here, you need to be able to reduce the execution cost of your arbitrage transaction by as much as possible, usually by writing your arb contracts directly in EVM assembly and having a large treasury of tokens. That's way too much heat for a small fish like, I'm just trying to make a little bit of money here, so I went looking elsewhere.

![Arbitrage Graph Explainer](https://github.com/08xmt/08xmt/blob/main/docs/assets/images/simple_arb_graph_explainer.png?raw=true)  
*A toy example, most arbitrage yields much smaller returns.*

### Hidden Paths
I enjoyed arcade racing games a lot as a kid. Mario Kart and Need for Speed especially. A staple of those games are the hidden shortcut. A path you take to shave a few seconds off a lap time, usually requiring specialized knowledge or skill. A similar concept exists in long tail arbitrage.  

When looking for arbitrage in the grand trading pair graph, leaf nodes are generally considered uninteresting. If there's only one way to and from a token, then you're always going to make a loss from visiting it. But not every leaf node is as isolated as it looks. This is where hidden shortcuts come in. Invisible to generalized arbitrage bots, but exploitable by more custom tooled specimens.  

![Leaf Nodes](https://github.com/08xmt/08xmt/blob/main/docs/assets/images/leaf_nodes.png?raw=true)  
*ytaUSDC and otaUSDC are leaf nodes, because they only have a single, liquid trading pair.*

Enter [Pendle. Pendle](https://pendle.finance/) is a DeFi protocol for turning variable yield tokens, into fixed yield tokens. The way the protocol accomplishes this is simple: It splits the yield generating token into two distinct tokens; a yield token(yt) representing the yield earned up until maturity, and a collateral token(ot) representing the underlying collateral. To earn a fixed yield, one can supply a yield bearing token, say Aave USDC(aUSDC), split it into ytaUSDC and otaUSDC, sell the ytaUSDC and wait until maturity to redeem the otaUSDC.  

However, there's a different mechanism for redeeming your yt and ot. If you have an equal amount of both, you can redeem them for the underlying, before maturity. Here we find our shortcut.  

![Leaf Nodes Pendle](https://github.com/08xmt/08xmt/blob/main/docs/assets/images/leaf_nodes_pendle.png?raw=true)  
*The cyan Pendle edge allows the creation of 1 ytaUSDC and 1 otaUSDC for 1 aUSDC, opening an arbitrage opportunity.*

Now, this shortcut wasn't very enticing for the Aave USDC or Compound DAI pairs. The markets moved slowly, and gas fees would eat most of the profit. Even worse was that I couldn't let the arbitrage get too big, as I noticed a couple of manual arb hunters on Etherscan. I respect the hustle. Luckily, Pendle launched a far more lucrative trade: Fixed yield on Sushiswap LP positions, mainly Pendle-Eth and Eth-USDC. Now these tokens were way more volatile than their interest bearing counterparts. This meant that imbalances moved with the market, and instead of having to wait for a steady stream of interest to cause a profitable imbalance, instead I could merely let the cryptomarket be it's volatile, lovely self, as I was the only one rebalancing this small trading pair.  

![Advanced Graph](https://github.com/08xmt/08xmt/blob/main/docs/assets/images/advanced_graph.png?raw=true)  
*Things get a little hairy once SLPs start getting traded.*

### All good things come to an end
Times were good, and at peak volatility the bot could secure arbs of multiple percent every day, even if the size of the trades were fairly limited due to the illiquidity of the markets.

Alas, nothing lasts forever and eventually competition showed up. My profits shrinked, and the other party was clearly more experienced than a first timer like me, so I abandoned the arb. Though it was definitely an profitable, educational and most importantly, entertaining little hustle.  

A few takeaways for me were:
- Plenty of small, untapped MEV opportunities still exist in the wild
- An understanding of DeFi and solidity is all you really need to get started
- Don't underestimate the surface area of opportunity created by DeFi composability
- No edge is likely to last forever

I hope you enjoyed the article, and feel free to reach out with any questions you may have!  
