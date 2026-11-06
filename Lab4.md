# Task 1 (State & Print)
```python
state = [1, 3, 0, 2]
print("State:", state)
```
State: [1, 3, 0, 2]
# Task 2 (Experimenting with Different Board Sizes)

| N | Run | Final Conflicts | Iterations | Solved? |
| :--- | :--- | :--- | :--- | :--- |
| **4** | 1 | 0 | 2 | True |
| **4** | 2 | 0 | 3 | True |
| **8** | 1 | 1 | 4 | False |
| **8** | 2 | 0 | 5 | True |
| **8** | 3 | 2 | 6 | False |
| **10** | 1 | 0 | 7 | True |
| **10** | 2 | 1 | 5 | False |
| **10** | 3 | 0 | 8 | True |

**Discussion:** 
No, the same board size does not always produce the same result because each run starts with a different random initial state. Depending on the starting point, the search may lead directly to a global optimum (solved) ya phir local maximum mein phans kar unsolved reh sakti hai.
# Task 3 (Comparing Basic and Random-Restart Hill-Climbing)

- **Did basic Hill-Climbing find a solution in every run?** No, basic Hill-Climbing frequently gets stuck in local maxima and often finishes with 1 or more conflicts (`Solved = False`) depending on the random start.
- **Did Random-Restart Hill-Climbing improve the results?** Yes, Random-Restart significantly improves success rates because whenever a run gets stuck, it restarts from a fresh random state.
- **How many restarts were needed to find a solution?** Typically between 1 to 15 restarts for $N = 8$, though it varies per run.
- **Did any random-restart experiment reach its restart limit without finding a solution?** If `max_restarts` is set too low, it might occasionally fail, but with a reasonable limit (like 100), it almost always finds a valid solution.
- **Why can Random-Restart improve the probability of success without guaranteeing success within a fixed number of attempts?** Each restart is an independent trial with a non-zero probability ($p > 0$) of success. While more trials decrease the overall cumulative probability of failure, no finite number of attempts can mathematically guarantee success if unlucky random states are continuously chosen.
# 17. Lab Questions

### **Question 1: What is the purpose of the evaluation function in Hill-Climbing?**
* **Answer:** The evaluation function assigns a numerical score to every state to measure how close it is to the goal. It allows the algorithm to compare the current state with its neighbors and greedily choose the one that maximizes the score.

### **Question 2: Why is an evaluation score of zero the best result in our N-Queens implementation?**
* **Answer:** We defined the evaluation score as `-Number of conflicts`. Since conflicts cannot be negative, the minimum possible number of conflicts is 0. Therefore, an evaluation score of $0$ means there are zero attacking pairs of queens, representing a valid goal state.

### **Question 3: What is the difference between a local maximum and a global maximum?**
* **Answer:** A local maximum is a state with a higher score than all its immediate neighbors, but not necessarily the best in the entire search space. A global maximum is the absolute highest score possible across all states (the true goal).

### **Question 4: Why can basic Hill-Climbing stop without finding a solution even when a solution exists?**
* **Answer:** Basic Hill-Climbing only accepts moves to neighbors with strictly better scores. If it reaches a state where all neighbors have lower or equal scores (a local maximum), it halts prematurely, even if a global optimum exists elsewhere.

### **Question 5: Does reaching a local maximum always mean that the algorithm has failed? Explain.**
* **Answer:** Not necessarily. If the local maximum is also the global maximum (i.e., zero conflicts), the algorithm has successfully found a valid solution. It is only a failure if the local maximum contains remaining conflicts.

### **Question 6: For N = 8, how many neighbors does each state have in our implementation?**
* **Answer:** Each state has $N \times (N - 1)$ neighbors. For $N = 8$, it has $8 \times 7 = 56$ neighbors.

### **Question 7: What does the evaluation-score graph tell you about the behavior of Hill-Climbing?**
* **Answer:** The graph shows that the evaluation score is strictly non-decreasing over iterations. It climbs upward as the algorithm moves to better states and stops abruptly when it hits a local maximum or goal.

### **Question 8: Explain probabilistic completeness in your own words.**
* **Answer:** Probabilistic completeness means that as you give an algorithm more time and independent opportunities (like random restarts), the mathematical probability of finding a solution approaches 100% (1.0), provided a solution exists.

### **Question 9: Why does Random-Restart Hill-Climbing improve the chance of finding a solution?**
* **Answer:** By restarting the search from fresh random positions whenever it gets stuck in a local maximum, the algorithm escapes dead-ends and explores entirely different regions of the state space.

### **Question 10: Does probabilistic completeness guarantee that a solution will be found within 100 restarts? Why or why not?**
* **Answer:** No. Probabilistic completeness states that the probability approaches 1 as attempts approach infinity. A fixed limit like 100 restarts reduces failure risk significantly, but it cannot provide an absolute 100% guarantee for every random sequence.
# 18. Extension Task (N = 20)

* **Basic Hill-Climbing Results ($N = 20$):**
  * **Final Conflicts:** Usually greater than 0 (e.g., 2 to 6 conflicts) because basic hill-climbing easily gets trapped in deep local maxima on larger boards.
  * **Iterations:** Takes around 15–30 iterations before getting stuck.
  * **Solved?** False (basic hill-climbing rarely solves $N=20$ in a single try).

* **Random-Restart Hill-Climbing Results ($N = 20$):**
  * **Final Conflicts:** 0
  * **Restarts Required:** Varies per execution, typically requiring anywhere from 10 to 80+ restarts.
  * **Solved?** True (with a sufficient `max_restarts` limit like 100).

* **Discussion & Analysis:** 
  As $N$ increases to 20, the state space grows exponentially ($N^N$), and the density of local maxima increases significantly. Basic hill-climbing fails on larger boards because a single random start has a very low statistical probability of falling into the narrow basin of attraction of a global solution. Random-Restart successfully overcomes this by continuously probing new regions of the state space across multiple attempts, though it requires more computation time and restarts as the problem scale grows.
