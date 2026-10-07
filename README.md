# Pac-Man AI — Search Algorithms

## Introduction

The Pac-Man projects were developed at the **University of California, Berkeley**. They apply a variety of Artificial Intelligence techniques to playing Pac-Man.

However, these projects are not primarily focused on building AI for video games. Instead, they introduce foundational AI concepts such as:

- Informed state-space search
- Probabilistic inference
- Reinforcement learning
- Navigation and path planning
- Heuristic search

These concepts form the foundation of real-world AI applications such as:

- Natural Language Processing (NLP)
- Computer Vision
- Robotics
- Autonomous navigation
- Decision-making systems

The projects were designed with three primary goals:

1. **Visualization** — The results of implemented AI techniques can be visualized directly in the Pac-Man environment.
2. **Learning** — The projects provide code examples and clear instructions without requiring students to navigate excessive scaffolding.
3. **Challenge** — Pac-Man provides a challenging problem environment that requires creative solutions, similar to real-world AI problems.

---

# Assignment Overview

In this assignment, we implement several fundamental search algorithms and apply them to navigation and pathfinding problems in the Pac-Man world.

The algorithms implemented are:

1. Depth-First Search (DFS)
2. Breadth-First Search (BFS)
3. Uniform-Cost Search (UCS)
4. A* Search
5. Corners Problem Search
6. A* Heuristic for the Corners Problem

The Pac-Man agent will be able to:

- Find paths through maze environments
- Reach specific destinations
- Find optimal paths based on different cost functions
- Visit all four corners of a maze
- Use heuristics to improve search efficiency

---

# Project Structure

The project consists of several Python files.

## Files to Edit

| File | Description |
|---|---|
| `search.py` | Contains the implementations of DFS, BFS, UCS, and A* search algorithms. |
| `searchAgents.py` | Contains search-based Pac-Man agents, the Corners Problem, and the corners heuristic. |

## Files to Understand

| File | Description |
|---|---|
| `pacman.py` | Main file used to run Pac-Man games and defines the `GameState`. |
| `game.py` | Contains supporting classes such as `AgentState`, `Agent`, `Direction`, and `Grid`. |
| `util.py` | Provides useful data structures such as `Stack`, `Queue`, and `PriorityQueue`. |

## Supporting Files

The following files generally do not need to be modified:

| File | Description |
|---|---|
| `graphicsDisplay.py` | Graphics for Pac-Man. |
| `graphicsUtils.py` | Graphics utility functions. |
| `textDisplay.py` | ASCII-based Pac-Man display. |
| `ghostAgents.py` | Ghost agent implementations. |
| `keyboardAgents.py` | Keyboard-controlled agents. |
| `layout.py` | Reads and stores maze layouts. |
| `autograder.py` | Assignment autograder. |
| `testParser.py` | Parses test and solution files. |
| `testClasses.py` | General autograding classes. |
| `searchTestClasses.py` | Search-project-specific autograding classes. |
| `test_cases/` | Test cases for each assignment question. |

---

# Getting Started

After downloading and extracting the project, navigate to the project directory.

Start a standard Pac-Man game with:

python pacman.py


---

# Welcome to Pac-Man

Pac-Man lives in a maze consisting of twisting corridors, walls, ghosts, and food pellets.

The first goal is to teach Pac-Man how to navigate the maze efficiently.

## GoWestAgent

The simplest agent included in `searchAgents.py` is the `GoWestAgent`.

This agent always attempts to move West.

Run it with:

python pacman.py --layout testMaze --pacman GoWestAgent


This simple strategy can occasionally solve a maze.

However, it fails when the maze requires Pac-Man to change direction:

python pacman.py --layout tinyMaze --pacman GoWestAgent


If Pac-Man becomes stuck, terminate the game using:

CTRL-C


The objective of this assignment is to develop search algorithms that allow Pac-Man to solve increasingly complex mazes.

---

# Pac-Man Command-Line Options

Pac-Man supports many command-line options.

To view all available options:

python pacman.py -h


Options can generally be specified using either their long or short form.

For example:

--layout


or:

-l


---

# Python Type Annotations

The project may use Python type annotations such as:

def my_function( a: int, b: Tuple[int, int], c: List[List], d: Any, e: float = 1.0 ):


These annotations indicate the expected types of arguments.

For example:

- `a: int` → `a` should be an integer.
- `b: Tuple[int, int]` → `b` should be a tuple containing two integers.
- `c: List[List]` → `c` should be a two-dimensional list.
- `d: Any` → `d` can contain any type.
- `e: float = 1.0` → `e` should be a floating-point number and defaults to `1.0`.

Python generally does not enforce these annotations at runtime. They are primarily used to make code easier to understand and maintain.

---

# Search Algorithms

All search algorithms implemented in this project must return a list of actions that moves Pac-Man from the initial state to a goal state.

Every action must be a legal Pac-Man movement.

The algorithms implemented are:

- Depth-First Search
- Breadth-First Search
- Uniform-Cost Search
- A* Search

A key observation is that these algorithms have very similar structures. The primary difference is how the **fringe/frontier** is managed.

---

# Q1 — Depth-First Search

**Points: 3**

## Objective

Implement the **Depth-First Search (DFS)** algorithm in:

search.py


Specifically, implement:

depthFirstSearch


The implementation should use **graph search**, meaning that already-expanded states should not be expanded again.

## Testing the SearchAgent

Before implementing DFS, verify that the provided `SearchAgent` works using the built-in `tinyMazeSearch` algorithm:

python pacman.py -l tinyMaze -p SearchAgent -a fn=tinyMazeSearch


Pac-Man should successfully navigate the maze.

## DFS Requirements

The DFS implementation should:

- Use the provided `Stack` data structure from `util.py`.
- Keep track of visited states.
- Avoid expanding states that have already been visited.
- Return a list of legal actions.
- Store enough information to reconstruct the path from the start state to the goal.

The important idea behind DFS is that it explores one branch as deeply as possible before backtracking.

## Test DFS

Run:

python pacman.py -l tinyMaze -p SearchAgent


Then test larger mazes:

python pacman.py -l mediumMaze -p SearchAgent


python pacman.py -l bigMaze -z .5 -p SearchAgent


## DFS Behavior

DFS does **not necessarily find the shortest path**.

For example, on `mediumMaze`, a correct DFS implementation may find a path of approximately:

130 actions


when successors are pushed in the order returned by `getSuccessors()`.

If successors are pushed in reverse order, the resulting path may be significantly longer.

This demonstrates an important property of DFS:

> DFS prioritizes depth over path optimality.

Therefore, DFS can find a solution without finding the least-cost solution.

## DFS Autograder

Run:

python autograder.py -q q1


You can also run an individual test:

python autograder.py -t testcases/q1/graphbacktrack


---

# Q2 — Breadth-First Search

**Points: 3**

## Objective

Implement **Breadth-First Search (BFS)** in:

search.py


Specifically:

breadthFirstSearch


Like DFS, BFS should be implemented as graph search and should avoid expanding states that have already been visited.

## BFS Requirements

The BFS implementation should:

- Use the provided `Queue` data structure from `util.py`.
- Explore states level by level.
- Keep track of visited states.
- Avoid unnecessary calls to `getSuccessors()`.
- Return a valid list of actions.

Because every Pac-Man movement has the same cost, BFS finds a path containing the minimum number of actions.

## Test BFS

Run:

python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs


For a larger maze:

python pacman.py -l bigMaze -p SearchAgent -a fn=bfs -z .5


If Pac-Man moves too slowly, reduce the frame time:

python pacman.py --frameTime 0 -l bigMaze -p SearchAgent -a fn=bfs -z .5


## BFS and Optimality

Unlike DFS, BFS explores states in order of their distance from the starting state.

When all actions have equal cost:

> BFS finds a least-action solution.

BFS is therefore appropriate when the objective is to minimize the number of movements.

## Eight Puzzle

If the search implementation is sufficiently generic, it can also be used with the Eight Puzzle without modifying the search algorithm.

Run:

python eightpuzzle.py


## BFS Autograder

Run:

python autograder.py -q q2


---

# Q3 — Uniform-Cost Search

**Points: 3**

## Objective

Implement **Uniform-Cost Search (UCS)** in:

search.py


Specifically:

uniformCostSearch


## Motivation

BFS finds a path with the fewest actions, but the path with the fewest actions is not always the cheapest path.

Different actions can have different costs.

For example, Pac-Man might prefer:

- Safer paths
- Paths containing more food
- Paths that avoid ghosts
- Paths with lower movement costs

UCS accounts for these varying costs.

## UCS Requirements

The implementation should:

- Use the provided `PriorityQueue`.
- Prioritize states according to their accumulated path cost.
- Track visited states appropriately.
- Return the lowest-cost path to the goal.

The priority of a node is based on:

g(n)


where `g(n)` is the cost accumulated from the start state to the current state.

## Test UCS

Run:

python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs


### Stay East Agent

python pacman.py -l mediumDottedMaze -p StayEastSearchAgent


### Stay West Agent

python pacman.py -l mediumScaryMaze -p StayWestSearchAgent


The `StayEastSearchAgent` and `StayWestSearchAgent` use different cost functions.

Because their cost functions are exponential, their resulting path costs can be extremely different.

## UCS Autograder

Run:

python autograder.py -q q3


---

# Q4 — A* Search

**Points: 3**

## Objective

Implement **A\* Search** in:

search.py


Specifically:

aStarSearch


A* combines the actual cost of reaching a state with an estimated cost of reaching the goal.

The evaluation function is:

f(n) = g(n) + h(n)


where:

- `g(n)` = cost from the start state to node `n`
- `h(n)` = estimated cost from node `n` to the goal
- `f(n)` = estimated total cost of the solution through node `n`

## Heuristics

A heuristic is a function that estimates the remaining cost to reach the goal.

It has the following form:

heuristic(state, problem)


The project provides a trivial heuristic:

nullHeuristic


which always returns zero.

A zero heuristic makes A* behave similarly to Uniform-Cost Search.

## Manhattan Distance

A Manhattan-distance heuristic is already implemented in `searchAgents.py`.

It can be used to test A*:

python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic


A* should find an optimal path while generally expanding fewer nodes than UCS.

For example, an implementation may expand approximately:

A*: 549 nodes UCS: 620 nodes


Exact numbers can vary because of tie-breaking behavior.

## A* Requirements

A* should:

- Use a `PriorityQueue`.
- Consider both path cost and heuristic value.
- Track the best known cost to each state.
- Allow a state to be considered again if a cheaper path is discovered.
- Return an optimal solution when the heuristic is admissible.

A useful data structure is:

best_g[state]


which stores the lowest known cost from the start state to that state.

## A* Autograder

Run:

python autograder.py -q q4


---

# Q5 — Finding All Four Corners

**Points: 3**

## Objective

The previous search problems involved finding a path to a single goal.

The **Corners Problem** introduces a more challenging objective:

> Find the shortest path that visits all four corners of the maze.

Implement:

CornersProblem


in:

searchAgents.py


## Corner Locations

The maze contains four important corner locations.

The search state must contain enough information to determine:

1. Pac-Man's current position.
2. Which corners have already been visited.

The state should **not** contain irrelevant information such as:

- Ghost positions
- Food locations
- The complete `GameState`
- Other information unrelated to the corners problem

## State Representation

A suitable state representation could contain:

(currentposition, visitedcorners)


For example:

((x, y), visited_corners)


The exact representation is up to the implementation as long as it contains all information necessary to solve the problem.

## Important Requirement

Do **not** use the entire Pac-Man `GameState` as the search state.

Instead, create an abstract state containing only the information required for the search.

Using the complete `GameState` would make the search extremely slow and would not satisfy the assignment requirements.

## Successor Function

The `getSuccessors` function should generate legal neighboring positions.

Each successor should have the form:

(successor_state, action, cost)


For this problem, every movement should have a cost of:

1


## Test Corners Problem

### Tiny Corners

python pacman.py -l tinyCorners -p SearchAgent -a fn=bfs,prob=CornersProblem


The optimal solution for `tinyCorners` requires:

28 steps


### Medium Corners

python pacman.py -l mediumCorners -p SearchAgent -a fn=bfs,prob=CornersProblem


A correct BFS implementation should expand roughly 2,000 nodes on `mediumCorners`.

## Corners Autograder

Run:

python autograder.py -q q5


---

# Q6 — Corners Problem Heuristic

**Points: 3**

## Objective

The final task is to create a heuristic for the Corners Problem.

Implement:

cornersHeuristic


in:

searchAgents.py


The heuristic will be used with A* search.

## Test A* on the Corners Problem

Run:

python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5


`AStarCornersAgent` is a shortcut for:

-p SearchAgent -a fn=aStarSearch,prob=CornersProblem,heuristic=cornersHeuristic


---

# Heuristic Requirements

A good heuristic should provide a useful estimate of the remaining cost while never overestimating the actual cost.

A heuristic is **admissible** if:

h(n) <= actual remaining cost


for every state `n`.

The heuristic must also satisfy:

h(n) >= 0


Therefore:

- The heuristic must never be negative.
- The heuristic must return `0` for goal states.
- The heuristic must not overestimate the true remaining cost.

---

# Heuristic Performance

The autograder evaluates the heuristic based on the number of nodes expanded.

Nodes Expanded	Score
More than 2000	0/3
At most 2000	1/3
At most 1600	2/3
At most 1200	3/3
The goal is to create a heuristic that is both:

Admissible
Informative
A heuristic that always returns zero is technically admissible, but it behaves like UCS and does not provide meaningful performance improvements.

A heuristic that computes the exact remaining cost would be very expensive and is not acceptable for the assignment.

Corners Heuristic Autograder
Run:

python autograder.py -q q6
Autograder Summary
Run each question independently using the following commands:

Question	Topic	Command
Q1	Depth-First Search	python autograder.py -q q1
Q2	Breadth-First Search	python autograder.py -q q2
Q3	Uniform-Cost Search	python autograder.py -q q3
Q4	A* Search	python autograder.py -q q4
Q5	Corners Problem	python autograder.py -q q5
Q6	Corners Heuristic	python autograder.py -q q6
Important Implementation Notes
Use the Provided Data Structures
The project provides specialized data structures in:

util.py
Use these implementations rather than replacing them with your own.

Algorithm	Data Structure
DFS	Stack
BFS	Queue
UCS	PriorityQueue
A*	PriorityQueue
Avoid Unnecessary Successor Expansion
The autograder checks the number of nodes expanded.

Therefore, unnecessary calls to:

getSuccessors()
can cause tests to fail.

Only expand a state when necessary.

Graph Search
DFS and BFS must be implemented as graph-search algorithms.

This means maintaining information about states that have already been visited or expanded.

Without duplicate-state detection, the search can:

Expand the same state repeatedly.
Become significantly slower.
Potentially fail to terminate efficiently.
Search Algorithm Comparison
Algorithm	Data Structure	Uses Cost?	Uses Heuristic?	Optimal?
DFS	Stack	No	No	No
BFS	Queue	No	No	Yes, when all step costs are equal
UCS	Priority Queue	Yes	No	Yes
A*	Priority Queue	Yes	Yes	Yes, with an appropriate admissible heuristic
What to Turn In
Only the following two files need to be submitted:

search.py
searchAgents.py
Upload the files separately.

Do not submit them as a ZIP file.

File Submission
The final project should contain your implementations in:

search.py
searchAgents.py
If Canvas changes a filename by adding a suffix such as:

search-1.py
this is not a problem according to the assignment instructions.

Grading
The assignment is worth 15% of the final grade according to the project instructions.

The provided rubric awards points for the six major components:

Criterion	Points
Depth-First Search	3
Breadth-First Search	3
Uniform-Cost Search	3
A* Search	3
Find All Four Corners	3
Corners Problem Heuristic	3
Total	18
The code is primarily evaluated through automated testing for technical correctness. However, the correctness of the implementation—not merely the autograder's judgment—is ultimately considered the final standard.

Learning Objectives
By completing this project, you will gain practical experience with:

State-space search
Graph search
Depth-First Search
Breadth-First Search
Uniform-Cost Search
A* Search
Priority queues
Path reconstruction
Cost functions
Heuristic functions
Admissibility
Search-state representation
Navigation and path planning
Algorithmic efficiency
These concepts provide a foundation for more advanced AI topics and real-world applications in areas such as robotics, autonomous systems, computer vision, and natural language processing.

Useful Commands
Start Pac-Man
python pacman.py
View Help
python pacman.py -h
Test Tiny Maze
python pacman.py -l tinyMaze -p SearchAgent
Test DFS
python pacman.py -l mediumMaze -p SearchAgent
Test BFS
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs
Test UCS
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs
Test A*
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic
Test Corners with BFS
python pacman.py -l tinyCorners -p SearchAgent -a fn=bfs,prob=CornersProblem
Test Corners with A*
python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5
Run Individual Autograders
python autograder.py -q q1
python autograder.py -q q2
python autograder.py -q q3
python autograder.py -q q4
python autograder.py -q q5
python autograder.py -q q6
Conclusion
This project demonstrates how classical AI search algorithms can be applied to a complex navigation problem.

Starting with simple uninformed algorithms such as DFS and BFS, the project progresses to cost-aware search with UCS and heuristic search with A*. The Corners Problem then introduces the challenge of designing compact state representations and effective admissible heuristics.

The Pac-Man environment provides an interactive way to visualize these algorithms and understand the trade-offs between:

Search depth
Path length
Path cost
Memory usage
Number of expanded states
Heuristic quality
Solution optimality
Together, these techniques form an important foundation for understanding modern Artificial Intelligence and intelligent decision-making systems.
