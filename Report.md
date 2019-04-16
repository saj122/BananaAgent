# Banana Agent

# Project's Goal

In this project, the goal is to train an agent to navigate a virtual world and collect as many yellow bananas as possible while avoiding blue bananas.

![alt text](https://github.com/saj122/BananaAgent/blob/master/images/banana.gif)

# Environment Details

The environment is based on Unity ML-agents

Note: The project environment provided by Udacity is similar to, but not identical to the Banana Collector environment on the Unity ML-Agents GitHub page.

> The Unity Machine Learning Agents Toolkit (ML-Agents) is an open-source Unity plugin that enables games and 
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

```
Deep Q-Learning:
   * Initialize replay memory D with capacity N
   
   * Initialize action-value function q with random weights w
   
   * Initialize target action-value weights w- <- w
   
   * for the episode e <- 1 to M:
   
      * Initial input frame x_t
      
      * Prepare initial state: S <- phi(<x_t>)
      
      * for time step t <- 1 to T:
         Sample:
            Choose action A from state S using policy pi <- e-Greedy(q(S,A,w))
         
            Take action A, observe reward R, and next input frame x_t+1
         
            Prepare next state: S' <- phi(<x_t-2,x_t-1,x_t,x_t+1>)
         
            Store experience tuple (S,A,R,S') in replay memory D
         
            S <- S'
         
         Learn:
            Obtain random minibatch of tuples(s_j,a_j,r_j,s_j+1) from D
         
            Set target y_j = r_j + gamma * max_a q(s_j+1,a,w-)
         
            Update: delta_w = alpha * (y_j - q(s_j,a_j,w)) * q_gradient(s_j,a_j,w)
         
            Every C steps, reset: w- <- w
```
# DQN Hyperparameters
   
   * BUFFER_SIZE = int(1e5)
   * BATCH_SIZE = 64
   * GAMMA = 0.99
   * TAU = 0.003
   * LR = 0.0005
   * UPDATE_EVERY = 4
   
# Code Implementation

The code was derived from the Lunar Lander code from the Deep Reinforcment Learning Nanodegree.

The code is all in one file Navigation.ipynb.

It includes the QNetwork model. This network will be trained to predict the action to perform depending on the environment observed states. The Neural Network model used:

   Input nodes (37) -> Fully Connected Layer (32 nodes, Relu activation) -> Fully Connected Layer (32 nodes, Relu activation) -> Fully Connected Layer (32 nodes, Relu activation) -> Ouput nodes (4)
   
A DQN Agent class is implemented as follows: 

   * The constructor initializes the memory buffer and 2 instances of the neural network. 
   * step(): The step method stores a step taken by the agent (state, action, reward, next_state, done) and places it in the replay buffer. Every 4 steps the target network is updated with the current weights from the local network.
   * act(): returns actions for the input state from the current policy.
   * learn(): Given experiences from the replay buffer the parameters of the network are updated.
   * soft_update(): updates the target weight values from the local weight values.
   
The Replay Buffer has a fixed size and stores experience tuples. There are two methods to add experience and to randomly sample a batch for learning.

The rest of the code includes the main block for training the agent and then playing back episodes with the latest checkpoint.
   
# Results

Given the hyperparameters and neural network the agent was able to achieve an average reward of 13 in 383 episodes.

![alt text](https://github.com/saj122/BananaAgent/blob/master/images/navigation.png)

# Future Work

The original DQN research paper utilized a CNN to learn from the pixels instead of ray vectors. 

Using a CNN, you could use a Double DQN which tends to reduce the observed overestimations of the DQN.

Since reality and games are not static and depend on time, utilizing an LSTM could be better at estimating motion in games.
