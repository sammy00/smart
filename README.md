# BFT-SMaRt  

## Components  
Basically, 3 building blocks as  
+ network  
+ Consensus  
+ state machine   

### Network (i.e., the Communication System in the paper)
This block is responsible of  
+ receive requests from clients  
+ receive messages from other replicas  
+ send messages to other replicas   

and consists of 3 modules as  
+ **C-R Net** (i.e., the Client Communication System in the paper)  
  - connect to replicas  
  - send requests to replicas  
  - receive responses from replicas  
+ **Request Pool** (a.k.a. the Client Manager)  
  - receive requests from clients  
  - verify requests  
  - log requests for each client, including  
    + last sequence number to prevent replay attack  
    + last reply for retransmissions  
    + pending requests  
+ **R-R Net** (i.e., the Server Communication System in the paper)  
  + implements a closed-group net to exchange messages between replicas  
  + connection restablish??

### Consensus  
The messages exchanging routine of this block is delegated to the `Network` block.  
Specifically, the consensus block comprises of 6 modules as  
#### Proposer  
##### function
- package the *PROPOSE* message  
- response as a leader  
##### trigger  
- replica is the **leader** for the next consensus instance 
- previous consensus instance is done
- has messages to ordered  

#### Core (a.k.a. Acceptor)  
  - PROPOSE  
  - ACCEPT  
  - WRITE  

#### C-R Exchange (a.k.a. Total Order Multicast, abbr. TOM)
  - kickstart the proposer module with a message from C-R net  
  - deliver request to replica  
  - manage timer for pending messages  

#### Execution Manager  
  - store information about consensus instance, such as view and primary  
  - start/stop/re-start a consensus  

#### View Changer (a.k.a. Leader Change Manager)  
  - validate requests (pending or executed) sequentially  

#### Reconfiguration Manager  
  - provide a consistent view of the group of replicas and number of tolerated faults  