# ML Fundamentals Cheatsheet

Core machine learning terms, from learning types and metrics to drift, MLOps and model serving.

> Related: [AI / LLM questions](../topics/06-ai-llm/README.md) · [ML fundamentals and MLOps skills](../skills/ml-fundamentals-and-mlops.md) · [Data and SQL questions](../topics/07-data-and-sql/README.md) · [LLM and generative AI](llm-and-generative-ai.md) · [Data engineering](data-engineering.md) · [Observability and reliability](observability-and-reliability.md)

## Types of learning

| Term | What it means |
|---|---|
| **ML** (Machine Learning) | Systems that learn patterns from data rather than following hand-written rules; useful when rules are too complex to write. |
| **Supervised learning** | Training on examples with known answers, such as spam labels; the most common type in business applications. |
| **Unsupervised learning** | Finding structure in unlabelled data, such as customer segments; useful for exploration when labels do not exist. |
| **Self-supervised learning** | Creating labels from the data itself, such as predicting hidden words; how embeddings and LLMs learn from raw text. |
| **Reinforcement learning** | An agent learns by taking actions and receiving rewards; suits sequential decisions like game playing and robotics. |
| **Classification** | Predicting a category, such as fraud or not fraud; typically trained with cross-entropy loss. |
| **Regression** | Predicting a continuous number, such as price or demand; typically trained with squared or absolute error loss. |
| **Clustering** | Grouping similar items without labels, for example grouping support tickets by theme. |
| **K-means** | A clustering algorithm that assigns points to the nearest of k centres; fast, but k must be chosen. |
| **Dimensionality reduction** | Compressing many features into fewer while keeping structure, for visualisation or efficiency; principal component analysis is the classic method. |

## Data splits and generalisation

| Term | What it means |
|---|---|
| **Training set** | The data a model learns from; performance here says little about how it will do on new data. |
| **Validation set** | Data for tuning settings and comparing models during development; repeated use slowly biases it. |
| **Test set** | Held-out data used once at the end, giving an honest estimate of performance on unseen data. |
| **Cross-validation** | Training and testing across several data splits for a more stable estimate; random splits are wrong for time series. |
| **Data leakage** | Training data containing information unavailable at prediction time; it creates great offline scores that collapse in production. |
| **Overfitting** | A model learns noise in the training data, scoring well there but poorly on new data. |
| **Underfitting** | A model too simple to capture the pattern, performing poorly on both training and new data. |
| **Bias** (in the bias-variance sense) | Error from overly simple assumptions, which makes a model consistently wrong in the same direction. |
| **Variance** | Error from being too sensitive to the particular training data, so predictions change a lot between samples. |
| **Bias-variance trade-off** | Simpler models tend to have higher bias, complex ones higher variance; the goal is the lowest total error. |
| **Regularisation** | Penalising model complexity to reduce overfitting; L1 can zero out features, L2 shrinks weights smoothly. |
| **Early stopping** | Ending training once validation performance stops improving; a simple and cheap guard against overfitting. |
| **Class imbalance** | One class being far rarer than others, as in fraud; handle with re-weighting, resampling, threshold tuning and suitable metrics. |

### Overfitting vs underfitting

| | Overfitting | Underfitting |
|---|---|---|
| Training error | Low | High |
| Validation error | High | High |
| Fixes | More data, regularisation, simpler model | More features, more complex model, longer training |

## Models and training

| Term | What it means |
|---|---|
| **Logistic regression** | A simple, interpretable classifier producing probabilities; often the right baseline to beat first. |
| **Decision tree** | A model of nested if-then splits; easy to explain, but a single tree overfits easily. |
| **Random forest** | Many trees trained on random samples, with results averaged; robust with little tuning. |
| **Gradient boosting** | Trees built in sequence, each correcting earlier errors; often the top performer on tabular data. |
| **Neural network** | Layers of weighted connections that learn complex patterns; strong on text, images and audio, but needs more data. |
| **Loss function** | The number training tries to minimise, measuring how wrong predictions are; must reflect what actually matters. |
| **Gradient descent** | Repeatedly adjusting weights in the direction that reduces loss; the core optimisation method for neural networks. |
| **Learning rate** | How large each weight update is; too high diverges, too low trains slowly. |
| **Batch size** | Number of examples used per update; affects memory use, training speed and gradient noise. |
| **Epoch** | One full pass over the training data; too many can cause overfitting. |
| **Hyperparameter** | A setting chosen before training, such as learning rate, rather than learned from the data. |
| **Contrastive learning** | Training so similar pairs sit close and different pairs sit apart; a common way to train embedding models. |
| **Transfer learning** | Reusing a model trained on one task as a starting point for another; needs less data and compute. |

## Metrics

| Term | What it means |
|---|---|
| **Confusion matrix** | A table of true and false positives and negatives; the starting point for most classification metrics. |
| **Accuracy** | Share of all predictions that are correct; misleading on imbalanced data, where always predicting the majority scores well. |
| **Precision** | Of items flagged positive, the share that truly are; prioritise it when false alarms are costly. |
| **Recall** (sensitivity) | Of truly positive items, the share found; prioritise it when missing a case is costly. |
| **F1 score** | The harmonic mean of precision and recall; a single balanced number, punishing a low value in either. |
| **ROC curve** (Receiver Operating Characteristic) | Plots true positive rate against false positive rate at every threshold, showing the trade-off across settings. |
| **AUC** (Area Under the Curve) | Summarises a curve as one number; ROC-AUC is the chance a random positive outranks a random negative. |
| **PR-AUC** (Precision-Recall Area Under the Curve) | Area under the precision-recall curve; more informative than ROC-AUC when positives are rare. |
| **Decision threshold** | The probability cut-off that turns a score into a decision; tune it to business costs, not a default 0.5. |
| **Calibration** | Whether predicted probabilities match reality, so 80% predictions come true about 80% of the time. |
| **MAE** (Mean Absolute Error) | The average size of errors in original units; easy to explain and less sensitive to outliers. |
| **RMSE** (Root Mean Squared Error) | Like MAE but penalises large errors more heavily; useful when big misses are especially costly. |

### Precision vs recall

| | Precision | Recall |
|---|---|---|
| Question answered | How many flagged cases are right? | How many real cases were caught? |
| Prioritise when | False alarms are costly, such as blocking genuine payments | Misses are costly, such as disease screening |
| Raised by | A higher threshold, usually | A lower threshold, usually |

## Features and drift

| Term | What it means |
|---|---|
| **Feature** | An input variable a model uses for predictions; feature quality often matters more than model choice. |
| **Feature engineering** | Creating useful inputs from raw data, such as a 30-day spend total; often the largest gain on tabular problems. |
| **Feature store** | A shared system that serves consistent features for training and inference; reduces skew and duplicate work. |
| **Point-in-time correctness** | Building training features using only values known at that moment; prevents future data leaking into training. |
| **Training-serving skew** | A difference between how features are computed in training and production; it quietly degrades live performance. |
| **Data drift** (covariate shift) | Input data distribution changes over time; models may degrade, and it is detectable without labels. |
| **Concept drift** | The relationship between inputs and the outcome changes, as when fraudsters adapt; usually needs labels to confirm. |
| **Label drift** | The distribution of outcomes changes, such as a rising fraud rate; thresholds and retraining may need revisiting. |
| **PSI** (Population Stability Index) | A common score measuring how much a feature's distribution has moved from a reference period; used to alert on drift. |
| **Delayed labels** | Ground truth arriving weeks after prediction; monitor inputs, score distributions and proxy metrics in the meantime. |

### Data drift vs concept drift

| | Data drift | Concept drift |
|---|---|---|
| What changes | The inputs | The input-to-outcome relationship |
| Detected by | Distribution tests on inputs | Performance against labels |
| Typical response | Investigate, possibly retrain | Retrain, often with new features |

## MLOps

| Term | What it means |
|---|---|
| **MLOps** (Machine Learning Operations) | Practices for reliably building, deploying and maintaining models in production; DevOps extended to data and models. |
| **Experiment tracking** | Recording parameters, code versions, data and metrics for each training run, so results can be compared and reproduced. |
| **Model registry** | A versioned catalogue of trained models with status and metadata; the control point for promotion and rollback. |
| **MLflow** | A popular open-source platform for experiment tracking, model packaging, registry and deployment. |
| **Data versioning** | Tracking which exact dataset produced a model; needed for reproducing, auditing and debugging results. |
| **Training pipeline** | An automated sequence of data preparation, training, evaluation and registration; replaces error-prone manual runs. |
| **Continuous training** | Automatically retraining on a schedule or trigger such as drift; keeps models current but must include evaluation gates. |
| **Model card** | A short document describing a model's intended use, data, performance and limits; supports governance and handover. |

## Model serving

| Term | What it means |
|---|---|
| **Inference** | Using a trained model to make predictions; its latency and cost shape how a model gets deployed. |
| **Batch inference** | Scoring many records on a schedule and storing results; cheap and simple when predictions can be stale. |
| **Online inference** | Scoring requests live behind an API; needed when decisions depend on fresh data, but must meet latency targets. |
| **Shadow deployment** | A new model scores live traffic without affecting users, so it can be compared safely against the current one. |
| **Canary release** | Sending a small share of traffic to a new model, then increasing if metrics hold; limits damage from a bad release. |
| **Model rollback** | Returning quickly to a previous version when a release underperforms; depends on registry versioning and compatible features. |
| **Dynamic batching** | Grouping incoming requests into one GPU run; raises throughput and lowers cost, with a small latency penalty. |
| **Cold start** | Delay while a scaled-to-zero model loads into memory; a trade-off against the saving from idle capacity. |
| **Model serving frameworks** | Software that exposes models as APIs, such as TorchServe, KServe, vLLM or managed cloud endpoints. Options change *(verify)*. |

### Batch vs online inference

| | Batch | Online |
|---|---|---|
| Latency | Hours or minutes | Milliseconds to seconds |
| Cost | Lower | Higher, must stay available |
| Example | Nightly churn scores | Payment fraud check |
