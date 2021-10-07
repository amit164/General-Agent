# General-Agent

General agent that operates in multiple domains and carries on a variety of tasks in these domains.

1. [Introduction](#introduction)  
2. [Agent Action Method](#Agent-Action-Method)
    *  [Learning](#Learning)
    *  [Execute](#Execute)
3. [Fast-Downward Planner](#fast-downward-planner)
4. [Dependencies](#dependencies)
5. [Installation](#installation)

## Introduction
The agent receive a domain description and task description in PDDL and to solve the task as fast as possible. 
Note: the agent can not assume anything about the environment.
The agent faces with deterministic and probabilistic domains, domains with multiple goals and even with revealed features of the environment. This agent uses the `fast-downward planner` that uses domain-independent planning heuristics and able to solve a variety of domains. The agent plan it's path by using the planner and replan if needed. The idea behind it is that most of the "wrong" actions can be fixed and the agent can return to the "right path" easily.


## Agent Action Method:
As mentioned, the agent uses the `fast-downward planner` which can solve many tasks in a spesific format. In order to do so, the agent distinguishes between deterministic and probabilistic domains. It sends the planner new domain and problem files and change the effects of the actions if necessary. 

The agent opens/create a file in which it saves all the data it learns. The agent maintain a dictionary that the keys are the locations in the maze and the value is another dictionay. The keys of the sub-dicionary is the possible actions in each location and the values are the Q-Value calculated based on the formula:

<div align="center"> <em> Q(s,a) = Q(s,a) + learning_rate * [reward + discaunt_factor * Q(current_location, max_possible_action) - Q(s,a)] </em> </div><br/>

At the function ```next_action()```, the agent updates the q-table according to the formula above. Then, in order to balance exploration and exploitation I chose to implement the agent to use Epsilon-Greedy - with a probability of 0.9 the agent picks the the max action in the q-table and with probability of 0.1 the agent picks a random action.

##### Rewards
A big part of rainforsment learning is to chose the reward to each action. The ```get_rward()``` function works by this policy:
* If the agent is in the food location, the reward is _1000_.
* If it is a location the agent has already have been, the reward is _-1 * the number of times it visits this location_
* Any other case, the rears is _-1_.

#### LearningPolicyExecutor:
This agent can executed only **after** a the Q-LearningExecutor. It choses the max action in each state, avoides infinity loops and also use the Epsilon-Greedy Action Selection. 

You can see more information about the learning process [here]().

## Dependencies:
* Ubuntu16 O.S
* python 2.7.18
* pddlsim (more info [here](https://bitbucket.org/galk-opensource/executionsimulation/src/master/))

## Installation:
1. Clone the repository:  
    ```
    $ git clone https://github.com/amit164/ex4-Intro-to-Intelligent-Systems.git
    ```
2. Write those commands in the terminal:
    ```
    $ cd ex4-Intro-to-Intelligent-Systems
    $ python my_executive <flag> <domain_file> <problem_file> <POLICYFILE>
    ```
    > _flag_ is for running the Q-LearningExecutor (-L) or the LearningPolicyExecutor (-E). 
    
    > _domain_file_ is the domain uploaded here (maze_domain_multi_effect_food.pddl).
 
    > _problem_file_ is a problem file for the domain you chose. You can use one of those here or create one of your own.
    
    > _POLICYFILE_ is the file name in which the agent will save the best policy.
   

