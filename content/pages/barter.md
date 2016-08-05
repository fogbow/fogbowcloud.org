Title: Barter of wares
url: barter
save_as: barter.html
section: arch
index: 4

## The Network of Favors

Fogbow implements the Network of Favors (NoF) incentive mechanism, proposed by Andrade <I>et al.</I> In the NoF, the allocation of a certain amount of resources for a given amount of time is called a favor. A federation member keeps a local record of the total amount of resources provided to and received from each other member with which it has interacted in the past, which is updated from time to time, depending on the accounting granularity autonomously defined by each FM. Based on these values, each member calculates a balance of the interactions as the difference between the amount of resources received from a member and the amount of resources provided to that member. In case of resource contention, a member will always prioritize the provision of resources to the members to whom it has the highest debt, i.e. those with which it has the highest value in the balance of previously exchanged resources.

This business model was first designed and evaluated in the context of peer-to-peer desktop grids and implemented by the <a href="http://www.ourgrid.org/" target=_blank>OurGrid middleware</a>. For a complete description and assessment of this implementation, refer to <a href="http://www.ourgrid.org/4.2.6/index.php/pt/research" target=_blank>OurGrid’s publications</a>. In particular, have a look at <a href="http://www.sciencedirect.com/science/article/pii/S0743731507000706" target=_blank>"Automatic grid assembly by promoting collaboration in peer-to-peer grids"</a>. In addition to the scientific publications describing this prioritization mechanism, there are also others that describe improvements that have been proposed for the particular context of the federation of private IaaS clouds. Please refer to the <a href="research.html">Research</a> section for further details.
