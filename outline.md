# Introduction
## Personal Introduction (This isn't read word for word by is summarized)
### Educational Background
- B.S. in Theoretical Mathematics
- B.S. in Applied Physics
- Currently working on M.S. in Applied Physics (Quantum Computing)

### Served as network/software engineer at the NASA Glenn Research Institute (federal contractor)
  - Worked on Cognitive Networks
  - Visualizing extraterrestrial networks (health, statistics, configuration, etc)
  - Built SaaS versions of complex engineering tools

### Expedia Group
- Pricing/Inventory Space
- Currently, serving as tech lead for fencing
- Main areas of work: Data Engineering, 

## Context
### Today's Topic
- I'll be reviewing an 8-month initiative I led/leading to overhaul a streaming service.
- It'll be broken up into two parts 
  - Part I: Stabilize and productionalize a stale pipeline
  - Part II: Design and propose an alternative solution
- Involvement Transparency 
  - I was part of the original team that created the pipeline but had been mostly hands-off until these incidents
  - Due to the urgent nature of this work, most of this work bypassed the normal intake process, so my 
    involvement fluctuates over the course of this work. This was also worked on concurrently with my normal 
    day-to-day responsibilities.

<br/><br/><br/>

# Part 0: The Pipeline
## What the pipeline does
- Currently, the stream the future availability of all hotels on Expedia in a single datalake. 
- As the domain owners, we were responsible for owning the inventory domain portion of the pipeline.
- Show high level diagram of pipeline

## Business Justification
- SEO bidding ($8-12 million dollars in savings)
- Unlocking our data for numerous ML projects
- Provided an aggregate view into availability without making computationally expensive calls

## Challenges Pipeline
- Availability is a complex value that needs to be calculated 
  - Each hotel has certain types of rooms which have certain rate plans and etc... 
  - Availability is computed using things like length of stay, check-in, and etc.. 
  - Consumes 10 streams and still requires generalizations to be made
- Catering to this complexity, the original design has a lot of moving parts
  - ~10 teams own components in the larger end-to-end solution
  - Leverages shared resources
- Dealing with a large amount of data
  - Pull metrics


<br/><br/><br/>


# Part 1: Taming an Unruly Pipeline 
## Initial Incident Context
The pipeline was turned off for the first half of covid-19, as a cost saving metric. Once it was turned back on it 
became progressively more and more unstable. It was plagued by:
  - A slow leakage of data
  - Poor Monitoring
  - Sporadic Outages leading to data loss
  - Volatile Accuracy
  - Data Misuse

Then there was a large outage that caused severe degradation for clients. Sadly, when the project was turned back on 
after its hiatus, not all of the alerts were not re-enabled causing a lot of the metrics to be lost due to data 
retention. So this wasn't going to be an easy investigation.

## Stabilization Initiative Goals 
- Get the pipeline functional again ASAP 
- Led the initial investigation and stabilization
- Picked off the high priority low-hanging fruit
- Served as tech lead for the feature squad

## Highlights of interest
### Sporadic Outages
- We made multiple cross region requests that resulted in timeouts and consumers to die
  - kafka rebalancing issue
- The shared resource environment was not the same post covid

### Accuracy Assumptions
- The existing solution used panel sampling
  - From a pipeline owner this makes sense as panel sampling is used to measure fluctuations over time
- I redesigned the next generation accuracy app that included random sampling

### Data Misuse
- Out of this incident we learned that a high priority team had misunderstood the accuracy metric and 
  began using the data produced by this Proof of Concept.
- This will be dug into in the next phase of this presentation.

## Actions and Results
- Gave a tech talk on my findings and the future work
- Helped the feature squad drive this to completion by
  - Onboarded the team to the pipeline 
  - Creating and prioritizing a backlog of tasks
  - Thoroughly reviewing changes and design docs
- The pipeline has since been stabilized and hasn't had any major issues since this initiative 


<br/><br/><br/>


# Part 2: building the future
## Tasked 
- Led the design effort to let selection use 
- Design a system that can be easily supported 
- Redesigning Principles
  - Simplicity
    - The previous pipeline had snowballed into a logistical nightmare which made supporting it challenging
    - Too many cooks issue
    - This meant that we would need to make generalizations at the expense of edge case accuracy
  - React-ability
    - The current pipeline was extremely slow moving
    - The new solution should leverage acceptable errors (false available)
  - Don't Duplicate Domain Logic
    - The existing solution duplicated the domain logic

## Examining the data sources
### Natural Traffic vs Consuming the DB deltas

## Examining the change propagation
### Push vs Pull 

## Resulting Design
- Walk through abstracted system diagram
This design uses natural search traffic 

## Hidden Challenges
- Sparse and/or Stale data
  - Since this data is being generated by natural traffic, there may be infrequently search combinations
    that won't be represented in the data
- Potentially cannibalizing the search results
  - If we stop searching for a hotel, it won't populate our search traffic. Meaning that if a hotel is sold out, 
    we'd miss if it became available (cancellations, etc.)
- cache invalidation
  - randomly purge data
  - cache ttl

## Current State/Result
- The new design reduces the 
- The design has been reviewed/approved
  - I plan on releasing a proof of concept in early Q1 2022
  - The implementation is slotted for Q3 2022
  
<br/><br/><br/>

# Conclusion
- Led a successful stabilization and productionalization effort 
- Led a cross-org design effort
- Designed a cross-org solution
- Served as tech lead for implementation
