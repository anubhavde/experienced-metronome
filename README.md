#### **experienced-metronome**

# **Implementing Deep Deterministic Policy Gradient algorithm to Inverted Pendulum Problem**

### **What is Deep Deterministic Policy Gradient model?**

Deep Deterministic Policy Gradient [(DDPG)](https://keras.io/examples/rl/ddpg_pendulum/) is a model-free off-policy algorithm for learning continuous actions. It combines the Experience Replay & slow-learning target networks from Deep Q-networks with Deterministic Policy Gradient to operate over continuous action spaces. 

To solve the classic Inverted Pendulum problem, we can take only 2 actions: swing left or swing right. Since it selects from an infinite actions from -2 to +2 (instead of 2 discrete actions liek -1 & +1); it can be considered as Continuous making it a difficult chanllenge for Q-learning Algorithms.

Apart from the 2 networks, i.e Actor(propose an action in a state) and Critic(predict if action is good), DDPG uses 2 more techniques not present in the original DQN. It uses 2 Target Networks because it adds stability to training & uses Experience Relay i.e instead of learning only from recent experiences, it learns from sampling all experiences by storing list of tuples ```(state, action, reward, next_state)``` .

To implement better exploration by the Actor network, Ornstein-Uhlenbeck process is used for generating noise.

**Critic loss** - Mean Squared Error of y - Q(s, a) where y is the expected return as seen by the Target network, and Q(s, a) is action value predicted by the Critic network. y is a moving target that the critic model tries to achieve; we make this target stable by updating the Target model slowly.

**Actor loss** - This is computed using the mean of the value given by the Critic network for the actions taken by the Actor network. We seek to maximize this quantity. 

Hence, for a given state; Actor network is updated so that it produces actions that get the maximum predicted value as seen by the Critic. The Actor & Critic networks are basic Dense Models with **ReLU** activation. The initialization for last layer of the Actor must be between -0.003 and 0.003 as this prevents us from getting 1 or -1 output values in the initial stages, which would squash our gradients to zero, as we use the tanh activation.

A ```policy()``` is used to sample actions and train with ```learn()``` is used to train at each time step, along with updating the Target networks at a rate ```tau```.

**NOTE:** If training proceeds correctly, the average episodic reward will increase with time.

**Before vs After Training:**

![](https://github.com/anubhavde/experienced-metronome/blob/main/before.gif) ![](https://github.com/anubhavde/experienced-metronome/blob/main/after.gif)
