# Paper

- <b>Title:</b> Multi-Agent Reinforcement Learning: Independent vs. Cooperative Agents
- <b>Authors:</b> Ming Tan <i>(GTE Labs, now, Verizon)</i>
- <b>Link:</b> http://web.media.mit.edu/~cynthiab/Readings/tan-MAS-reinfLearn.pdf
- <b>Year:</b> 1993
- <b>Platform:</b> International Conference on Machine Learning (ICML)

# Summary

### What?

* Given the same number of RL agents, will <i>cooperative agents outperform independent agents</i> who do not communicate during learning?
* What is the price for such cooperation?

### Expectations

* Multiple agents together will outperform any single agent due to the fact that they have more resources and better chances of receiving rewards.
* Ideally, intelligent agents would learn when to cooperate and which cooperative method to use to achieve maximum gain.

### How?

> Compares performance of <i>n</i> cooperative agents with the performance of <i>n</i> independent agents.
<br>
Using Independent Agents as a benchmark, cooperative agents can communicate in terms of:

1. Instantaneous information such as sensation (S), actions (A), or rewards (R).
2. Episodes that are sequences of SAR triplets experienced by agents.
3. Learned decision policies.

### RL Formulation

* Each RL agent uses one-step Q-learning algorithm. The agent selects each action <i>a</i> with a probability given by the Boltzmann distribution:

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%207.24.01%20PM.png" height="80" width="360"></img>

* In each time step, the agent updates Q(x, a) by recursively discounting future utilities and weighing them by positive learning rate <i>beta</i>.
  
<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%207.25.51%20PM.png" height="150" width="500"></img>

### Task Description for Case Study

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%207.27.46%20PM.png" height="200" width="300"></img>

- Hunter agents seek to capture randomly moving prey agents in a 10*10 grid world.<br>
- On each time step, agents have four options to choose from i.e. move up, down, right or left.<br>
- More than one agent can occupy the same cell.<br>
- Reward for catching prey is +1. And, reward for a move without catching the prey is -0.1.

Each hunter’s sensation is represented by (x, y) where (x, y) is the relative distance of the closest prey to the hunter according to its x(y) axis.<br><br>
Each run consists of a sequence of trials. Each trial ended when the first prey was captured. Each run was given a sufficient number of trials until the decision policies of hunters converged, i.e. the performance of hunters stabilized.<br><br>
After convergence, the average number of time steps per trial where actions were selected by the highest Q value, over at least 1000 trials. Results averaged over at least 5 runs.

#### Case Study 1 - <i>Sharing Sensation</i>

Sensory information from another (scouting) agent is beneficial if the information is relevant and sufficient for learning.<br>
Construct:<br>
- One prey and one hunter task with a scouting agent that cannot prey.
- Scout makes random moves.
- Sensation from the scout is helpful as there is a limit on the visual field for hunter.
- If the hunter doesn't get a sensation, scout's sensation is leveraged for information.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%207.47.22%20PM.png" width="650" height="300"></img>

<b>Results (CS1)</b> - <br>
- As the scout’s visual field depth increases, the difference in their performance increases.
- However, when the visual field depth was limited to 2, <i><u>sharing sensory information hindered training</i></u> because a short-sighted scout couldn’t stay with the prey long enough for the other hunter to learn to catch up with the prey.
- Sensory information from another agent should be used prudently, and extra, insufficient information can interfere with learning. Scouting also incurs communication cost.

#### Case Study 2 - <i>Sharing Policies or Episodes</i>

Instead of sharing sensation, if the agent is adequate to accomplish the task, does cooperation help? 
Use the same decision policy
Exchange their individual policies at certain frequencies
Such cooperative agents can speed up learning, measured by the average number of steps in training, even though they reach the same asymptotic performance as individual agents.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%208.00.31%20PM.png" width="600" height="500"></img>

- When the two hunters used the same policy, they converged much quicker than two independent agents.
- If agents perform the same task, their decision policies during learning can differ because they may have explored the different parts of a state space. They can complement each other by exchanging policies and use what the other agent had already learned for its own benefit. Assumed that agents average their policies at certain frequencies. (10, 50 and 200 steps)
- Caveat: Information communicated by each policy-exchanging agent per step is bounded by (N-1)*F*P where N = hunters, P = size of policy, F = frequency of policy exchanging. When P and F are large, communication can be costly.
Policy exchanging can selectively choose what decisions of the other agent to consider.

Episode refers to the capture of prey by a hunter. Post that, the entire solution is transfered. Exchanging episodes	can be used by heterogeneous RL agents i.e. with different visual field depths.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%208.18.32%20PM.png" height="350" width="350"></img>

#### Case Study 3 - <i>Joint Tasks</i>

- Prey can only be captured by two hunters who either occupy the same cell as prey or are next to the prey.
- Hunters cooperate by either passively observing each other or actively sharing their sensations and locations. It’s demonstrated that cooperative agents can learn to perform the joint task significantly better than independent agents although they start slowly.<br>
Problem with independent hunters is that they ignore each other. They start slow, but such passively-observing hunters began to overtake the independent hunters after 400 trials, eventually getting reduced to 49 average steps. SImilar is observed for mutually scouting hunters.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/Screenshot%202019-06-19%20at%208.26.46%20PM.png" height="400" width="800"></img>

Even for the 1 prey-task, one may feel that the 2 hunters independently could reach the prey. But, knowing the whereabouts of the partner, hunter can learn better herding techniques.

### Conclusion

