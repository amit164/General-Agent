
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
The agent receive a domain description and task description in PDDL and it's goal is to solve the task as fast as possible. 
Note: the agent can not assume anything about the environment.
The agent faces with deterministic and probabilistic domains, domains with multiple goals and even with revealed features of the environment. This agent uses the `fast-downward planner` that uses domain-independent planning heuristics and able to solve a variety of domains. The agent plan it's path by using the planner and replan if needed. The idea behind it is that most of the "wrong" actions can be fixed and the agent can return to the "right path" easily.


## Agent Action Method:
As mentioned, the agent uses the `fast-downward planner` which can solve many tasks in a spesific format. In order to do so, the agent distinguishes between deterministic and probabilistic domains. It sends the planner new domain and problem files and change the effects of the actions if necessary to the effect with the highest probabilistic. 
In order to find hidden objects, the agent uses a parser and run the planner with a problem file in which the goal is the condition that reveal the object. We assume that the hidden object is necessary to reach the goal and there exist only one hidden object.

The agent writes the planner's output to a file and read the actions the planner did from it. The agent act by this chart:
![Chart](https://pdf.ac/Kuyc1)

The agent saves a list of all the actions in the planner's output file and work by it: the agent tries the next action on the list, if succeeded the agent repeats to try the next action until it reaches the goal. Otherwise, `current_state != expected_state`, i.e., the action that the planner did at this point led to anouther state from the state that reached by the action that the agent did, the agent replan.

#### Replan
The agent runs the planner again while choosing the effect of any probabilistic action in random and changing the initial state to the current state.

### Learning:
The fast-downward planner uses a search algorithm in order to complete the task. It can recive as an argument the search algorithm, `a-star` or `lazy-greedy` for example. 
Every time that the agent runs the planner, it chooses an algorithm that it never ran before. 
The agent opens/create a file for each domain in which it saves all the data it learns. The agent maintain a dictionary that the keys are the problem name and the value is a list with 3 elements: 
   1. A list of all the action that the agent made on the best execution.
   2. The search algorithm on the best execution.
   3. All algorithms that the agent ran.

### Execution:
If the domain or problem has never learned before, the agent will proccess the learning phase. Otherwise, it chooses the best algorithm it knows and tries to follow the actions one by one to reach the goal. If at some point the `current_state != expected_state` the agent will replan again by the best algorithm it knows. 

## Fast-Downward Planner:
"Fast Downward is a classical planning system based on heuristic search. It
can deal with general deterministic planning problems encoded in the proposi-
tional fragment of PDDL2.2, including advanced features like ADL conditions
and effects and derived predicates (axioms). Like other well-known planners
such as HSP and FF, Fast Downward is a progression planner, searching the
space of world states of a planning task in the forward direction. However,
unlike other PDDL planning systems, Fast Downward does not use the propo-
sitional PDDL representation of a planning task directly. Instead, the input
is first translated into an alternative representation called multivalued planning
tasks, which makes many of the implicit constraints of a propositional planning
task explicit. Exploiting this alternative representation, Fast Downward uses
hierarchical decompositions of planning tasks for computing its heuristic func-
tion, called the causal graph heuristic, which is very different from traditional
HSP-like heuristics based on ignoring negative interactions of operators." [Malte Helmert]

For more informain you can read Helmert [article](https://arxiv.org/pdf/1109.6051.pdf) or see the Git [main repository](https://github.com/aibasel/downward).  


## Dependencies:
* Ubuntu16 O.S
* python 2.7.18
* pddlsim (more info [here](https://bitbucket.org/galk-opensource/executionsimulation/src/master/))
* Fast-Downward Planner (follow the installation instuction bellow).

## Installation:
1. Clone the repository:  
    ```
    $ git clone https://github.com/amit164/General-Agent.git
    ```
2. Write this command in the terminal:
    ```
    $ cd General-Agent
    ```   

	If you already have the Fast-Downward planner in a directory called `General-Agent/dwonward` you can skip to step 4.
3. Clone the repository:  
    ```
    $ git clone https://github.com/aibasel/downward.git downward
    ```
4. Write those commands in the terminal:
    ```
    $ cd dwonward
    $ ./build.py
    ```
5. Run the agent:
    ```
    $ python my_executive.py -flag <domain_file> <problem_file>
    ```
     > You can use the ``-L`` _flag_ for running the Learning Phase or the ``-E`` _flag_ for running the Execution Phase. 
    
    > _domain_file_ is a path to a domain file (e.g. ``rover_domain.pddl`` which you can find in the main folder).
 
    >  _problem_file_ is a path to a problem file (e.g. ``rover_problem.pddl`` which you can find in the main folder).

