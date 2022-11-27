---
title: Half-baked thoughts November 2022
date: "2022-11-27 15:00:00"
tags:
- Half-baked thoughts
---

This is a series of blog posts where I try to condense the different threads that I’ve been exploring in the past month. Just a collection of thoughts.

### Distributed Computing

Distributed computing is an exciting area of artificial intelligence infrastructure research, specifically due to its ability to train machine learning models across a large network of decentralized devices (perhaps even across different device types), share data, make inferences at arbitrary nodes, and overall create a dynamic, open network where participants freely join, earn rewards for contributing value, and have access to resources that would have otherwise been difficult to obtain (which can take a number of different forms).

I came across three projects that each offer slightly different solutions (although the space of distributed ML projects overall is far wider). The first one is [MLC at Home](https://www.mlcathome.org/), which, according to their website is a *“distributed computing project dedicated to understanding and interpreting complex machine learning models”*. They also use the BOINC infrastructure, which is used by projects like [Gridcoin](https://gridcoin.us/), so it would be interesting to see how we can effectively align incentive with research in distributed AI. Next is [gensyn](https://www.gensyn.ai/), a layer-1 project for decentralized ML development. This project is really exciting and their [litepaper](https://docs.gensyn.ai/litepaper/) goes quite deep into their use of parallelization and how they handle privacy and proofs of ML compute jobs. The last of the decentralized compute projects that is definitely worth experimenting with is [Bacalhau](https://www.bacalhau.org/). Bacalhau is a compute infrastructure on top of IPFS, but more importantly, it’s already live and you can run compute jobs on it. For instance, it’s possible to use the recent OpenAI Whisper model to transcribe video and audio files, as shown in this [walkthrough](https://medium.com/nerd-for-tech/how-to-use-bacalhau-and-openai-whisper-to-transcribe-a-youtube-video-7b6ee0135ce2).

### Molecular Machines

Moving away from distributed compute to a completely different topic (although I suppose distributed systems still play a role, as always), I’ve started to explore molecular machines, particularly with this [paper](https://pubs.acs.org/doi/10.1021/acscentsci.0c00064). Molecular machines are fascinating, both from an engineering standpoint (What role will AI & simulated autonomous agents play in the design of molecular machines?) and as a potentially transformational technology.

### Active Inference

Active inference research is accelerating at an ever increasing rate. One question that seems to always be relevant is that of the structure of the generative model (W*hat is the optimal way to encode beliefs about hidden states and state transitions?* *How to reflect the model’s ability to learn more effective representations?* etc.) The paper *[Structure learning enhances concept formation in synthetic Active Inference agents](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0277199)* considers Bayesian model reduction as the structure learning paradigm for active inference agents. It also resolves the uncertainty in distinguishing structure learning and parameter learning within their respective contexts. On the question of implementation, this [paper](https://www.nature.com/articles/s41598-018-38246-3) covers the different message passing schemes and statistical approximations and compares them in a simulated inference task.

### Reinforcement Learning and Large Language Models

The trend towards distributed compute infrastructure for AI development seems to speak to the need for composability and interoperability across different domains, both within and outside of pure AI R&D. [LangChain](https://github.com/hwchase17/langchain) looks promising for achieving these two goals for LLMs and it would be great to explore its usage in something akin to Meta AI’s recent CICERO model, which combines an imitation learning/reinforcement learning with a language component to be able to play the game Diplomacy, which involves both strategic planning and a verbal interaction with other players. This, along with new schemes for continually improving autonomous agents (like the one presented in this [paper](https://arxiv.org/abs/2211.11602)), will significantly increase the availability of these models to more developers and researcher, as well as drive a lot of new use cases that rely on agents being able to interact with multi-modal environments (this is also where active inference comes into play).