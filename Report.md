# Banana Agent

# Project's Goal

In this project, the goal is to train an agent to navigate a virtual world and collect as many yellow bananas as possible while avoiding blue bananas.

![alt text](https://github.com/saj122/BananaAgent/blob/master/images/banana.gif)

# Environment Details

The environment is based on Unity ML-agents

Note: The project environment provided by Udacity is similar to, but not identical to the Banana Collector environment on the Unity ML-Agents GitHub page.

    The Unity Machine Learning Agents Toolkit (ML-Agents) is an open-source Unity plugin that enables games and 
    simulations to serve as environments for training intelligent agents. Agents can be trained using reinforcement 
    learning, imitation learning, neuroevolution, or other machine learning methods through a simple-to-use Python API.

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana. Thus, the goal of your agent is to collect as many yellow bananas as possible while avoiding blue bananas.

The state space has 37 dimensions and contains the agent's velocity, along with ray-based perception of objects around the agent's forward direction. Given this information, the agent has to learn how to best select actions. Four discrete actions are available, corresponding to:

0 - move forward.

1 - move backward.

2 - turn left.

3 - turn right.

The task is episodic, and in order to solve the environment, your agent must get an average score of +13 over 100 consecutive episodes.

# Algorithm
Deep Q-Learning was used to solve the enviroment.

Deep Q-Learning:
   * Initialize replay memory D with capacity N
   
   * Initialize action-value function q with random weights w
   
   * Initialize target action-value weights w- <- w
   
   * for the episode e <- 1 to M:
   
      * Initial input frame x_t
      
      * Prepare initial state: S <- phi(<x_t>)
      
      * for time step t <- 1 to T:
         #### Sample:
         Choose action A from state S using policy pi <- e-Greedy(q(S,A,w))
         
         Take action A, observe reward R, and next input frame x_t+1
         
         Prepare next state: S' <- phi(<x_t-2,x_t-1,x_t,x_t+1>)
         
         Store experience tuple (S,A,R,S') in replay memory D
         
         S <- S'
         
         #### Learn:
         Obtain random minibatch of tuples(s_j,a_j,r_j,s_j+1) from D
         
         Set target y_j = r_j + gamma * max_a q(s_j+1,a,w-)
         
         Update: delta_w = alpha * (y_j - q(s_j,a_j,w)) * q_gradient(s_j,a_j,w)
         
         Every C steps, reset: w- <- w
