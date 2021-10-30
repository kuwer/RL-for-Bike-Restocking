- ### Reinforcement Learning Model for Rebalancing Bikes stocks

---

### Overview

Bikes accumulate or deplete quickly at certain popular locations. Companies, such as CitiBike, spend lots of effort and money to manage bike stock at each station to ensure availability to riders throughout the day. Re-balancing is currently monitored and orchestrated by human based on conversation with citiBike frontline operators.


### What is Reinforcement Learning?
Reinforcement Learning (RL) program is the "brain" in Google's AlphaGo, Telsa's self-driving car, robots made by Boston Dynamics, and some automatic trading algorithms at Hedge Funds. It is the technique, and some said it is the true AI, that enables autonomous machines. There are various applications beyond the ones we mentioned. Large scale operations with complex constraints and changing conditions, such as Smart City operation and multi-channel digital marketing, are the ideal condidates. There are lots of untapped potentials to gain productivity and new human-manchine interaction in these domains.

### How is bike balancing being done now?
Bikes accumulate or deplete quickly at certain popular locations. Companies, such as CitiBike, spend lots of effort and money to manage bike stock at each station to ensure availability to riders throughout the day. Re-balancing is currently monitored and orchestrated by human based on conversation with citiBike frontline operators.

### How do we envision bike balancing can be done?
We aim to develop a computer agent that is able to understand 1) what the bike stock limit is and 2) decide how many bikes to move to where without hard coding any rules. The only thing we specify is the reward, which the agent will receive if it manages to keep the number of bikes to be less than 50 at the end of each day (23:00). The agent will also receive penalty based on the number of bikes it moves and if the number of bikes exceed 50.

### What will be the impact of our work?
Hopefully the agent can be more precise, timely, and optimized than human agents. If we are successful, doing this may be one step closer to creating an "operational brain" for smart cities. Also, doing this is an attempt to apply Reinforcement Learning in large scale operational and public sector context, which is beyond games and finance.

### How do we measure success (KPIs)?
We measure if the computer agent can achieve the following without deliberate human programming, but only with reward and penalty based on bike stocks:  
1) **High Success Ratio**: % of times it learns to limit bike stock equal or less than 50 by each day 23:00
2) **Low Cost**: Total cost the agent used to balance bike stocks; the agent will receive a penalty of -0.1 * (number of bikes moved)
3) **Short Learning Time**: the agent should be able to develop new rebalancing strategy quickly when external objectives change (e.g. bike stock limit, fluctuating  daily traffic flow, allowed actions)

### What are the key thesis to investigate in near term?
- Can Deep Learning (e.g. RNN) and time series forecasting or simulation techniques improve the performance KPIs (e.g. increase success ratio, lower cost, and shorten learning time)?
- Can the agent balance multiple stations (e.g. 5 stations with inter-connected traffic flow) at the same time?
- Can the agent develop its own action space, which is specified by human programmer at the moment?
- Can parallel learning (Asynchronous Advantaged Actor-Critic)  improve the speed and accuracy of operations in new dynamics?

---
### Preliminary

**Terminology**
- **Agent**: Reinforcement Learning object acting as a "bike re-balancing operator"
    - Policy: agent's behaviour function, which is a map from state to action
    - Value Function: a prediction of future rewards
    - Model: agent's representation of the environment
- **Environment**: a bike station object that will provide feedback such as the number of bikes and reward / penalty
- **State**: the number of bike stock at a given time
- **Training**: interactions between the agent and environment for the agent to learn what the goal is and how to achieve it the best
- **Episode**: number of independent training session (the environment is reset, but agent keeps the learning from one episode to another); each episode has 24 hour inter-dependent instances with bike stock info based on the environment setup and agent actions
- **Session**: each session has multiple episodes with both environment and agent reset; the goal is to benchmark agent performances based on the number of episodes (e.g. will more training episode leads to high success ratio? When should we stop the training?)
- **Q-Table**: a matrix the agent use to decide future action based on state-action-reward tuples; the agent develop this Q-Table from each training episode based on environment feedback

**Setup**

- **Environment**: The following results were generated using a simulation with linearly increasing bike stock. The initial stock at 00:00 is 20 and 3 additional bikes were added hourly. There would be 89 bikes at 23:00 if no bikes were removed during the day.

- **Agent**: The agent can only remove bikes from a station in a quantity of 0, -1, -3, or -10 in each hour. 

- **Reward / Penalty**: 
    - +10 if the bike stock is equal or less than 50 at hour 23:00
    - -10 if the bike stock is more than 50 at any given hour
    - -0.1 * number of bike removed at each hour
    - -20 if bike stock becomes negative at any given hour


