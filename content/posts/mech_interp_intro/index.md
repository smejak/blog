---
title: Starting in mechanistic interpretability
date: "2024-01-22 10:00:00"
---

I recently began exploring the field of mechanistic interpretability. It’s fascinating in a very similar way as the field of active inference ~1-2 years ago (let’s hope it maintains that in the way as actinf has), as in it seems to attract a lot of researchers from multiple disciplines, and there’s a number of different threads to consider before one gets an idea on what is going on. It’s also still very new, which I found out by diving into multiple papers that turned out to be about more or less the same underlying concept (finding causal structure in transformers), but which used completely different terms and approaches. So in this short post, I wanted to write a few half-baked thoughts on the different threads in mech interp and some open questions.

## Finding the Causal Structure

This seems to be a major thread in mech interp and based on my current understanding the standard for differentiating between “we understand what the model is doing in context X” versus “we don’t understand what the model is doing in context X”. The first question that comes to mind is: What kind of causal structure are we looking for? Or should be looking for? As far as I can tell there are a few and it’s still not completely obvious to me which regimes of causality within LLMs are more informative.

[Conmy et al.](https://arxiv.org/abs/2304.14997) developed an algorithm (ACDC) within a regime of interpretability research where you’re interested in finding a circuit within the model’s computational graph (i.e. a graph of all computational units, MLPs and attention heads (although the paper does also mention that the graph can be in activation space, that seems to depend on the modeler)) that implements a particular behavior. This is one approach to finding causal structure within the model, i.e. a circuit that implements the desired behavior, and depending on how detailed you want it to be, you can define the graph in either computational-unit-space or activation-space (though I think activation-space is likely more difficult).

From a slightly different perspective, there’s the [causal](https://arxiv.org/abs/2106.02997) [abstraction](https://arxiv.org/abs/2301.04709) [work](https://arxiv.org/abs/2305.08809), primarily pioneered by [Atticus Geiger](https://arxiv.org/abs/2303.02536) and colleagues. This work is also about explaining how a model does something, but this time we assume some higher-level causal model (not in any domain related to the LLM, think a causal graph describing the mechanism of addition or some boolean logic) and then we are trying to find the activations within the transformer that align to this higher-level structure. The result will be some structure in activation-space with some level of equivalence to the higher-level model. What’s really cool about this work is that the representations of this imposed causal structure can be distributed, which I haven’t seen addressed in other work (let me know if I missed something).

The last thread I wanted to mention is the Sparse Autoencoder work spearheaded by [Bricken et al.](https://transformer-circuits.pub/2023/monosemantic-features) and [Cunningham et al.](https://arxiv.org/abs/2309.08600), where we seek to better understand how MLPs represent and use features by training sparse autoencoders on their outputs. These SAEs will then project the activations to a higher-dimensional space with the hope of disentangling polysemantic features (i.e. when multiple features are captured by the same neuron). Cunningham et al. used this in conjunction with the ACDC algorithm and other methods (activation patching) to find how features in one layer causally affect features in subsequent layers. Based on both Bricken et al. and Cunningham et al., the question that arises next is how do you effectively scale the SAE approach to large models with many layers.

## Some open questions

* At what point do we say a model is interpretable? (the map is not the territory)

* What do circuits in activation space/computational-unit-space/higher-level causal models tell us about the model’s behavior that we can apply in further research?

* For instance, would it be helpful to consider something like, “What kind of generative model best explains the transformer behavior in context X?” Would we start to see similar-enough behaviors across tasks?

* This is related to a question on how dynamic should our explanations of model behavior be. One thing is treating a particular set of activations from a forward pass as a single dataset and then we can ask what kind of model best explains that dataset, another thing is trying to identify some smaller model which you can probe in multiple directions that will still tell you something about the LM’s behavior in different contexts (though there are bound to be limits on that).

* It also seems that a lot of work uses similar, albeit slightly distinct/custom terminology, what are the necessary components of a “mechanistic interpretability ontology”?