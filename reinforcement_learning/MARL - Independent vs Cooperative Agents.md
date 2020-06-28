# Paper

- <b>Title:</b> Multi-Agent Reinforcement Learning: Independent vs. Cooperative Agents
- <b>Authors:</b> Ming Tan <i>(GTE Labs, now, Verizon)</i>
- <b>Link:</b> http://web.media.mit.edu/~cynthiab/Readings/tan-MAS-reinfLearn.pdf
- <b>Year:</b> 1993
- <b>Platform:</b> International Conference on Machine Learning (ICML)

# Summary

### What?

* Given the same number of RL agents, will <i><b>cooperative agents outperform independent agents</b></i> who do not communicate during learning?
* What is the price for such cooperation?

### Expectation

* Multiple agents will outperform any single agent due to the fact that they have more resources and better chances of receiving rewards.
* Ideally, intelligent agents would learn <i>when to cooperate</i> and <i>which cooperative method</i> to use for maximum gain.

### How?

> Compares performance of <i><b>n</b></i> cooperative agents with that of <i><b>n</b></i> independent agents.

Using independent agents as a benchmark, cooperative agents have been made to communicate via:

CS-1. Instantaneous information such as sensation (S), actions (A), or rewards (R).<br>
CS-2. Episodes that are sequences of SAR triplets experienced by agents.<br>
CS-3. Learned decision policies.

### RL Formulation

* Each RL agent uses one-step <b>Q-learning</b> algorithm. The agent selects each action <i><b>a</b></i> with a probability given by the <i>Boltzmann distribution</i>:

    <img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_1.png" height="80" width="360"></img>

* In each time step, the agent updates Q(x, a) by recursively discounting future utilities and weighing them by positive learning rate <i><b>beta</b></i>.
  
    <img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_2.png" height="150" width="500"></img>

### Task Description for Case Study

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_3.png" height="200" width="300"></img>

- Hunter agents seek to capture randomly moving prey agents in a 10*10 grid world.<br>
- On each time step, agents have four options to choose from i.e. move up, down, right or left.<br>
- More than one agent can occupy the same cell.<br>
- Reward for catching the prey is +1. And, reward for a move without catching the prey is -0.1.

#### Evaluation Strategy
* Each hunter’s sensation is represented by (x, y) where (x, y) is the relative distance of the closest prey to the hunter according to its x(y) axis.<br>
* Each run consists of a sequence of trials. 
    * Each trial ended when the first prey was captured. 
    * Each run was given a sufficient number of trials until the decision policies of hunters converged, i.e. the performance of hunters stabilized.<br>
* After convergence, the average number of time steps per trial where actions were selected by the highest Q-value, over at least 1000 trials. 
    * Results were averaged over at least 5 runs.

#### Case Study 1 - <i>Sharing Sensation</i>

- One prey / one hunter task with a scouting agent that cannot prey, but makes random moves.
- There is a visual field limit to the sensation captured by the hunter about the prey.
- If the hunter doesn't get a sensation owing to range limits, scout's sensation can be leveraged for relevant information.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_4.png" width="600" height="250"></img>

- As the scout’s visual field depth increases, the difference in performance compared to no-scouting increases.
- However, when the visual field depth was limited to 2, <i><u>sharing sensory information hindered training</i></u> because a short-sighted scout couldn’t stay with the prey long enough for the other hunter to learn to catch up with the prey.

#### Case Study 2 - <i>Sharing Policies or Episodes</i>

Agents may use the same decision policy or exchange their individual policies at certain frequencies.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_5.png" width="600" height="500"></img>

- When the two hunters used the same policy, they converged much quicker than two independent agents.
- If agents perform the same task, their decision policies during learning can differ because they may have explored the different parts of a state space. 
    - They can complement each other by exchanging policies and using what the other agent had already learned for its own benefit.
    - It has been assumed that agents average their policies at certain frequencies. (10, 50 and 200 steps)
- <b>Caveat:</b> Information communicated by each policy-exchanging agent per step is bounded by <i>(N-1) * F * P</i> where N: hunters, P: size of policy, F: frequency of policy exchanging. 
    - When P and F are large, communication can be costly.
    - Policy exchanging can selectively choose what decisions of the other agent to consider.

Episode refers to the capture of prey by a hunter.<br>
Post that, the entire solution is transfered. Exchanging episodes can be used by heterogeneous RL agents i.e. with different visual field depths.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_6.png" height="350" width="350"></img>

#### Case Study 3 - <i>Joint Tasks</i>

- Prey can only be captured by two hunters who either occupy the same cell as prey or are next to the prey.
- Hunters cooperate by either <b>passively observing each other</b> or <b>actively sharing their sensations and locations</b>. It’s demonstrated that cooperative agents can learn to perform the joint task significantly better than independent agents although they start slowly.<br>
- Problem with independent hunters is that they ignore each other. They start slow, but such passively-observing hunters began to overtake the independent hunters after 400 trials, eventually getting reduced to 49 average steps. SImilar is observed for mutually scouting hunters.

<img src="https://raw.githubusercontent.com/guptakhil12/research-papers/master/images/marl_independent_cooperative_7.png" height="400" width="800"></img>

Even for the 1 prey-task, one may feel that the 2 hunters independently could reach the prey. But, knowing the whereabouts of the partner, hunter can learn better herding techniques.

### Results

- Additional sensation from other agent is beneficial only if it is relevant and sufficient for learning. Otherwise, the hunter may get lost due to insufficient sensation, and independent agent may end up performing superior.
- Cooperation via learned policies or episodes speeds up learning, but doesn't impact the asymptotic performance. There's always the added cost of communication to be considered.
- Cooperative agents can reach highest level for joint tasks, something never achievable by independent agents. Though they start slow, they improve to exceed the independent agents.

> If cooperation is done intelligently, each agent can benefit from other agents’ instantaneous information, episodic experience, and learned knowledge.

### Conclusion

* RL agents can learn cooperative behavior in a simulated social environment. Can be applied to cooperation among autonomous learning agents.
* Cooperative RL agents can learn faster and converge sooner than independent agents via sharing learned policies or solution episodes. They can also broaden their sensation via mutual scouting, and can handle joint tasks via sensing other partners.
* Extra sensory information can interfere with learning, sharing knowledge or episodes comes with a communication cost, and it takes a larger state space to learn cooperative behavior for joint tasks.

## Open Questions

- Sensation must be selective. Heuristic used is that each hunter pays attention to the nearest prey or hunter. Can such selective sensation be learned?
- Generalization techniques for reduction of state space and improved performance on complex and noisy tasks.
- How can learning be more focused? It would take a long time if the prey was smart enough to know how to escape.
- Cost of communication information can explode with slight increase in state space. Can agents learn to communicate?
- Other cooperative methods?
- Can homogenous agents learn to divide job or specialize differently? Can heterogenous agents (scouting agents vs. blind hunting agents) cooperate?
