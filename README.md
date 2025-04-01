# Q1:
The paper claims to work on heterogeneous graphs, but it actually deals with heterophilous graphs. The datasets and baselines confirm this mistake.

# A1:
We appreciate the correction. While the term "homogeneous/heterogeneous graph" is used to refer to graphs without typed nodes or edges, we agree that "homophilic" and "heterophilic" more accurately reflect the semantic relationship between node features and connections. We will revise the terminology throughout the paper to reflect this distinction.

# Q2:
The method relies on balancing diffusion and feature similarity, but the paper does not provide theoretical insights into why this improves performance. What is the real novelty of FIFR? Balancing diffusion with feature similarity seems quite common—how is this different from past work?

# A2:
We appreciate the reviewer’s thoughtful feedback and the opportunity to clarify our contributions.

We propose a novel extension of the GDC framework, termed Constrained Graph Diffusion (CGD) (Section 3.1), which introduces a new mechanism to dynamically control the diffusion depth via the saturation-aware metric $\delta_t$ and constraint factor $\beta = 2K_0$. This modification directly addresses limitations of prior GDC-like methods, including over-diffusion, and inefficient global information propagation.

Only after establishing this new constrained diffusion mechanism, we integrate it with Feature Information Flow Routing (FIFR) as an enhancement module. Our goal with FIFR is to further improve feature-level guidance during diffusion, especially in graphs with complex semantics. However, the core contribution lies in the constrained diffusion formulation itself, rather than in a simple architectural combination.

Furthermore, compared to Adaptive Diffusion Convolution (ADC), CGDConv offers several key advantages:

- Parameter-free diffusion control: While ADC usually needs to learn to continuously update the parameters of each channel during the diffusion process to achieve adaptive diffusion, our method can infer the appropriate diffusion range from the graph structure itself, and no additional parameter updates are required after inserting GNNs.
- Saturation-aware adaptivity: CGDConv introduces a principled, empirical convergence criterion based on the growth of nonzero entries in the diffusion kernel $\delta_t$, providing a more transparent and theoretically grounded stopping mechanism than truncations used in ADC.
- Computational efficiency and pluggability: CGDConv avoids expensive optimization over kernel weights, making it computationally lightweight, but the ADC method model is highly coupled. This design allows CGDConv to serve as a plug-in diffusion layer compatible with diverse GNN architectures.

We agree that additional clarification on the motivation and role of FIFR would strengthen the presentation. In the revision, we will expand Section 3.2 (**👉see Figure Re.1**). We also demonstrate our method’s complexity analysis to reflect the uniqueness of the algorithm (**👉see Figure Re.2**).

Thank you again for pointing this out — your comment helps us better communicate our method.


<div align="center"><strong>Figure Re.1: Supplementary Appendix Section (D. Interpretability of FIFR).
  
<img width="536" alt="image" src="https://github.com/user-attachments/assets/f9415e60-ac3d-448e-b7b5-bd8949dc7634" /></strong></div>


<div align="center"><strong>Figure Re.2: Complexity Analysis.
  
<img width="733" alt="image" src="https://github.com/user-attachments/assets/84b0985c-8fda-4171-b0dd-0321db669419" /></strong></div>

# Q3:
How is $\beta$ chosen, and why is it the best choice? The paper does not provide any theoretical reasoning or strong justification.
# A3:
We thank the reviewer for raising the question about how $\beta$ is selected and why it is an appropriate choice.

We clarify that the constraint factor $\beta$ is not arbitrarily set, but determined based on empirical analysis of the diffusion dynamics. Specifically, as detailed in **👉Section 4.3 (Parameter Analysis - Constrained Factor $\beta$)**：

The first $K_0$ steps are necessary and sufficient for capturing global structural reachability, based on actual diffusion growth.

The second $K_0$ steps act as a buffer to enhance robustness against small estimation errors and provide stabilization margin, especially in graphs with irregular or sparse topology.

This formulation is empirical but principled: it avoids arbitrary fixed diffusion depth (as in PPR, GDC), and instead provides an adaptive, data-driven control that varies with the graph structure. 

To support this choice, we present extensive experiments in **👉Table 6**, comparing different values of $\beta$ across datasets and backbone models. The results show that $\beta = 2K_0$ achieves the best or near-best accuracy in most cases, validating its effectiveness. 

We hope this resolves the reviewer’s concern, and we thank you again for your review.

