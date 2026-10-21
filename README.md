---
title: "Transfer Learning"
aliases: ["Domain Adaptation", "Fine-tuning", "Foundation Models"]
tags:
  - transfer-learning
  - deep-learning
  - pre-training
---
# Transfer Learning

- **[Transfer learning survey](https://www.cse.cuhk.edu.hk/~sinnopan/publications/TLsurvey_0822.pdf)** — Pan & Yang, 2010, TKDE. Organizes transfer by differences between source and target domains, tasks, and available labels; covers instance, feature, parameter, and relational knowledge transfer.

## Pretraining and task adaptation

- **[ULMFiT](https://aclanthology.org/P18-1031/)** — Howard & Ruder, 2018, ACL. Pretrains a language model, adapts it to target text, then tunes a classifier. Layer-specific learning rates and gradual unfreezing control how pretrained weights change.
- **[BERT](https://aclanthology.org/N19-1423/)** — Devlin et al., 2019, NAACL. Masked-token and next-sentence prediction pretrain a bidirectional encoder; task-specific output layers and encoder fine-tuning transfer it to language understanding tasks.
- **[T5](https://jmlr.org/papers/v21/20-074.html)** — Raffel et al., 2020, JMLR. Casts NLP tasks as text-to-text prediction; compares pretraining objectives, architectures, data, and transfer strategies within a shared framework.
- **[Domain- and task-adaptive pretraining](https://aclanthology.org/2020.acl-main.740/)** — Gururangan et al., 2020, ACL. Continues language-model pretraining on unlabeled domain text or task-specific text before supervised fine-tuning; studies both separately and together.
- **[CLIP](https://proceedings.mlr.press/v139/radford21a.html)** — Radford et al., 2021, ICML. Contrastive image–text pretraining aligns visual and language representations. Text descriptions of candidate classes enable zero-shot classification without target-dataset fine-tuning.

## Frozen features and fine-tuning

- **[DeCAF](https://proceedings.mlr.press/v32/donahue14.html)** — Donahue et al., 2014, ICML. Reuses activations from a supervised object-recognition CNN as fixed features for scene recognition, domain adaptation, and fine-grained classification.
- **[How transferable are features?](https://arxiv.org/abs/1411.1792)** — Yosinski et al., 2014, NeurIPS. Layer-transfer experiments separate task specialization from optimization difficulties caused by splitting co-adapted neurons; transferability depends on layer and task distance.
- **[L2-SP](https://proceedings.mlr.press/v80/li18a.html)** — Li et al., 2018, ICML. Regularizes fine-tuning toward pretrained parameter values rather than zero, explicitly encouraging retention of the source solution.
- **[Linear probing then fine-tuning](https://arxiv.org/abs/2202.10054)** — Kumar et al., 2022, ICLR. Fits a linear head before updating the whole model. Studies how fine-tuning can distort useful pretrained features and hurt out-of-distribution accuracy despite improving in-distribution accuracy.

## Parameter-efficient adaptation

- **[Bottleneck adapters](https://proceedings.mlr.press/v97/houlsby19a.html)** — Houlsby et al., 2019, ICML. Inserts small trainable modules into a pretrained Transformer. The original recipe also tunes layer normalization and the task head; most backbone parameters stay fixed.
- **[Prefix tuning](https://aclanthology.org/2021.acl-long.353/)** — Li & Liang, 2021, ACL. Freezes the language model and learns continuous prefix activations across its layers, providing task-specific conditioning for generation.
- **[Prompt tuning](https://aclanthology.org/2021.emnlp-main.243/)** — Lester et al., 2021, EMNLP. Learns soft prompt embeddings prepended to the input while freezing the language model; the trainable prompt operates at the input rather than every layer.
- **[LoRA](https://arxiv.org/abs/2106.09685)** — Hu et al., 2021 preprint; ICLR 2022. Freezes pretrained weights and learns low-rank additive updates to selected matrices. Reduces trainable parameters and task-specific storage; the base weights need not be low-rank.
- **[IA3](https://arxiv.org/abs/2205.05638)** — Liu et al., 2022, NeurIPS. Learns vectors that rescale attention keys, values, and intermediate feed-forward activations while freezing the backbone; introduced with the T-Few adaptation recipe.
- **[QLoRA](https://arxiv.org/abs/2305.14314)** — Dettmers et al., 2023, NeurIPS. Trains LoRA adapters through a frozen 4-bit base model. NF4 storage, double quantization, and paged optimizers reduce training memory; computation uses higher precision.
- **[DoRA](https://arxiv.org/abs/2402.09353)** — Liu et al., 2024, ICML. Separates pretrained weights into magnitude and direction; learns magnitude changes and uses LoRA for directional updates.

## Domain adaptation

- **[Covariate-shift weighting](https://jmlr.org/papers/v10/bickel09a.html)** — Bickel et al., 2009, JMLR. Reweights source examples toward the target input distribution. Assumes unchanged label probabilities given inputs and source support covering the target; estimates weights discriminatively.
- **[DANN](https://jmlr.org/papers/v17/15-239.html)** — Ganin et al., 2016, JMLR. Uses labeled source and unlabeled target data. A gradient-reversal layer trains features to support source classification while confusing a source-versus-target discriminator.
- **[Deep CORAL](https://arxiv.org/abs/1607.01719)** — Sun & Saenko, 2016, ECCV workshop. Aligns source and target feature covariances during neural-network training, extending linear CORAL to learned nonlinear representations.
- **[SHOT](https://proceedings.mlr.press/v119/liang20a.html)** — Liang et al., 2020, ICML. Adapts from a pretrained source model and unlabeled target data without revisiting source data. Freezes the classifier and updates features through information maximization and pseudo-labeling.

## Test-time adaptation

- **[Test-Time Training](https://proceedings.mlr.press/v119/sun20b.html)** — Sun et al., 2020, ICML. Jointly trains prediction and a self-supervised auxiliary task, then updates shared features on unlabeled test inputs before prediction. Includes per-sample and online variants.
- **[Tent](https://arxiv.org/abs/2006.10726)** — Wang et al., 2020 preprint; ICLR 2021. Minimizes prediction entropy on unlabeled test batches by updating normalization statistics and channel-wise affine parameters; requires no source data or changes to source training.
- **[CoTTA](https://openaccess.thecvf.com/content/CVPR2022/html/Wang_Continual_Test-Time_Domain_Adaptation_CVPR_2022_paper.html)** — Wang et al., 2022, CVPR. Adapts to changing target streams using averaged teacher predictions and stochastic restoration of source weights to reduce error accumulation and forgetting.

## Generalization to unseen domains

- **[Invariant Risk Minimization](https://arxiv.org/abs/1907.02893)** — Arjovsky et al., 2019 preprint. Seeks a representation admitting the same optimal classifier across training environments. [Counterexamples](https://arxiv.org/abs/2010.05761) show this objective can still fail on unseen shifts.
- **[Group DRO](https://arxiv.org/abs/1911.08731)** — Sagawa et al., 2019 preprint; ICLR 2020. Minimizes worst-group training loss using predefined groups. Experiments show regularization or early stopping matters for worst-group generalization in overparameterized networks.

## Few-shot and in-context adaptation

- **[Matching Networks](https://arxiv.org/abs/1606.04080)** — Vinyals et al., 2016, NeurIPS. Learns to predict a query label from a small labeled support set; adapts to new classes through the support examples without fine-tuning the network.
- **[Prototypical Networks](https://arxiv.org/abs/1703.05175)** — Snell et al., 2017, NeurIPS. Learns an embedding space and represents each class by its mean support embedding; classifies queries by distances to these prototypes.
- **[MAML](https://proceedings.mlr.press/v70/finn17a.html)** — Finn et al., 2017, ICML. Meta-learns initial parameters across tasks so a few gradient updates on a new task's examples produce an adapted predictor.
- **[In-context learning in GPT-3](https://arxiv.org/abs/2005.14165)** — Brown et al., 2020, NeurIPS. Specifies tasks and demonstrations in the input text, with no gradient updates at deployment. Few-shot conditioning changes the context; fine-tuning changes parameters.

## Learning with limited labels

- **[Mean Teacher](https://arxiv.org/abs/1703.01780)** — 2017. Averages model weights to produce teacher targets for consistency training on perturbed inputs. This uses unlabeled observations during fitting; its benefit depends on the architecture and the relation between labeled and unlabeled data.
- **[FixMatch](https://arxiv.org/abs/2001.07685)** — 2020. Uses confident predictions on weakly augmented unlabeled images as targets for strongly augmented versions. Combines pseudo-labeling with consistency; the confidence threshold and augmentation policy determine which unlabeled evidence enters training.
- **[BALD active learning](https://arxiv.org/abs/1112.5745)** — 2011. Chooses labels expected to reduce posterior uncertainty about the model, using disagreement among predictive distributions. Houlsby et al. derive it for Gaussian-process classification; querying labels differs from passive few-shot adaptation.
- **[Semi-supervised evaluation](https://arxiv.org/abs/1804.09170)** — 2018. Oliver et al. find that out-of-class unlabeled examples can degrade tested methods and that strong supervised baselines are often underreported. Match backbones and label-selection budgets; more unlabeled data need not improve performance.

## Continual adaptation and model merging

- **[Elastic weight consolidation](https://arxiv.org/abs/1612.00796)** — 2016. Penalizes changes to parameters important for previous tasks using a diagonal Fisher-weighted quadratic penalty; a regularization approach to reducing catastrophic forgetting.
- **[Learning without Forgetting](https://arxiv.org/abs/1606.09282)** — 2016. Trains on new-task data while distilling the previous model's old-task outputs on those same inputs. Retains old-task supervision without storing its original training examples.
- **[Dark Experience Replay](https://arxiv.org/abs/2004.07211)** — 2020. Stores past examples and model logits, then matches those logits during later training. Combines replay with distillation when task boundaries blur; its comparisons require accounting for memory and replay computation.
- **[Task arithmetic](https://arxiv.org/abs/2212.04089)** — 2022. Defines task vectors as fine-tuned weights minus a shared pretrained initialization. Adding, subtracting or combining these vectors edits model behavior without another gradient-training run.

## Evidence and evaluation

- **[Domain-adaptation theory](https://link.springer.com/article/10.1007/s10994-009-5152-4)** — Ben-David et al., 2010, Machine Learning. Relates target error to source error, domain divergence, and the existence of a hypothesis that works in both domains. Distribution alignment alone is insufficient.
- **[Negative transfer](https://openaccess.thecvf.com/content_CVPR_2019/html/Wang_Characterizing_and_Avoiding_Negative_Transfer_CVPR_2019_paper.html)** — Wang et al., 2019, CVPR. Defines harmful transfer relative to the same learning algorithm without source data; analyzes source–target mismatch and target-label availability, and proposes filtering unrelated source examples.
- **[DomainBed study](https://arxiv.org/abs/2007.01434)** — Gulrajani & Lopez-Paz, 2020 preprint; ICLR 2021. Compares domain generalization under controlled architectures and model selection. Carefully tuned empirical risk minimization is competitive on the studied benchmarks.
- **[WILDS](https://proceedings.mlr.press/v139/koh21a.html)** — Koh et al., 2021, ICML. Benchmarks naturally occurring distribution shifts across hospitals, locations, time, and other groups, with prescribed splits and evaluation metrics.
- **[LoRA Learns Less and Forgets Less](https://arxiv.org/abs/2405.09673)** — Biderman et al., 2024, TMLR. In studied code and math adaptation settings, standard low-rank LoRA learns less than full fine-tuning but retains more performance outside the target domain.

### Comparisons to run

- **Frozen versus adapted features** — Proposed: compare a linear probe, full fine-tuning, and LP-FT from one checkpoint, with matched tuning budgets and both in-domain and shifted tests. Record seeds, preprocessing, and trainable layers. [LP-FT](https://arxiv.org/abs/2202.10054).
- **Adaptation versus retention** — Proposed: compare LoRA ranks, QLoRA, and full fine-tuning on the same data; measure target quality, retained abilities, peak training memory, and inference cost separately. [LoRA study](https://arxiv.org/abs/2405.09673), [QLoRA](https://arxiv.org/abs/2305.14314).
- **Test-time update protocol** — Proposed: compare no updates, Tent, and CoTTA under fixed stream order and batch sizes; report whether state resets between shifts, and evaluate revisited domains for forgetting. [Tent](https://arxiv.org/abs/2006.10726), [CoTTA](https://openaccess.thecvf.com/content/CVPR2022/html/Wang_Continual_Test-Time_Domain_Adaptation_CVPR_2022_paper.html).

## Software

- **[PEFT](https://huggingface.co/docs/peft/index)** — Hugging Face library for adapting pretrained models through a subset of parameters; integrates with Transformers, Diffusers, and Accelerate.
- **[TorchVision models](https://docs.pytorch.org/vision/stable/models.html)** — Pretrained vision weights with associated preprocessing transforms. Select a specific weight version for reproducible transfer experiments.
- **[DomainBed](https://github.com/facebookresearch/DomainBed)** — Domain-generalization algorithms, datasets, training scripts, and model-selection procedures in a shared experimental framework.
- **[WILDS package](https://github.com/p-lambda/wilds)** — Dataset loading, predefined splits, group metadata, evaluation functions, and baseline code for distribution-shift benchmarks.

## Related Topics

- **[Feature extraction](https://mlpapers.org/feature-extraction/)** — Feature construction, learned representations, and dimensionality reduction.
- **[Neural networks](https://mlpapers.org/neural-nets/)** — Architectures, optimization, and training methods.
