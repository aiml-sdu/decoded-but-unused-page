# Decoded but Unused

> Markdown version of the project page at https://aiml-sdu.github.io/decoded-but-unused-page/. Interactive figures are on the web page. Every number here also appears there and comes from the paper or its stored results.

Instruction tuning routes moral framing into the judgment readout

Chat models change their moral verdicts when the same event is retold. On a matched pair of Gemma-3-4B checkpoints we find that the pretrained model already encodes the framing and does not act on it, and that instruction tuning routes the same signal into the Yes/No answer.

Phongsakon Mark Konrad, Toygar Tanyel, Serkan Ayvaz · University of Southern Denmark and Promake

ICML 2026 Workshop on Mechanistic Interpretability · Seoul, 2026

Explore the 100 dilemmas ↓[OpenReview](https://openreview.net/forum?id=da9ylT2doy) · [PDF](https://openreview.net/pdf?id=da9ylT2doy) · [Markdown version](index.md) · [Code and data](https://github.com/aiml-sdu/decoded-but-unused) · arXiv · coming soon · Cite this article

**Figure 0**Each dot is one dilemma, placed by the tuned model's Yes minus No logit when the event is told to condemn the actor (across) and to sympathise with the actor (up). Dots above the dashed line moved toward Yes when told sympathetically, which 83 of 100 did. The 17 inversions are drawn in terracotta. Hand annotation found that 15 of them are told from the perpetrator's own point of view, and which 15 those are is not stored with the figure data. The pretrained model has stored aggregates only, a mean shift of +0.04 with a 95% interval of −0.09 to 0.16.

## Abstract

Framing studies record that moral verdicts in language models shift when an event is retold, and they leave open where in the network the shift happens. We compare a pretrained and an instruction-tuned checkpoint of Gemma-3-4B on 100 dilemmas from two fictional sources.

First, the framing is decoded in both checkpoints and used only after tuning. A probe recovers it at 0.92 accuracy in the pretrained model. The paired shift, the change in the Yes minus No logit between the sympathetic and the condemning telling of one dilemma, is +0.04 there and +12.18 after tuning, with 83 of 100 dilemmas moving toward Yes. Second, the route is local. The alignment between the framing direction and the answer direction peaks at layer 25 at 0.307, which is 8.4 times the pretrained value, and ten writer heads concentrated at layers 23 to 25 carry 59% of the shift. Finally, the pattern is specific and it replicates. The framing direction aligns with all 9 evaluative answer pairs we tested and stays at or below 0.05 for all 5 neutral pairs, and the shift is positive in all 10 instruction-tuned models we tested, from three families.

## Introduction

We compared Gemma-3-4B before and after instruction tuning on 100 moral dilemmas, each told in a sympathetic, a condemning and an abstract framing. A linear probe reads the framing from both models, at 0.92 accuracy before tuning and 0.97 after. Only the tuned model acts on it, and the framing moves its Yes/No verdict by 12.18 logits against 0.04 before tuning. Inside the tuned model the framing direction turns toward the answer direction at layer 25, where the alignment is 8.4 times the pretrained value, and patching and head ablations show that this route is used. For this one matched pair and this dataset, instruction tuning changes how the representation is read out, not whether it exists.

Three results would have spoken against this reading, and we tested each. If the pretrained verdict already moved with the framing, instruction tuning would be beside the point. A tuned model whose alignment had no layer-localised peak would leave no route to point at. And if patching at that depth recovered nothing, or ablating the candidate heads did no more than ablating random ones, the route would be decorative. None of these happened, although each was tested on one matched pair and this dataset only.

Two framings of one event from the dataset (paper, Appendix A). A third, abstract framing states the same action in neutral terms.

Ask a chat model whether an action was justified, then retell the same event in a sympathetic light and ask again. The verdict can change. We asked a narrower question with a matched pair of models. Does the pretrained model already represent the framing, and if it does, why does the framing leave its verdict alone?

## Some definitions

Seven terms carry the rest of the post. Each is defined on one dilemma told two ways.

**Framing triplet.** One event told three times, sympathetic to the actor, condemning of the actor, and abstract.

**Paired shift.** For one dilemma, the Yes minus No logit under the sympathetic telling minus the same quantity under the condemning telling.

**δ_frame(ℓ).** The mean activation of sympathetic prompts minus that of condemning prompts, at the last token and layer ℓ.

**δ_judge.** Two columns of the unembedding matrix give the output directions for “ Yes” and “ No”, and δ_judge is their difference.

**ρ(ℓ).** Cosine between δ_frame(ℓ) and δ_judge, near 0 when the framing points away from the answer and grows as it turns toward it.

**Recovery R.** The share of the natural gap between two runs that is restored when one run's activation is copied into the other.

**Writer head.** An attention head whose sympathetic-minus-condemning output points along δ_judge, so that it writes the framing toward the answer.

## Both models encode the framing. Only the tuned model uses it.

Two measurements carry the argument. A linear probe asks whether the framing can be read from the residual stream at all. The alignment cosine ρ(ℓ) asks whether the direction that separates sympathetic from condemning prompts points toward the direction that separates Yes from No.

Drag the layer slider. The probe stays high in both models at every layer, and the cosine stays near zero in the pretrained model everywhere. In the tuned model it rises late and peaks at layer 25.

*Figure.* **Figure 1**Each dial draws the angle between δ_frame(ℓ) and δ_judge in the full residual space. At layer 25 the tuned model reaches 0.307 against 0.036 for the pretrained model, 8.4 times higher at the same layer.

## One free variable, the instruction-tuning step

Gemma-3-4B comes as a pretrained and an instruction-tuned checkpoint with the same architecture, tokeniser, training corpus and pretraining seed. Comparing the two isolates what post-training changed. We score each prompt by the final-token logit gap between “ Yes” and “ No”.

The paired shift is +0.04 logits in the pretrained model, and its confidence interval straddles zero. In the tuned model it is +12.18, and 83 of the 100 dilemmas move toward Yes when told sympathetically, as the field of dots at the top shows. The probe recovers the framing in both.

## One event, written three ways

Each of the 100 dilemmas tells one canonical event three times. The sympathetic and condemning versions take a side, and the abstract one strips the rhetoric and restates the action in neutral third-person terms. Fifty come from *Attack on Titan* and fifty from *Game of Thrones*, and each framing runs to about 400 to 950 characters. Below are the two worked examples from the paper's appendix, one on each side of the perpetrator split.

Mean over all 100 dilemmas, from the last layer of the stored logit-lens data. Scores for these two example dilemmas are not stored separately.

*Figure.* **Appendix A**Texts reproduced from the paper. Anonymisation turns names into role labels, factions into generic descriptors and iconic concepts into functional descriptions. The abstract framing has no entity references, so it is the same in both versions.

## The pretrained model separates the framings without pointing them at the answer

Project the final-token activations onto their top two principal components at each model's peak-alignment layer. Both models pull sympathetic and condemning prompts apart. The arrows are the two directions from the full space, flattened into this plane.

PCA does not preserve angles from the full space, so the pretrained arrows can look close here while their true cosine is +0.05. The label above each panel is the measurement, and the plot is a view of it.

*Figure.* **Figure 2**100 points per framing. Hover a point to ring the same dilemma's other framing, or connect every pair.

## The three framings agree for twenty layers, then split

The logit lens reads each layer's residual stream through the final norm and the unembedding, which gives a running Yes minus No gap. In the tuned model the three framings track each other until about layer 20. Then the sympathetic trajectory peels away while the alignment climbs to its peak. The pretrained model shows no such transition, and its trajectories collapse onto one flat line.

*Figure.* **Figure 3**Gemma-3-4B-IT, mean over 100 dilemmas. At the output the condemning framing ends near −12 and the abstract near −8, while the sympathetic ends near zero. Under these prompts the tuned model leans toward condemning by default.

## Patching the framing in moves the verdict, more easily in the direction of the model's default

Alignment shows that two directions line up, and patching asks whether the model uses them. We run the model on one framing and then on the other, and copy the first run's residual activation into the second at the final token. The recovery R is the share of the natural judgment gap that this copy alone restores. In the released code the clean activation is taken at the corrupt prompt's final index, which is an earlier clean token when the clean framing is longer.

At layer 20 the condemning activation copied into the sympathetic run restores 0.885 of the gap, and the reverse restores 0.521. The tuned model leans toward condemning under our prompts, so one direction of the patch pushes with its default and the other pushes against it.

A second test removes a low-rank subspace. We take the top four principal directions of the framing differences and keep only their part orthogonal to δ_judge. Removing that subspace still takes away 0.82 of the shift at layers 20 and 25, while random subspaces of the same rank and norm take away at most 0.23. By layer 28 the framing subspace has merged into the judgment direction.

*Figure.* **Figure 4**Left, recovery by hook at layer 20 (paper Table 2). Right, shrinkage of the paired shift when a subspace is removed (paper Table 3).

## Ten writer heads, concentrated at layers 23 to 25, carry 59% of the shift

For each attention head we take its sympathetic-minus-condemning contribution at the final token and project it onto δ_judge. Five heads at layers 23 and 24 each exceed |p| = 0.24, and no head elsewhere in layers 10 to 25 exceeds 0.10. The strongest writer is layer 23, head 3.

Then we remove them. Mean-ablating the top ten writers cuts the paired shift from 12.18 to 5.04 logits. Ten random heads matched to the writers' layers barely move it (+0.23 ± 0.95 over ten seeds), so the writer effect sits 7.8 standard deviations from that null.

*Figure.* **Figure 5**Writer attribution and its causal check on Gemma-3-4B-IT. Mean ablation replaces a head's output with its dataset mean, and zero ablation would push the residual norm out of distribution.

## The route needs moral content, and it ends at evaluation in general

On the output side we swap Yes/No for other answer pairs. The framing direction aligns with every evaluative pair we tried, from True/False to justified/unjustified, and with none of the neutral ones such as cat/dog. Evaluative pairs align 4.1 times as strongly as neutral ones in the tuned model, against 1.25 times in the pretrained one.

On the input side we swap moral content for non-moral sentiment, positive and negative descriptions of food, music, places or weather. At layer 25 that framing aligns at +0.022, fourteen times less. Sentiment content asked the moral question gives −0.007, so the moral question on its own does not open the route.

*Figure.* **Figure 6**Top, alignment at layer 25 for the input-side controls. Sentiment framing has its own, weaker peak at layer 32 (ρ = +0.140). Bottom, peak |ρ| in Gemma-3-4B-IT across 34 layers for each answer pair (paper Table 4). Evaluative peaks sit at layers 17 to 25, and neutral peaks are scattered.

## Ten instruction-tuned models, three families, one sign

The paired shift is positive, with a 95% interval above zero, in every instruction-tuned checkpoint we tried, from 2B to 32B parameters across Gemma, Qwen and Llama. All ten reach a within-family peak alignment above +0.20.

Llama-3.1-8B is the instructive outlier. Its peak cosine (+0.249 at layer 29) is comparable to Gemma's, and its shift is four times smaller than that of Gemma-3-12B. The cosine measures where the framing points. How strongly later layers amplify it is a separate quantity that the cosine does not see.

*Figure.* **Figure 7**The grey row is the only pretrained checkpoint, the matched Gemma-3-4B-PT, and its interval straddles zero. Hover a row for the numbers.

## Most of the inversions are perpetrator stories

In the tuned model 17 of the 100 dilemmas shift the wrong way. Hand-annotated for narrative perspective, 15 of them are perpetrator-POV. The sympathetic frame is narrated by the actor, and the act is the canonical wrongdoing of the story, as in the Rumbling example in the dataset section. In these cases the verdict follows the depicted act more than the narrator's voice.

Remove them and the shift rises to +15.22, with 98% of the remaining dilemmas positive. The routing keeps its direction, and its size is held down when the act itself is clearly condemnable. A benchmark built on first-person wrongdoer narration could therefore underestimate the routing.

Squares are drawn in count order, not per dilemma.

## What this does and doesn't show

The evidence supports a routing claim for one matched pair on this dataset. It is neither a complete circuit map nor a general account of moral framing.

- In Gemma-3-4B on these 100 dilemmas, the framing is decodable before instruction tuning but moves the verdict only after it (Figure 1).

- Four measurements converge on one mid-to-late route in the tuned model, namely the alignment cosine, residual patching, a ten-head writer set and a rank-4 subspace (causal tests, writer heads).

- The positive shift, and a within-family peak alignment above +0.20, appear in ten instruction-tuned models from three families.

- That this holds beyond one matched pair. The ten other checkpoints are instruction-tuned only.

- That the framing direction is pure framing. The variants are not minimal pairs, since they also differ in narrator, perspective and emphasis.

- Held-out causal estimates. Layers and heads were chosen on the same 100 dilemmas they were measured on, and the patching and ablation effects have no confidence intervals.

- The rest of the circuit. The ten heads carry 59% of the shift, and the remainder is not localised (try the ablations).

### A screen you can run on other behaviours

The alignment cosine needs one cached forward pass per layer, a difference of class means and one column of W_U. Given a chat model and its base checkpoint, compute ρ(ℓ) in both for any two contrasting prompt classes, such as refusal and compliance. Look for a layer-localised peak in the tuned model and a flat line in the base one, then test it with interventions matched to the behaviour. A peak is only a candidate route until those interventions confirm it.

Next tests from the paper are non-fictional and minimal-pair datasets, a held-out split for choosing layers and heads with bootstrap intervals, path patching downstream of the writer heads, and a run across fine-tuning checkpoints.

## Cite this article

```bibtex
@inproceedings{konrad2026decoded,
  title     = {Decoded but Unused: Instruction Tuning Routes Moral
               Framing into the Judgment Readout},
  author    = {Konrad, Phongsakon Mark and Tanyel, Toygar and Ayvaz, Serkan},
  booktitle = {ICML 2026 Workshop on Mechanistic Interpretability},
  year      = {2026},
  url       = {https://openreview.net/forum?id=da9ylT2doy}
}
```

Code, dataset and the cached results behind every figure are at [github.com/aiml-sdu/decoded-but-unused](https://github.com/aiml-sdu/decoded-but-unused). An arXiv version is in preparation.
