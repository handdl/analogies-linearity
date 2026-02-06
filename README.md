# Empirical verification of "Analogies Explained" 

**TL;DR** By using synthetic co-occurrence data where paraphrases hold by design I showed that paraphrase structure alone is indeed
sufficient for analogies to emerge, even with simple embeddings (PCA) on a tiny synthetic dataset.

**Some Motivation.** The discovery that word embeddings encode semantic relationships as linear directions - that king - man + woman = queen actually works - 
was one of the first surprising results in neural network interpretability. But why should vector arithmetic capture meaning?
Understanding why such linear relationships emerge in the simplest embedding models is an extremely interesting question in
light of the hype surrounding LLM and interpretability.

**Why Do Analogies Work?** The classic word2vec result: `king - man + woman ≈ queen`. But why should vector arithmetic capture semantic relationships?
The paper's answer: **paraphrases**. If "king" means roughly "man + royalty" and "queen" means "woman + royalty", then the royalty component cancels:

```
king - man + woman = (man + royalty) - man + woman = woman + royalty = queen
```

**Core Equation.** For a word $w^*$ that paraphrases as set $W$ (e.g., king ≈ {man, royalty}):

$$\text{PMI}(c, w^*) = \sum_{w \in W} \text{PMI}(c, w) + \rho(c) + \sigma(c) - \tau$$

Where:
- $\rho(c)$ — paraphrase quality: how well $w^*$ and $W$ predict the same contexts
- $\sigma(c)$ — conditional dependence: whether words in $W$ co-occur more than expected given context
- $\tau$ — marginal dependence: whether words in $W$ co-occur more than expected overall

**Why This Leads to Analogies?** Consider two parallel paraphrases `king` ≈ `man` + `royalty` and  `queen` ≈ `woman` + `royalty`.
Both extend a gender term by the same concept (`royalty`), and thanks to linearity the error terms cancel:

```
(king - man) - (queen - woman) = error_king - error_queen ≈ 0
```

Rearranging gives us `king - man + woman ≈ queen`, i.e. the analogy works because both words **share** the same paraphrase extension.

P.S. There are works suggesting that paraphrase may not be the reason why `word2vec` exhibits linear analogies — analogies work even when 
word co-occurrence is zero. I don't have the passion to dig deeper into this, but it's worth keeping in mind: if true, it means that 
while paraphrase structure is sufficient for linear analogies, it may not be why we actually observe them in practice.

**For careful readers** - read the paper and enjoy the math, it's quite elegant. It shows that we don't need to explicitly find paraphrases 
(which would be hard), their mere existence is enough for linear analogies to emerge and be useful.
