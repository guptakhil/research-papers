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

Each hunter’s sensation is represented by (x, y) where (x, y) is the relative distance of the closest prey to the hunter according to its x(y) axis.<br>
Each run consists of a sequence of trials. Each trial ended when the first prey was captured. Each run was given a sufficient number of trials until the decision policies of hunters converged, i.e. the performance of hunters stabilized.
After convergence, the average number of time steps per trial where actions were selected by the highest Q value, over at least 1000 trials. Results averaged over at least 5 runs.
