---
title:  "Out-of-distribution(OOD) generalization"

categories:
  - project-ood
tags:
  - GNN
  - machine learning
 
use_math: true
---

### Experiment 1. Generalization across graph topologies

1. **Structured graphs**

![img](https://paper-attachments.dropbox.com/s_9CDAC1F5BF293DE3A98D349DDB337452C1DCE34D63AD0A4072478C05CD9DF560_1611725134009_image.png)

- Observations
  - The small basin around each training graph (red triangle in Fig. 1) along the axis of network topology shows that GNNs generalize best to the networks of the same topology with a training dataset.
  - Crucially, however, the size of the basin varies with what network topology GNNs were trained on, suggesting that 15 structured graphs in this experiment are not likely to be of equal distance from each other in contrast with their arrangement in Figure 1. 
  - We next examine the generalization pattern by training GNNs on graphs from coupling strengths of wider distributions and find that, notably, the topology-dependent predictive pattern is preserved (three color codes in Figure. 1), although the performance degrades as $\sigma_{J}$ increases and the pattern becomes less prominent at stronger couplings, possibly due to being in the hard inference regime at strongly coupled networks.
  - We hypothesize that how far GNNs could generalize to relies on the proximity between training and test graphs in a latent graph topological space.
  - In an attempt to address this, we explore the space of random graphs to single out the effects of various graph properties.
  
2. **Random graphs**

<img src="https://paper-attachments.dropbox.com/s_9CDAC1F5BF293DE3A98D349DDB337452C1DCE34D63AD0A4072478C05CD9DF560_1612713725091_file.png" alt="img" style="zoom:50%;" />

<img src="https://paper-attachments.dropbox.com/s_110D05CA50351F4DBCA160181BD053E1B0EA1176B24AA3F2756A511D9A70D9D6_1614148440482_file.png" alt="img" style="zoom:50%;" />

- 1st & 2nd column: loss(NodeGNN) $\approx$ loss(MsgGNN) ← Low
- 3rd column: loss(NodeGNN) < loss(MsgGNN)
- 4th column: loss(NodeGNN) > loss(MsgGNN)
- 5th column: loss(NodeGNN) $\approx$ loss(MsgGNN) ← High

- Observations
  - Generalizability
    - (Probably) All GNNs typically generalize to the graphs that matches the average unique degree of the training data, which is characterized by the vertical blue band in PCA.
    - Repeat the same experiment $\|V\|$=16 & 100 by adding “skip-connection” to see if the band-like generalization pattern is indeed the signature of the attention module only
    - Skip-connection does not show band-pattern. 
    - *Mean(degree)* of train/test dataset are similar. There are little deflection in column 5 probably caused by lack of test graph data.

- Trainability
  - (Figure 2) The performance of attention model (pale blue in 2nd row) looks worse than the rest (solid blue in 1st & 3rd rows), and wonder if it is statistically significant. Or is it due to the color range in a color bar?
  - Reinterpret Figure 1 given the lesson in random graph experiments.

### Experiment 2. Generalization across graph sizes

1. **Random graphs**
   - Attention network show most strict band-like generalization pattern. Generalization performance of NodeGNN, however, outperforms the others over entire test graph.
   - Degree distribution of train graph set(especially mean value of degree) play a key role in generalization on test graph set even graph sizes are different.
     - GNN training on $\|V\|$=16, $\sigma_{J}$ = 0.3, $\sigma_{b}$ = 0.25
![img](https://paper-attachments.dropbox.com/s_110D05CA50351F4DBCA160181BD053E1B0EA1176B24AA3F2756A511D9A70D9D6_1617005109412_Unknown.png)

   - GNN testing on $\|V\|$=36, $\sigma_{J}$ = 0.3, $\sigma_{b}$ = 0.25
![img](https://paper-attachments.dropbox.com/s_110D05CA50351F4DBCA160181BD053E1B0EA1176B24AA3F2756A511D9A70D9D6_1617005387118_Unknown.png)

   - GNN testing on $\|V\|$=100, $\sigma_{J}$ = 0.3, $\sigma_{b}$ = 0.25
![img](https://paper-attachments.dropbox.com/s_110D05CA50351F4DBCA160181BD053E1B0EA1176B24AA3F2756A511D9A70D9D6_1617005984667_Unknown.png)



### Experiment 3. Generalization across network coupling strengths

1. **Random graphs**
   - GNN training on $\|V\|$=16, $\sigma_{J}$ = 0.75/0.6, $\sigma_{b}$ = 0.25
   - GNN testing on $\|V\|$=16, $\sigma_{J}$ = 0.3, $\sigma_{b}$ = 0.25


### Experiment 4. 
1. Training on multiple groups of graphs
2. Inductive learning
   a. Show statistics of graph properties in real data
   b. Figure out if the past research on real data is consistent with our findings
   c. If yes, hopefully, reinterpret why a model/task was (not) successful

3. Transductive learning
   a. Figure out if our finding can be extended to the transductive setting in the same marginal prob. datasets
   b. If yes, test on real data

4. Virtual Node (https://paperswithcode.com/paper/neural-message-passing-for-quantum-chemistry#code)

5. Regularization
   - Place dropout/batchnorm1D layer before ReLU activation in every 2 layer MLP(Output, message function). 
     1. Dropout
        - MLP : Linear → Dropout(p = 0.1) → ReLU → Linear → Dropout(p=0.5) → ReLU → Linear

     2. BatchNorm(Only in Output function)
        - MLP : Linear → BatchNorm1D → ReLU → Linear → BatchNorm1D → ReLU → Linear

| **Graph Topologies** **(GT)** | **Graph Features** **(GF)**$\sigma_{J}$**,** $\sigma_{b}$ **= 0.****25** | **Graph Sizes** **(GS)**$\|V\|$ | **(train config) →** **(test** **config)** |
| ----------------------------- | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------ |
| B*inary* *tree*               | $\sigma_{J}$ = 0.3 (green)                                 | $\|V\|$=16                      | (GT1-5, GF1-3, GS1) → (GT6)                |
| G*rid*                        | $\sigma_{J}$ = 0.6 (blue)                                  | $\|V\|$=36                      | (GT1-5, GF1-3, GS1) → (GF1-3)              |
| T*rigrid*                     | $\sigma_{J}$ = 0.9 (orange)                                | $\|V\|$=64                      | (GT1-5, GF1-3, GS1) → (GS1-4)              |
| B*ipartite*                   |                                                              | $\|V\|$=100                     |                                            |
| Four above                    |                                                              |                                   |                                            |
| Every 15 topology             |                                                              |                                   |                                            |
