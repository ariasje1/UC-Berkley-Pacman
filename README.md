# UC-Berkley-Pacman
Pac-Man AI Search

This project is based on the Pac-Man projects developed at the University of California, Berkeley. The project uses the Pac-Man environment to teach foundational artificial intelligence concepts, particularly state-space search.

The

Depth-First Search (DFS)

Breadth-First Search (BFS)

Uniform Cost Search (UCS)

A* Search

A search problem for visiting all four corners

A heuristic function for the Corners Problem

These algorithms demonstrate fundamental AI techniques that are also used in areas such as robotics, computer vision, and natural language processing.

Project Structure
Files I Modified
File	Purpose
search.py	Contains the general search algorithms
searchAgents.py	Contains search-based Pac-Man agents and search problems
Files Used by the Project
File	Purpose
pacman.py	Main program that runs Pac-Man
game.py	Contains the game logic and supporting classes
util.py	Provides data structures such as Stack, Queue, and PriorityQueue
autograder.py	Runs the automated tests
Supporting Files

The following files are primarily supporting infrastructure and do not normally need to be modified:

graphicsDisplay.py

graphicsUtils.py

textDisplay.py

ghostAgents.py

keyboardAgents.py

layout.py

testParser.py

testClasses.py

searchTestClasses.py

test_cases/

How the Pac-Man Program Works

The main program is pacman.py.

A typical command looks like:

python pacman.py -l tinyMaze -p SearchAgent -a fn=dfs


The three important command-line options are:

-l → selects the layout/environment

-p → selects the Pac-Man agent

-a → provides arguments to the agent, such as which search algorithm to use

For example:

python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs


means:

mediumMaze is the environment

SearchAgent is the Pac-Man agent

bfs tells the SearchAgent to use Breadth-First Search

The same SearchAgent can therefore be used with different search algorithms.

Search Algorithms
Q1 — Depth-First Search

File: search.py

The first task is implementing Depth-First Search (DFS).

DFS uses a Stack, meaning the most recently added state is explored first.

The algorithm:

Get the starting state.

Add it to a stack.

Repeatedly remove a state from the stack.

Check whether it is the goal.

If it is not the goal, expand its successors.

Keep track of visited states so states are not expanded multiple times.

Store the actions needed to reach each state.

Return the action sequence when the goal is found.

Test
python pacman.py -l tinyMaze -p SearchAgent

python pacman.py -l mediumMaze -p SearchAgent

python pacman.py -l bigMaze -z .5 -p SearchAgent

Autograder
python autograder.py -q q1


DFS does not necessarily find the shortest path. It prioritizes going deeper into the search space.

Q2 — Breadth-First Search

File: search.py

Breadth-First Search (BFS) explores states level by level.

Instead of a Stack, BFS uses a Queue.

The algorithm:

Add the starting state to the queue.

Remove the oldest state from the queue.

Check whether it is the goal.

Expand its successors.

Add unvisited successors to the queue.

Continue until the goal is reached.

Test
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs

python pacman.py -l bigMaze -p SearchAgent -a fn=bfs -z .5

Autograder
python autograder.py -q q2


Because each Pac-Man movement has the same cost, BFS finds a fewest-actions path.

Q3 — Uniform Cost Search

File: search.py

Uniform Cost Search (UCS) chooses the state with the lowest total path cost.

UCS uses the PriorityQueue provided in util.py.

Unlike BFS, UCS considers the actual cost of the path rather than simply the number of actions.

Test
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs

python pacman.py -l mediumDottedMaze -p StayEastSearchAgent

python pacman.py -l mediumScaryMaze -p StayWestSearchAgent

Autograder
python autograder.py -q q3


UCS is useful when different actions have different costs.

Q4 — A* Search

File: search.py

A* Search combines:

the cost already traveled

an estimate of the remaining cost

The priority is:

f(n) = g(n) + h(n)


where:

g(n) = cost from the start to the current state

h(n) = heuristic estimate from the current state to the goal

For this project, the Manhattan distance heuristic is provided in searchAgents.py.

Test
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic

Autograder
python autograder.py -q q4


A* can find an optimal solution while expanding fewer states than UCS when given a good heuristic.

Q5 — Corners Problem

File: searchAgents.py

Q5 moves beyond simply finding a path to one fixed location.

The goal is to create a search problem where Pac-Man must visit all four corners of the maze.

The search state must contain enough information to determine:

Pac-Man's current position

Which corners have already been visited

The entire GameState should not be used as the search state because it contains unnecessary information.

Test
python pacman.py -l tinyCorners -p SearchAgent -a fn=bfs,prob=CornersProblem

python pacman.py -l mediumCorners -p SearchAgent -a fn=bfs,prob=CornersProblem

Autograder
python autograder.py -q q5


The shortest solution for tinyCorners should take 28 steps.

Q6 — Corners Heuristic

File: searchAgents.py

Q6 requires creating a heuristic for the Corners Problem.

The heuristic should estimate the remaining cost required to visit all unvisited corners.

A valid heuristic must be:

Non-negative

Admissible

0 when the goal has been reached

Useful enough to reduce the number of states expanded

Test
python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5


AStarCornersAgent is shorthand for:

python pacman.py -l mediumCorners -p SearchAgent -a fn=aStarSearch,prob=CornersProblem,heuristic=cornersHeuristic

Autograder
python autograder.py -q q6

Heuristic Grading
Nodes Expanded	Score
More than 2000	0/3
2000 or fewer	1/3
1600 or fewer	2/3
1200 or fewer	3/3
Important Concepts
SearchProblem

SearchProblem defines the interface that search algorithms use.

A search problem provides four important methods:

getStartState()
isGoalState(state)
getSuccessors(state)
getCostOfActions(actions)


The search algorithms in search.py are written generically so they can work with different search problems.

Search States

A search state represents the information needed to solve the current problem.

For the basic maze problem, the state can simply be Pac-Man's position.

For the Corners Problem, the state needs to contain both:

Pac-Man's position
+
Corners already visited

Search Nodes

A search node generally needs to keep track of information such as:

current state
actions taken to reach the state
path cost


The exact information needed depends on the search algorithm.

Testing

Individual questions can be tested using the autograder.

python autograder.py -q q1
python autograder.py -q q2
python autograder.py -q q3
python autograder.py -q q4
python autograder.py -q q5
python autograder.py -q q6


A specific test can also be run:

python autograder.py -t test_cases/q1/graph_backtrack

Assignment Requirements

The final submission consists of:

search.py
searchAgents.py


These files should contain the completed implementations for the required questions.

Do not change the names of the provided functions or classes because the autograder depends on those names.

Grading
Question	Topic	Points
Q1	Depth-First Search	3
Q2	Breadth-First Search	3
Q3	Uniform Cost Search	3
Q4	A* Search	3
Q5	Find All Corners	3
Q6	Corners Heuristic	3
Total		18
Useful Commands
Run Pac-Man
python pacman.py

View command options
python pacman.py -h

Run DFS
python pacman.py -l mediumMaze -p SearchAgent -a fn=dfs

Run BFS
python pacman.py -l mediumMaze -p SearchAgent -a fn=bfs

Run UCS
python pacman.py -l mediumMaze -p SearchAgent -a fn=ucs

Run A*
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=astar,heuristic=manhattanHeuristic

Run an autograder question
python autograder.py -q q1

Progress

 Q1 — Depth-First Search

 Q2 — Breadth-First Search

 Q3 — Uniform Cost Search

 Q4 — A* Search

 Q5 — Corners Problem

 Q6 — Corners Heuristic

Key Takeaway

The first four questions focus on implementing general-purpose search algorithms in search.py.

The last two questions introduce a new search problem and heuristic in searchAgents.py.

The overall progression is:

DFS
 ↓
BFS
 ↓
UCS
 ↓
A*
 ↓
Corners Problem
 ↓
Corners Heuristic
