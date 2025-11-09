# SOFTWARE CRITICO TEMA 1
# Diseño de Software Crítico
 fault  -> error -> failure  
 Try to detect `error` and prevent `failure`.  (Once the system is running, it is too late to detect faults, but errors 
often leave observable traces)  
## Anomaly Detection
**Two types of anomalies can be identified:**
- `point anomalies`
- `contextual anomalies` (in the file descriptor example)  
### Supervised Anomaly Detection
(labeled dataset, where the anomalous instances are known)
- SupportVector Machines (SVMs)  
    - used for binary classification problems (sepatate two classes)
- DecisionTrees  
    - classification and regression problems (predict next value)
- Logistic Regression  
    - determine the probability that a given instance is anomalous  
    - used to model the relationship between a dependent variable and one (or more) independent variables

---
### Unsupervised Anomaly Detection
- Clustering  
    - group instances in a data set based on similarity  
    -  identify instances that do not belong to any of the clusters as anomalies (K means)
- Density-based methods  
    -  rely on the assumption that normal instances in the data form a dense cluster: DBSCAN
- Statistical methods  
    - assume that normal instances in the data follow a certain probability distribution
## Evaluation of Anomaly Detection Techniques
- `Precision` is the fraction of instances that are correctly identified as anomalies out of all instances that have been identified as anomalies. TP / (TP + FP)
- `Recall` is the fraction of instances that are 
correctly identified as anomalies out of all instances that are 
actually anomalies. TP / (TP + FN)  
**trade-off exists between them**  
`High precision` instances that are not actually
 anomalies but are NOT identified as such  
`High recall` means that the algorithm is able to identify more 
of the actual anomalies.  
- `Accuracy` the fraction
 of instances that have been correctly identified as normal or anomalous (can be misleading <= disbalance)  
- `F1 score` The F1 score is a metric that is used to balance precision and 
recall. (summarizes the balance between precision and recall. USEFUL)  

---
## Clustering
### K-means Clustering  
Each cluster is associated with a centroid:  
Outlier score defined as relative distance (the ratio  of the  distance of point  from the closest  centroid to the median distance of all points in the cluster from the centroid)

---
### Isolation Tree => Isolation Forest
### Machine Learning in Anomaly Detection
#### General approach:
building a model of normal behavior using available data -> anomaly score is then assigned to each data point 
**Autoencoders NN**  
**encoder -> low-dimensional representation (latente) -> decoder**  
- semi-supervised approach  
- where the reconstruction error is small for normal data samples, and large for abnormal data samples  
  
**LSTMs - Long Short Term Memorynetworks**  
- recurrent neural network (RNN) capable of learning long-term dependencies in sequence data
## Discrete – Time Markov Chains
### Markov Chain Definition
A stochastic `(changes randomly over time)` process { Xn } is called a Markov chain if :  
****The future behavior of the system depends only on the current state i and not on any of the previous states.****  
The one-step transition matrix for a Markov chain + states (130U or 130D)  
## Kalman Filters
**A Kalman filter is a predictor/corrector algorithm:**  
At time T, the algorithm predicts the value that will be received from the sensors at 
time T + 1, and, when the actual reading is received, it adjusts the predictor to make 
it more accurate next time.  
  
super-smart fusion center for data, helping you make the best possible guess about the true state of a system that is constantly moving and being measured imperfectly  
`ESTIMATE & UPDATE (real time)`  
#  Manejode errores: Rejuvenation y Bloques de Recuperación
## Error Handling: Rejuvenation
`press the RESET button from time to time`  
to intercept and remove errors before they become failures  
Most embedded systems have a natural operating periodicity
- An aircraft flies for no more than 36  hours
- a medical device is in continuous use for no more than a month  
demanding a periodic rebooting is considered failure for designers  
```
NO rejuvenation:

Stale -> Failed -> Fresh
  ^                  |
  +------------------+

REJUVENATION:

  +------Refresh-----+
  |                  ↓
Stale -> Failed -> Fresh
  ↑                  |
  +------------------+

```

When the system is stale, rejuvenation occurs, and this can avoid the 
move to the failed state.  
- The main problems associated with rejuvenation are determining the correct
 rejuvenation rate  

### Rejuvenation in Replicated Systems
-  rejuvenation may be triggered by time  
- or whenever an anomaly is detected in the hot (operating) subsystem  

### Recovery Blocks
 The basic pattern of a recovery block is that, instead of calling a particular function to carry out a 
computation, we instead structure the code as follows:  
```
ensure <acceptance test> by <f1>
else by <f2>
...
else bt <f3>
else ERROR
```
**One disadvantage:**  
any side effects (system changes) made by a routine must be undone  

## Diseño de Software Crítico. Replicación y diversificación  
`System will probably encounter conditions that it was not designed to handle`  
`behavior of the system under these conditions must be defined`
**Action to be taken is to move to **SAFE STATE****  
 1. It must be externally visible in an unambiguous manner
 2. It must be entered within a predefined time of the condition being detected
 3. It must be documented in the safety manual  

**`a move to the design safe state is not a dangerous failure`**  
## Recovery
-  to attempt to recover, for example, by using recovery blocks
- to move directly to the design safe state  
The intent is to try to `preserve as much of the state of the system as possible`, to 
`restrict the corruption` that could be caused by invalid 
information being transferred to another part of the system, and then to 
`restart and resynchronize the affected subsystems` with as little system 
level disruption as possible  

### Crash only model
**The crash-only model is self-explanatory:   on detecting an unanticipated condition, the system crashes and stops any form of further processing**  
`"Something is wrong, I'm stopping right now!"`






