This program is a recommendation engine for lightning network node operators.

It analyzes the public network graph and makes recommendations to which channels
a node operator should open channels.
It will also suggest the capacity of the channels that are recommended.

Currently the library does not use any private information like previous routing
successes or failures.

## lib_autopilot.py is the core of the project:

lib_autopilot is a library which based on a networkx graph tries to 
predict which channels should be added for a new node on the network. The
long term is to generate a lightning network with good topological properties.
This library currently uses 4 heuristics to select channels and supports
two strategies for combining those heuristics.

1.) Diverse: which tries to to get nodes from every distribution
2.) Merge: which builds the mixture distribution of the 4 heuristics

The library also estimates how much funds should be used for every newly
added channel. This is achieved by looking at the average channel capacity
of the suggested channel partners. A probability distribution which is 
proportional to those capacities is created and smoothed with the uniform
distribution. 

The 4 heuristics for channel partner suggestion are: 

1.) Random: following the Erdoes Renyi model nodes are drawn from a uniform 
distribution
2.) Central: nodes are sampled from a distribution proportional to the 
betweeness centrality of nodes
3.) Decrease Diameter: nodes are sampled from distribution of the nodes which 
favors badly connected nodes
4.) Richness: nodes with high liquidity are taken and it is sampled from a 
uniform distribution of those

## Who uses lib_autopilot
* Currently c-lightning by blockstream maintains a plugin which uses lib_autopilot.
Checkout [their repository](https://github.com/lightningd/plugins/tree/master/autopilot)
* Electrum has showed interest in using lib_autopilot for their lightning network
implementation

## other autopilots:
* lnbig has published a javascript autopilot: https://gist.github.com/LNBIG-COM/dfe5d25bcea25612c559e02fd7698660
* lnd uses an autopilot in their software. Someone could write a go wrapper for lib_autopilot.py

## Future work

The library is supposed to be extended by a simulation framework which can 
be used to evaluate which strategies are useful on the long term. For this
heavy computations (like centrality measures) might have to be reimplemented
in a more dynamic way. 
Also it is important to understand that this program is not optimized to run
efficiently on large scale graphs with more than 100k nodes or on densly 
connected graphs.
The programm needs the following dependencies:
pip install networkx numpy pylightning
