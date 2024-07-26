# AnyLoss
## |Paper| 
* AnyLoss: Transforming Classification Metrics into Loss Functions (https://arxiv.org/abs/2405.14745) 
* Generating a loss function that aims at any confusion matrix-based evaluation metric (e.g., f1 score, balanced accuracy, etc.) in binary classification tasks.

## |How to use|
### 1. The code of AnyLoss in Single-Layer Perceptron (SLP) and Multi-Layer Perceptron (MLP) is shown below.
* The code in detail: AnyLoss - Code.ipynb
* For Quick Use (Python Code with Tensorflow): Define AnyLoss using "def" and put the loss name in the "[   ]" when compiling.
  
```
import tensorflow as tf
def Ours_Accu(y_true, y_pred):
    y_pred = 1/(1+tf.math.exp(-L*(y_pred-0.5)))
    yl = y_train.shape[0]
    accu = (yl-tf.reduce_sum(y_true)-tf.reduce_sum(y_pred)+2*tf.reduce_sum(y_true*y_pred)) / yl
    return 1-accu

def Ours_Fbeta(y_true, y_pred):
    beta = 1  # 1=F1, 0.5=F0.5, 2=F2... 
    y_pred = 1/(1+tf.math.exp(-L*(y_pred-0.5)))
    numerator = (1+beta**2)*tf.reduce_sum(y_true*y_pred)
    denominator = (beta**2)*tf.reduce_sum(y_true) + tf.reduce_sum(y_pred)
    return 1-(numerator/denominator)

def Ours_Gmean(y_true, y_pred):
    y_pred = 1/(1+tf.math.exp(-L*(y_pred-0.5)))
    syhy = tf.reduce_sum(y_true*y_pred)
    sy = tf.reduce_sum(y_true)
    yl = (y_train.shape[0])
    gmean = tf.sqrt(syhy*(yl-tf.reduce_sum(y_pred)-sy+syhy)/(sy*(yl-sy)))
    return 1-gmean

def Ours_BAccu(y_true, y_pred):
    y_pred = 1/(1+tf.math.exp(-L*(y_pred-0.5)))
    syhy = tf.reduce_sum(y_true*y_pred)
    sy = tf.reduce_sum(y_true)
    yl = y_train.shape[0]
    baccu = (yl*(syhy+sy)-sy*(tf.reduce_sum(y_pred)+sy)) / (2*sy*(yl-sy))
    return 1-baccu

L = 73   # Hyper Parameter, positive real number but integer used for convenience.
model.compile(loss=[  ], optimizer=opt, metrics=['accuracy']) # [  ]: MSE // BCE // Ours_Accu // Ours_Fbeta // ...
```

### 2. The code of experiments with 102 diverse datasets in the paper is shown below. (Section 4.1 in the paper)
* 4-30: (MSE/BCE/AnyLoss) Performance on 102 Datasets - SLP
* 4-40: (MSE/BCE/AnyLoss) Performance on 102 Datasets - MLP
### 3. The code of experiments with 4 imbalanced datasets in the paper is shown below. (Section 4.2 in the paper)
* 4-10: (MSE/BCE/AnyLoss) Performance on 4 Datasets - SLP
* 4-13: (SOL) Performance on 4 datasets - SLP
* 4-20: (MSE/BCE/AnyLoss) Performance on 4 Datasets - MLP
* 4-22: (SOL) Performance on 4 Datasets - MLP
### 4. The code of experiments for comparison with the resampling strategy. (Section 4.2 in the paper)
* 4-11: (BCE_SMOTE/BCE_RandomUndersampling/AnyLoss) Comparison on 4 Datasets - SLP
### 5. The code of experiments for measuring learning time. (Section 4.3 in the paper)
* 4-51: (MSE/BCE/AnyLoss) Learning Speed on 4 Datasets - SLP
* 4-52: (SOL) Learning Speed on 4 Datasets - SLP
* 4-62: (MSE/BCE/AnyLoss/SOL) Learning Speed on 4 Datasets - MLP
### 6. Misc.
* Datasets (breast_cancer, diabetes_prediction_dataset) are too big to upload and they can be found at Kaggle. (See the reference in the paper)
* 102 datasets: 'data_num' folder
* Bayesian Sign Test: bayesiantests.py and BAYES_Mine.ipynb
* Additional Information for the paper

### 7. Additional work for the section 5 in the paper

[Description of 102 Diverse Datasets]
  
| **Dataset** | **#Sample** | **#Feature** | **Class Dist ('0':'1')** | **Dataset** | **#Sample** | **#Feature** | **Class Dist ('0':'1')** |
|:--------:|:--------:|:---------:|:--------:|:--------:|:--------:|:---------:|:--------:|
|	1	|	250	|	12	|	0.64	:	0.36	|	52	|	990	|	13	|	0.91	:	0.09	|
|	2	|	250	|	9	|	0.62	:	0.38	|	53	|	1000	|	19	|	0.70	:	0.30    |
|	3	|	306	|	3	|	0.74	:	0.26	|   54	|	1000	|	20	|	0.74	:	0.26    |
|	4	|	310	|	6	|	0.68	:	0.32	|	55	|	1043	|	37	|	0.88	:	0.12	|
|	5	|	320	|	6	|	0.67	:	0.33	|	56	|	1055	|	32	|	0.66	:	0.34	|
|	6	|	327	|	37	|	0.87	:	0.13	|	57	|	1066	|	7	|	0.83	:	0.17	|
|	7	|	328	|	32	|	0.69	:	0.31	|	58	|	1074	|	16	|	0.68	:	0.32	|
|	8	|	335	|	3	|	0.85	:	0.15	|	59	|	1077	|	37	|	0.88	:	0.12	|
|	9	|	336	|	14	|	0.76	:	0.24	|	60	|	1109	|	21	|	0.93	:	0.07	|
|	10	|	349	|	31	|	0.62	:	0.38	|	61	|	1156	|	5	|	0.78	:	0.22	|
|	11	|	351	|	33	|	0.64	:	0.36    |	62	|	1320	|	17	|	0.91	:	0.09	|
|	12	|	358	|	31	|	0.69	:	0.31	|	63	|	1324	|	10	|	0.78	:	0.22	|
|	13	|	363	|	8	|	0.77	:	0.23	|	64	|	1458	|	37	|	0.88	:	0.12	|
|	14	|	365	|	5	|	0.92	:	0.08	|	65	|	1563	|	37	|	0.90	:	0.10	|
|	15	|	381	|	38	|	0.85	:	0.15	|	66	|	1728	|	6	|	0.70	:	0.30	|
|	16	|	392	|	8	|	0.62	:	0.38	|	67	|	1941	|	31	|	0.65	:	0.35	|
|	17	|	400	|	5	|	0.78	:	0.22	|	68	|	2000	|	6	|	0.90	:	0.10	|
|	18	|	403	|	35	|	0.92	:	0.08	|	69	|	2000	|	76	|	0.90	:	0.10	|
|	19	|	450	|	3	|	0.88	:	0.12	|	70	|	2000	|	216	|	0.90	:	0.10	|
|	20	|	458	|	38	|	0.91	:	0.09	|	71	|	2000	|	47	|	0.90	:	0.10	|
|	21	|	462	|	9	|	0.65	:	0.35	|	72	|	2000	|	64	|	0.90	:	0.10	|
|	22	|	470	|	13	|	0.85	:	0.15	|	73	|	2000	|	239	|	0.90	:	0.10	|
|	23	|	475	|	3	|	0.87	:	0.13	|	74	|	2000	|	139	|	0.72	:	0.28	|
|	24	|	475	|	3	|	0.87	:	0.13	|	75	|	2000	|	140	|	0.87	:	0.13	|
|	25	|	500	|	25	|	0.61	:	0.39	|	76	|	2001	|	2	|	0.76	:	0.24	|
|	26	|	500	|	22	|	0.84	:	0.16	|	77	|	2109	|	20	|	0.85	:	0.15	|
|	27	|	504	|	19	|	0.91	:	0.09	|	78	|	2201	|	2	|	0.68	:	0.32	|
|	28	|	522	|	20	|	0.80	:	0.20	|	79	|	2310	|	17	|	0.86	:	0.14	|
|	29	|	531	|	101	|	0.90	:	0.10	|	80	|	2407	|	299	|	0.82	:	0.18	|
|	30	|	540	|	20	|	0.91	:	0.09	|	81	|	2417	|	115	|	0.74	:	0.26	|
|	31	|	559	|	4	|	0.86	:	0.14	|	82	|	2534	|	71	|	0.94	:	0.06	|
|	32	|	562	|	21	|	0.84	:	0.16	|	83	|	3103	|	12	|	0.93	:	0.07	|
|	33	|	569	|	30	|	0.63	:	0.37	|	84	|	3103	|	12	|	0.92	:	0.08	|
|	34	|	583	|	10	|	0.71	:	0.29	|	85	|	4052	|	5	|	0.76	:	0.24	|
|	35	|	593	|	77	|	0.68	:	0.32	|	86	|	4474	|	11	|	0.75	:	0.25	|
|	36	|	600	|	61	|	0.83	:	0.17	|	87	|	4521	|	14	|	0.88	:	0.12	|
|	37	|	609	|	7	|	0.63	:	0.37	|	88	|	4601	|	7	|	0.61	:	0.39	|
|	38	|	641	|	19	|	0.68	:	0.32	|	89	|	4859	|	120	|	0.83	:	0.17	|
|	39	|	645	|	168	|	0.94	:	0.06	|	90	|	5000	|	40	|	0.66	:	0.34	|
|	40	|	661	|	37	|	0.92	:	0.08	|	91	|	5000	|	19	|	0.86	:	0.14	|
|	41	|	683	|	9	|	0.65	:	0.35	|	92	|	5404	|	5	|	0.71	:	0.29	|
|	42	|	705	|	37	|	0.91	:	0.09	|	93	|	5473	|	10	|	0.90	:	0.10	|
|	43	|	748	|	4	|	0.76	:	0.24	|	94	|	5620	|	48	|	0.90	:	0.10	|
|	44	|	768	|	8	|	0.65	:	0.35	|	95	|	6598	|	169	|	0.85	:	0.15	|
|	45	|	797	|	4	|	0.81	:	0.19	|	96	|	6598	|	168	|	0.85	:	0.15	|
|	46	|	812	|	6	|	0.77	:	0.23	|	97	|	7032	|	36	|	0.89	:	0.11	|
|	47	|	841	|	70	|	0.62	:	0.38	|	98	|	7970	|	39	|	0.93	:	0.07	|
|	48	|	846	|	18	|	0.74	:	0.26	|	99	|	8192	|	12	|	0.70	:	0.30	|
|	49	|	958	|	9	|	0.65	:	0.35	|	100	|	8192	|	19	|	0.70	:	0.30	|
|	50	|	959	|	40	|	0.64	:	0.36	|	101	|	8192	|	32	|	0.69	:	0.31	|
|	51	|	973	|	9	|	0.67	:	0.33	|	102	|	9961	|	14	|	0.84	:	0.16	|

[Performance with different L on 4 Datasets]

|     **Dataset**      | **L=30** | **L=50**  | **L=73**  | **L=90**  | **L=110** |
|:--------------------:|:--------:|:---------:|:---------:|:---------:|:---------:|
|  **1. Random**       |          |           |           |           |           |
|       Accuracy       |  0.915   |   0.923   | **0.924** |   0.895   |   0.922   |
|      F-1 score       |  0.636   |   0.632   | **0.637** |   0.634   |   0.635   |
|      B_Accuracy      |  0.840   |   0.834   | **0.852** |   0.826   |   0.825   |
|  **2. Credit Card**  |          |           |           |           |           |
|       Accuracy       |  0.952   |   0.994   | **0.995** |   0.994   |   0.994   |
|      F-1 score       |  0.935   |   0.942   | **0.945** | **0.945** |   0.944   |
|      B_Accuracy      |  0.950   |   0.915   | **0.962** |   0.956   |   0.871   |
| **3. Breast Cancer** |          |           |           |           |           |
|       Accuracy       |  0.998   |   0.995   | **1.000** |   0.997   |   0.997   |
|      F-1 score       |  0.980   |   0.980   | **0.989** |   0.980   |   0.969   |
|      B_Accuracy      |  0.949   |   0.950   | **1.000** |   0.989   |   0.996   |
|    **4. Diabete**    |          |           |           |           |           |
|       Accuracy       |  0.948   | **0.961** | **0.961** | **0.961** | **0.961** |
| F-1 score            |  0.728   |   0.733   | **0.736** |   0.735   |   0.734   |
| B_Accuracy           |  0.837   |   0.859   | **0.882** |   0.832   |   0.826   |

[Win number with different L on 102 Datasets]

|  **Win Number**  | **L=30** | **L=50**  | **L=73**  | **L=90**  | **L=110** |
|:--------------------:|:--------:|:---------:|:---------:|:---------:|:---------:|
|   AnyLoss-Acc   |  21   |   18   | **31** |   21   |   24   |
|   AnyLoss-F1    |  21   |   19   | **27** |   21   |   16   |
|   AnyLoss-B_Acc |  17   |   20   | **26** |   17   |   23   |

[Performance with ResNet on MNIST Dataset]

| **Metrics** |  **MSE**  |  **BCE**  |     **AnyLoss**      |
|:-----------:|:---------:|:---------:|:--------------------:|
|Accuracy     |   0.994   |   0.994   |**0.998** (${L_A}$)   |
|F1           |   0.965   |   0.965   |**0.980** (${L_{F1}}$)|
|Balanced Acc.|   0.972   |   0.970   |**0.980** (${L_B}$)   |

[Performance on Huge Titanic Dataset]

|**Structure**|***SLP***|     |       |***MLP***|     |       |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|**Metrics**|**MSE**|**BCE** |**AnyLoss**|**MSE**|**BCE** |**AnyLoss**|
|Accuracy     |0.800|0.806|**0.814** (${L_A}$)|0.807|0.814|**0.822** (${L_A}$)|
|F1           |0.726|0.734|**0.739** (${L_{F1}}$)|0.730|0.729|**0.746** (${L_{F1}}$)|
|Balanced Acc.|0.780|0.786|**0.788** (${L_B}$)|0.783|0.784|**0.796** (${L_B}$)|
