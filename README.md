---
title: "Transfer Learning"
aliases: ["Domain Adaptation", "Fine-tuning", "Foundation Models"]
tags:
  - transfer-learning
  - deep-learning
  - pre-training
---
# Transfer Learning

- **Transfer learning survey** - Organizes transfer by differences between source and target domains, tasks, and available labels; covers instance, feature, parameter, and relational knowledge transfer. [Pan and Yang 2010](<https://www.cse.cuhk.edu.hk/~sinnopan/publications/TLsurvey_0822.pdf> "A Survey on Transfer Learning; TKDE").

## Pretraining and task adaptation

- **ULMFiT** - Pretrains a language model, adapts it to target text, then tunes a classifier. Layer-specific learning rates and gradual unfreezing control how pretrained weights change. [Howard and Ruder 2018](<https://aclanthology.org/P18-1031/> "Universal Language Model Fine-tuning for Text Classification; ACL").
- **BERT** - Masked-token and next-sentence prediction pretrain a bidirectional encoder; task-specific output layers and encoder fine-tuning transfer it to language understanding tasks. [Devlin et al. 2019](<https://aclanthology.org/N19-1423/> "BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding; NAACL").
- **T5** - Casts NLP tasks as text-to-text prediction; compares pretraining objectives, architectures, data, and transfer strategies within a shared framework. [Raffel et al. 2020](<https://jmlr.org/papers/v21/20-074.html> "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer; JMLR").
- **Domain- and task-adaptive pretraining** - Continues language-model pretraining on unlabeled domain text or task-specific text before supervised fine-tuning; studies both separately and together. [Gururangan et al. 2020](<https://aclanthology.org/2020.acl-main.740/> "Don’t Stop Pretraining: Adapt Language Models to Domains and Tasks; ACL").
- **CLIP** - Contrastive image-text pretraining aligns visual and language representations. Text descriptions of candidate classes enable zero-shot classification without target-dataset fine-tuning. [Radford et al. 2021](<https://proceedings.mlr.press/v139/radford21a.html> "Learning Transferable Visual Models From Natural Language Supervision; ICML").

## Frozen features and fine-tuning

- **DeCAF** - Reuses activations from a supervised object-recognition CNN as fixed features for scene recognition, domain adaptation, and fine-grained classification. [Donahue et al. 2014](<https://proceedings.mlr.press/v32/donahue14.html> "DeCAF: A Deep Convolutional Activation Feature for Generic Visual Recognition; ICML").
- **How transferable are features?** - Layer-transfer experiments separate task specialization from optimization difficulties caused by splitting co-adapted neurons; transferability depends on layer and task distance. [Yosinski et al. 2014](<https://arxiv.org/abs/1411.1792> "How transferable are features in deep neural networks?; NeurIPS").
- **L2-SP** - Regularizes fine-tuning toward pretrained parameter values rather than zero, explicitly encouraging retention of the source solution. [Li et al. 2018](<https://proceedings.mlr.press/v80/li18a.html> "Explicit Inductive Bias for Transfer Learning with Convolutional Networks; ICML").
- **Linear probing then fine-tuning** - Fits a linear head before updating the whole model. Studies how fine-tuning can distort useful pretrained features and hurt out-of-distribution accuracy despite improving in-distribution accuracy. [Kumar et al. 2022](<https://arxiv.org/abs/2202.10054> "Fine-Tuning can Distort Pretrained Features and Underperform Out-of-Distribution; ICLR").

## Parameter-efficient adaptation

- **Bottleneck adapters** - Inserts small trainable modules into a pretrained Transformer. The original recipe also tunes layer normalization and the task head; most backbone parameters stay fixed. [Houlsby et al. 2019](<https://proceedings.mlr.press/v97/houlsby19a.html> "Parameter-Efficient Transfer Learning for NLP; ICML").
- **Prefix tuning** - Freezes the language model and learns continuous prefix activations across its layers, providing task-specific conditioning for generation. [Li and Liang 2021](<https://aclanthology.org/2021.acl-long.353/> "Prefix-Tuning: Optimizing Continuous Prompts for Generation; ACL").
- **Prompt tuning** - Learns soft prompt embeddings prepended to the input while freezing the language model; the trainable prompt operates at the input rather than every layer. [Lester et al. 2021](<https://aclanthology.org/2021.emnlp-main.243/> "The Power of Scale for Parameter-Efficient Prompt Tuning; EMNLP").
- **LoRA** - Freezes pretrained weights and learns low-rank additive updates to selected matrices. Reduces trainable parameters and task-specific storage; the base weights need not be low-rank. [Hu et al. 2021](<https://arxiv.org/abs/2106.09685> "LoRA: Low-Rank Adaptation of Large Language Models; arXiv").
- **IA3** - Learns vectors that rescale attention keys, values, and intermediate feed-forward activations while freezing the backbone; introduced with the T-Few adaptation recipe. [Liu et al. 2022](<https://arxiv.org/abs/2205.05638> "Few-Shot Parameter-Efficient Fine-Tuning is Better and Cheaper than In-Context Learning; NeurIPS").
- **QLoRA** - Trains LoRA adapters through a frozen 4-bit base model. NF4 storage, double quantization, and paged optimizers reduce training memory; computation uses higher precision. [Dettmers et al. 2023](<https://arxiv.org/abs/2305.14314> "QLoRA: Efficient Finetuning of Quantized LLMs; NeurIPS").
- **DoRA** - Separates pretrained weights into magnitude and direction; learns magnitude changes and uses LoRA for directional updates. [Liu et al. 2024](<https://arxiv.org/abs/2402.09353> "DoRA: Weight-Decomposed Low-Rank Adaptation; arXiv").

## Domain adaptation

- **Covariate-shift weighting** - Reweights source examples toward the target input distribution. Assumes unchanged label probabilities given inputs and source support covering the target; estimates weights discriminatively. [Bickel et al. 2009](<https://jmlr.org/papers/v10/bickel09a.html> "Discriminative Learning Under Covariate Shift; JMLR").
- **DANN** - Uses labeled source and unlabeled target data. A gradient-reversal layer trains features to support source classification while confusing a source-versus-target discriminator. [Ganin et al. 2016](<https://jmlr.org/papers/v17/15-239.html> "Domain-Adversarial Training of Neural Networks; JMLR").
- **Deep CORAL** - Aligns source and target feature covariances during neural-network training, extending linear CORAL to learned nonlinear representations. [Sun and Saenko 2016](<https://arxiv.org/abs/1607.01719> "Deep CORAL: Correlation Alignment for Deep Domain Adaptation; arXiv").
- **SHOT** - Adapts from a pretrained source model and unlabeled target data without revisiting source data. Freezes the classifier and updates features through information maximization and pseudo-labeling. [Liang et al. 2020](<https://proceedings.mlr.press/v119/liang20a.html> "Do We Really Need to Access the Source Data? Source Hypothesis Transfer for Unsupervised Domain Adaptation; ICML").

## Test-time adaptation

- **Test-Time Training** - Jointly trains prediction and a self-supervised auxiliary task, then updates shared features on unlabeled test inputs before prediction. Includes per-sample and online variants. [Sun et al. 2020](<https://proceedings.mlr.press/v119/sun20b.html> "Test-Time Training with Self-Supervision for Generalization under Distribution Shifts; ICML").
- **Tent** - Minimizes prediction entropy on unlabeled test batches by updating normalization statistics and channel-wise affine parameters; requires no source data or changes to source training. [Wang et al. 2021](<https://arxiv.org/abs/2006.10726> "Tent: Fully Test-time Adaptation by Entropy Minimization; ICLR").
- **CoTTA** - Adapts to changing target streams using averaged teacher predictions and stochastic restoration of source weights to reduce error accumulation and forgetting. [Wang et al. 2022](<https://openaccess.thecvf.com/content/CVPR2022/html/Wang_Continual_Test-Time_Domain_Adaptation_CVPR_2022_paper.html> "Continual Test-Time Domain Adaptation; CVPR").

## Generalization to unseen domains

- **Invariant Risk Minimization** - Seeks a representation admitting the same optimal classifier across training environments. Counterexamples show this objective can still fail on unseen shifts. [Arjovsky et al. 2019](<https://arxiv.org/abs/1907.02893> "Invariant Risk Minimization; arXiv"), [Rosenfeld et al. 2021](<https://arxiv.org/abs/2010.05761> "The Risks of Invariant Risk Minimization; ICLR").
- **Group DRO** - Minimizes worst-group training loss using predefined groups. Experiments show regularization or early stopping matters for worst-group generalization in overparameterized networks. [Sagawa et al. 2019](<https://arxiv.org/abs/1911.08731> "Distributionally Robust Neural Networks for Group Shifts: On the Importance of Regularization for Worst-Case Generalization; arXiv").

## Few-shot and in-context adaptation

- **Matching Networks** - Learns to predict a query label from a small labeled support set; adapts to new classes through the support examples without fine-tuning the network. [Vinyals et al. 2016](<https://arxiv.org/abs/1606.04080> "Matching Networks for One Shot Learning; NeurIPS").
- **Prototypical Networks** - Learns an embedding space and represents each class by its mean support embedding; classifies queries by distances to these prototypes. [Snell et al. 2017](<https://arxiv.org/abs/1703.05175> "Prototypical Networks for Few-shot Learning; NeurIPS").
- **MAML** - Meta-learns initial parameters across tasks so a few gradient updates on a new task's examples produce an adapted predictor. [Finn et al. 2017](<https://proceedings.mlr.press/v70/finn17a.html> "Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks; ICML").
- **In-context learning in GPT-3** - Specifies tasks and demonstrations in the input text, with no gradient updates at deployment. Few-shot conditioning changes the context; fine-tuning changes parameters. [Brown et al. 2020](<https://arxiv.org/abs/2005.14165> "Language Models are Few-Shot Learners; NeurIPS").

<a id="learning-with-limited-labels"></a>

## Learning with limited or unreliable labels

- **Mean Teacher** - Averages model weights to produce teacher targets for consistency training on perturbed inputs. This uses unlabeled observations during fitting; its benefit depends on the architecture and the relation between labeled and unlabeled data. [Tarvainen and Valpola 2017](<https://arxiv.org/abs/1703.01780> "Mean teachers are better role models: Weight-averaged consistency targets improve semi-supervised deep learning results; NeurIPS").
- **FixMatch** - Uses confident predictions on weakly augmented unlabeled images as targets for strongly augmented versions. Combines pseudo-labeling with consistency; the confidence threshold and augmentation policy determine which unlabeled evidence enters training. [Sohn et al. 2020](<https://arxiv.org/abs/2001.07685> "FixMatch: Simplifying Semi-Supervised Learning with Consistency and Confidence; NeurIPS").
- **BALD active learning** - Chooses labels expected to reduce posterior uncertainty about the model, using disagreement among predictive distributions. Houlsby et al. derive it for Gaussian-process classification; querying labels differs from passive few-shot adaptation. [Houlsby et al. 2011](<https://arxiv.org/abs/1112.5745> "Bayesian Active Learning for Classification and Preference Learning; arXiv").
- **Semi-supervised evaluation** - Oliver et al. find that out-of-class unlabeled examples can degrade tested methods and that strong supervised baselines are often underreported. Match backbones and label-selection budgets; more unlabeled data need not improve performance. [Oliver et al. 2018](<https://arxiv.org/abs/1804.09170> "Realistic Evaluation of Deep Semi-Supervised Learning Algorithms; NeurIPS").
- **Confident Learning** - Uses out-of-sample probabilities to estimate the joint distribution of observed and latent labels and rank suspected errors. Assumes class-conditional label noise; exact recovery also needs constraints on prediction errors. [Northcutt et al. 2021](<https://arxiv.org/abs/1911.00068v6> "Confident Learning: Estimating Uncertainty in Dataset Labels; JAIR"); [code](<https://github.com/cleanlab/cleanlab>).

## Continual adaptation and model merging

- **Elastic weight consolidation** - Penalizes changes to parameters important for previous tasks using a diagonal Fisher-weighted quadratic penalty; a regularization approach to reducing catastrophic forgetting. [Kirkpatrick et al. 2017](<https://arxiv.org/abs/1612.00796> "Overcoming catastrophic forgetting in neural networks; PNAS").
- **Learning without Forgetting** - Trains on new-task data while distilling the previous model's old-task outputs on those same inputs. Retains old-task supervision without storing its original training examples. [Li and Hoiem 2016](<https://arxiv.org/abs/1606.09282> "Learning without Forgetting; ECCV").
- **Dark Experience Replay** - Stores past examples and model logits, then matches those logits during later training. Combines replay with distillation when task boundaries blur; its comparisons require accounting for memory and replay computation. [Buzzega et al. 2020](<https://arxiv.org/abs/2004.07211> "Dark Experience for General Continual Learning: a Strong, Simple Baseline; NeurIPS").
- **Task arithmetic** - Defines task vectors as fine-tuned weights minus a shared pretrained initialization. Adding, subtracting or combining these vectors edits model behavior without another gradient-training run. [Ilharco et al. 2023](<https://arxiv.org/abs/2212.04089> "Editing Models with Task Arithmetic; ICLR").

## Evidence and evaluation

- **Domain-adaptation theory** - Relates target error to source error, domain divergence, and the existence of a hypothesis that works in both domains. Distribution alignment alone is insufficient. [Ben-David et al. 2009](<https://link.springer.com/article/10.1007/s10994-009-5152-4> "A theory of learning from different domains; Mach. Learn.").
- **Negative transfer** - Defines harmful transfer relative to the same learning algorithm without source data; analyzes source-target mismatch and target-label availability, and proposes filtering unrelated source examples. [Wang et al. 2019](<https://openaccess.thecvf.com/content_CVPR_2019/html/Wang_Characterizing_and_Avoiding_Negative_Transfer_CVPR_2019_paper.html> "Characterizing and Avoiding Negative Transfer; CVPR").
- **DomainBed study** - Compares domain generalization under controlled architectures and model selection. Carefully tuned empirical risk minimization is competitive on the studied benchmarks. [Gulrajani and Lopez-Paz 2020](<https://arxiv.org/abs/2007.01434> "In Search of Lost Domain Generalization; arXiv").
- **WILDS** - Benchmarks naturally occurring distribution shifts across hospitals, locations, time, and other groups, with prescribed splits and evaluation metrics. [Koh et al. 2021](<https://proceedings.mlr.press/v139/koh21a.html> "WILDS: A Benchmark of in-the-Wild Distribution Shifts; ICML").
- **LoRA Learns Less and Forgets Less** - In studied code and math adaptation settings, standard low-rank LoRA learns less than full fine-tuning but retains more performance outside the target domain. [Biderman et al. 2024](<https://arxiv.org/abs/2405.09673> "LoRA Learns Less and Forgets Less; TMLR").

### Comparisons to run

- **Frozen versus adapted features** - Proposed: compare a linear probe, full fine-tuning, and LP-FT from one checkpoint, with matched tuning budgets and both in-domain and shifted tests. Record seeds, preprocessing, and trainable layers. [Kumar et al. 2022](<https://arxiv.org/abs/2202.10054> "Fine-Tuning can Distort Pretrained Features and Underperform Out-of-Distribution; ICLR").
- **Adaptation versus retention** - Proposed: compare LoRA ranks, QLoRA, and full fine-tuning on the same data; measure target quality, retained abilities, peak training memory, and inference cost separately. [Biderman et al. 2024](<https://arxiv.org/abs/2405.09673> "LoRA Learns Less and Forgets Less; TMLR"), [Dettmers et al. 2023](<https://arxiv.org/abs/2305.14314> "QLoRA: Efficient Finetuning of Quantized LLMs; NeurIPS").
- **Test-time update protocol** - Proposed: compare no updates, Tent, and CoTTA under fixed stream order and batch sizes; report whether state resets between shifts, and evaluate revisited domains for forgetting. [Wang et al. 2021](<https://arxiv.org/abs/2006.10726> "Tent: Fully Test-time Adaptation by Entropy Minimization; ICLR"), [Wang et al. 2022](<https://openaccess.thecvf.com/content/CVPR2022/html/Wang_Continual_Test-Time_Domain_Adaptation_CVPR_2022_paper.html> "Continual Test-Time Domain Adaptation; CVPR").

## Software

- **PEFT** - Hugging Face library for adapting pretrained models through a subset of parameters; integrates with Transformers, Diffusers, and Accelerate. [docs](<https://huggingface.co/docs/peft/index>).
- **TorchVision models** - Pretrained vision weights with associated preprocessing transforms. Select a specific weight version for reproducible transfer experiments. [docs](<https://docs.pytorch.org/vision/stable/models.html>).
- **DomainBed** - Domain-generalization algorithms, datasets, training scripts, and model-selection procedures in a shared experimental framework. [code](<https://github.com/facebookresearch/DomainBed>).
- **WILDS package** - Dataset loading, predefined splits, group metadata, evaluation functions, and baseline code for distribution-shift benchmarks. [code](<https://github.com/p-lambda/wilds>).

## Related topics

- [Feature extraction](https://mlpapers.org/feature-extraction/)
- [Neural networks](https://mlpapers.org/neural-nets/)
