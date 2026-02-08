# Empirical verification of "Analogies Explained" 
**TL;DR** Designed simulation shows that paraphrase structure alone is sufficient for analogies to emerge, even with a simple embedding model (PCA) on a tiny synthetic dataset.

**Motivation.** The discovery that word embeddings encode semantic relationships as linear directions (e.g. `king - man + woman ≈ queen`) was one of the first surprising results in NN interpretability. Understanding why such linear relationships (especially as vector arithmetic) emerge in the simplest embedding models is an extremely interesting question in light of the hype around LLMs.

**Why do analogies work?** In word2vec we have `king - man + woman ≈ queen`. But why should vector arithmetic capture semantic relationships? The paper's answer: **paraphrases**. If "royalty" shifts "man" to "king" in the same sense as "woman" to "queen", then in the case of symmetry, errors in pairs man=king+royalty, queen=woman+royalty may cancel:
```
king = man + royalty + error1
queen = woman + royalty + error2
king - man + woman = queen + (error1 - error2) ≈ queen  # if errors are small or cancel each other
```

**Core Equation.** For a word $w^\*$ that paraphrases as set $W$ (e.g., king ≈ {man, royalty} in some sense):
$$\text{PMI}(c, w^\*) = \sum_{w \in W} \text{PMI}(c, w) + \rho(c) + \sigma(c) - \tau$$
Where:
- $\rho(c)$ — paraphrase quality: how well $w^*$ and $W$ predict the same contexts
- $\sigma(c)$ — conditional dependence: whether words in $W$ co-occur more than expected given context
- $\tau$ — marginal dependence: whether words in $W$ co-occur more than expected overall

Note, making all errors zero is almost impossible, but it works because these terms may cancel each other if the shared paraphrase has symmetry. Also interesting: we don't need to actually find this paraphrase. If it exists - we can use the structure it creates for us; if it doesn't exist we don't care. I suggest enjoying the paper for careful readers because the math inside is _elegant_. Honestly, that's why I decided to spend some time with this project. This error decomposition is the key to my experiments - now I can _simulate_ data in such a way to make all terms as small (or symmetric) as possible to actually see if I can reproduce analogies emergence even under the simplest embedding model and toy synthetic data. 

**Is paraphrase necessary?** There are works suggesting that paraphrase may not be the reason why `word2vec` exhibits linear analogies — analogies work even when word co-occurrence is zero. I don't have the passion to dig deeper into this, but it's worth keeping in mind: if true, it means that while paraphrase structure is _sufficient_ for linear analogies, it may not be why we actually observe them in practice.
