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
#  Manejo de errores: Rejuvenation y Bloques de Recuperación
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

# Replication and Diversification
-  `replication` deploys two or more identical copies of a system
- `diversification` incorporates different implementations 
of the same function  

to increase either the 
availability or the reliability  
## Component or System Replication
`in general, replicating components is better than replicating systems`  
**The Structure Function:**
N components can be described by a vector of N elements, each 
representing the state (1 = functioning, 0 = failed) of one component  
Ф(1, 0, 1) = 1
Ф(0, 0, 1) = 0  

---
+ Components in Series (product of Xs)
+ components in parallel (1 / product of Xs)  

`k-out-of-n structure (KooN)` functioning if and only if at least k of the n components are functioning:  
```
NooN -> structure is a series structure of n components
1ooN -> structure is a parallel structure of n components
```
# System Replication
`often the increased complexity of replication outweighs the advantages`  
**NEEDED:**
- selector
- State transfer and synchronization  
### Cold Stand By
only one of the subsystems is operating and the 
other is simply monitoring its behavior, ready to take over  
- additional potential failure points
-  difficult to scale 
## Time Replication
most bugs encountered are Heisenbugs => simply running each 
computation twice and comparing the answers  
**The system runs more slowly, but more reliably**  
## Diversification: Hardware Diversity  
two subsystems to be running on different processors (can avoid Heisenbugs)  
`we do not need diversity (different code) to protect against Heisenbugs, we can use replication (same code)`  
**`Diversification of hardware would be effective against Bohrbugs`** (very rare)  

## Software Diversity
- ***Code-level Diversity*** the simplest is to use a tool to transform one source code into an equivalent one
- ***Coded Processors*** (The Self-Checking Computer)  
The concept of Coded Processors takes the idea of having diverse (different) software and applies it right down to how the computer handles data. It's like giving every piece of data a secret security tag that the computer is constantly checking.  
`Incorrect Compilers, Hardware Errors (Bit Flips), computational errors`  
### N-Version Programming
`the independent generation of N >=2 functionally equivalent programs`  
programming efforts are carried out by N individuals that do not interact (different algorithms and programming languages in each effort)  
- many faults are introduced due to ambiguity in 
the specifications that are common to all N teams  
## Data Diversity
`store the same information using two or more different data representations`  
- defense against both Bohrbugs and Heisenbugs in programs  
## Active Replication (also called group synchrony)
-  form of replication (or diversification)  

(Servers join groups but client thinks there is one server)  

- if one member of the group sees that server X has left the group and then sees client Y's 
request, then all members of the group will see those two events in that order  
- This is virtual synchrony, not locked-step operation  
-  For some systems, the requirement for common arrival order of all events at each server in the group can be relaxed and different types of 
guaranteed can be provided:  
    - Sender order
    - Causal order
    - Total or agreed order  
  
**several technical solutions and implementations of 
this approach in the form of `middleware` of `virtualization` 
platforms**  
  
**`divide and conquer is really the only option`**  
**`services slow down at scale`** because of:  
- Lock contention (more concurrent task will probably access the same object) 
- Abort
- Deadlock

## Sharding! (devide into shards of few servers (assigned by hash of key))
- Each transaction accesses just one shard
- No locks
- NO 2-phase commit are require
- scales very well
- state machine replication within the shard
### State Machine Replication
`This is a model in which we can read from any replica`
- update -> an atomic multicast or a durable Paxos write `e replicas see the same updates in the same sequence`  

---
- Transactions that touch multiple shards need locks and 2-phase commit  
### Virtual Synchrony
Virtual synchrony middleware
1. ->  replicates and orders 
the incoming requests  
->  ensures that each of the four server instances 
receives the requests in the same order  
2. Each server process and gives the answer to virtual synchrony middleware, which manages the answers and send smth back to client

### Advantages of **Active Replication**
- It is tolerant to Heisenbugs
- It scales well (linearly)
- It can be tuned to provide reliability or availability
- **Silent failure (the main disadvantage of cold-standby) is avoided** (All instances of the 
server are active all the time)
- **The selector or switch is removed**
-  Statetransfer is avoided (except for new members)
### Disadvantages of **Active Replication**
- **The whole concept of group membership is technically very complex**
  - `!! CAP theorem and FLP impossibility result`

# Locked step processors
two processors running 
the same code in locked step, each executing the same machine code 
instruction at the same time  
`NOW IT'S USELES`  
`(8nm) => Lockedstep processing may again become useful`
## Diverse Monitor
- The main system performs its function of reading input values and transforming 
these into output values
- The monitor observes the inputs and outputs, and may also observe internal states 
of the main system
- The difference between the main system and the monitor is that the main system 
is much more complex
### Watchdog Diverse Monitor
- The main system is required to “kick“ the watchdog (the 
monitor) periodically 
- watchdog notices that it hasn't been kicked for a pre
determined time, then it “barks“ => SAFE STATE
- difficulties: Place and time of kick

---
# Architectural Balancing
- Usefulness/Safety Balance

---
---
# CAP Theorem
`It is impossible for a web service to provide following three guarantees at the same time`  
- `Availability` (Node failures do not prevent survivors from continuing to operate)
- `Consistency` (All nodes should see the same data at the same time)
- `Partition-tolerance` (The system continues to operate despite network partitions)
  
**At most can be sutisfied two of them**

