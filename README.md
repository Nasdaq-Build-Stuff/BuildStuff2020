# BuildStuff 2020 - Design and win PlayStation 5!

## Rock music festival ticket sales system

Below is a list of requirements for a distributed system. 
Your task is to propose a design that would fulfil the requirements as closely as possible. 
 
The document with design proposal should be sent via email: <BuildStuffTask@nasdaq.com>

### Overview and scope

A rock musing festival is being organized. In preparation for the festival a new ticket 
sales system will be developed:

- In total there will be around 1 million tickets, and it is anticipated that demand will be very high
    - All tickets will be sold in a few hours or less
- Ticket sales system must be distributed and deployed globally in datacenters around the world.
- Users living in different geographical regions will be connected to different 
datacenters (and thus different distributed parts of the system)
- Number of users, wanting to buy tickets, will be similar (on average) in all regions
    - All datacenters would see similar number of requests

## Requirements

### Functional requirements

- **F1**: each sold ticket must be logged - it must be possible to check a) who, and b) when 
bought the ticket.
    - Clarification: Definition of "who" and "when" is up to you; but "when" must at least be a date 
    including time up to second precision
- **F2**: it must be possible to query how many unsold tickets are left in the system. 
    System must return the answer within 10 seconds.
    - Clarification: because of distributed manner, and because network partitions might happen
     anytime, system may answer with "Failure"

### Non-functional requirements

All non-functional requirements below are marked as either "hard req" or "soft req". 
"hard req" means that design must implement this requirement fully. "soft req" means that 
some deviations are allowed, but it must be documented what kind of deviations and why.

- **NF1**: No single point of failure, especially DB (hard req) 
- **NF2**: One and the same ticket must not be sold to different customers (soft req)
- **NF3**: System should be fair (soft req)
    - "First come, first served" - if several customers are competing for one remaining ticket, 
    customer who was the first should win (not customer who is closest to DB where ticket is stored)
        - please propose the definition of "first" in your design 
    - if tickets allocated to one region are sold out, available tickets in the
    other region should be sold.
- **NF4**: System should be deployed in at least 5 datacenters around the world (hard req) 
- **NF5**: Failure of some datacenter must not render the whole system unavailable (hard req)
    - Clarification: system may be unavailable for users that are geographically close to the failed 
    datacenter. But for users, connected to other datacenters (not failed) system must be available.
- **NF6**: System should be designed to work in case of network partitions (hard req)
    - At any moment any number of datacenters could become temporarily unreachable for other datacenters
     (till network connectivity is restored)
    - At any moment any _group_ of regional datacenters could be temporarily isolated (loose network connectivity) from all remaining datacenters.
        - But network connectivity _within_ the group is OK.
    
### Out of scope
The following areas/functions are out of scope:

- User authentication
- Payment processing
- Connecting user to closest datacenter
- Ticket segregation ("sitting/standing places", "sectors", etc.)
    - Consequently, all tickets are "equal" - user does not need to pick category/type of ticket.

### Evaluation criteria:

**You don‘t have to solve all the problems - just try to collect as many points as you can!**  

Up to 3 points are given for each requirement (functional and non-functional).
In addition, points will be given for showing how the design answers these questions/issues 
(one point per each question):

- **C1**: Can _conflict-free replicated data types_ (CRDT) be used for this task? If not, why?
- _Availability_ versus _consistency_ (in case of network partition occurrence): 
    - **C2**: What happens if some datacenter crashes or becomes unreachable to other datacenters?
    - **C3**: Does the system give preference to availability or consistency during such failures? 
    - **C4**: How not so sell the same ticket twice?
- **C5**: What technology stack would you use? Is there anything suitable in cloud providers and 
    how could it be used?
- **C6**: How would the system behave in split-brain scenario? 
(For example, North America region with 3 datacenters loses all the connectivity to Europe region 
with 3 datacenters)
    - Ticket sale stops till split-brain is resolved
    - Ticket sale continues in some datacenters but not others (which ones exactly?)
    - other options?
- **C7**: If F2 requirement implementation returns approximate result, can system estimate the accuracy? For example: "number of unsold tickets at the moment is 200000 plus/minus 1000". 
 How can the system estimate this "1000"?
- **C8**: F2 requirement, 10 second latency - can we design for lower latency? 
What constraints are limiting factors?

### The document with design proposal should be sent via email: <BuildStuffTask@nasdaq.com>
