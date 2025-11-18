#  Dynamic Portfolio Rebalancing using Deep Q-Learning

A reinforcement learning–based portfolio management system that dynamically reallocates capital across **Stocks**, **Bonds**, and **Cash** using a **Deep Q-Network (DQN)**.  
The agent learns optimal rebalancing strategies across **high**, **medium**, and **low volatility** markets to maximize risk-adjusted returns and control downside risk.

---

##  Introduction

Traditional rebalancing strategies are **static** and fail to adjust quickly during volatile markets.  
This often results in:
- Lower Sharpe Ratios  
- Higher drawdowns  
- Inefficient capital allocation  

To overcome this, we propose a **Deep Q-Network (DQN)** that:
- Learns portfolio allocation dynamically  
- Responds to changing market conditions  
- Balances growth with risk management  

**Objective:** Maximize risk-adjusted returns while minimizing drawdowns.

---

##  Reinforcement Learning Framework

### **Agent**
Deep Q-Network (DQN) that learns optimal rebalancing decisions.

### **Environment**
Historical financial market data.

### **State Space (5-Dimensional)**
- 3 Portfolio weights (Stocks, Bonds, Cash)  
- 2 Scaled market indicators (Stock return, Bond return)

### **Action Space**
1. Increase Stock weight / Decrease Bond weight  
2. Increase Bond weight / Decrease Stock weight  
3. Hold current allocation  

Rebalance step = **5%**, Max allocation = **70%**, No short selling.

### **Reward**
Profit after each rebalance expressed as continuous log-return:

   Reward = ln(new balance) − ln(old balance) − penalty
Captures compounded portfolio growth and discourages over-trading.

### **Value Function**
Final cumulative profit after training.

---

##  Environment Setup

- **Initial Portfolio Weights:** (1/3, 1/3, 1/3)  
- **Initial Balance:** ₹100,000  
- **Assets:** Stocks, Bonds, Cash  
- **Constraints:**  
  - Rebalance step = 5%  
  - Max per asset = 70%  
  - No short selling

Derived daily returns include:
- **Stocks Return** = (P_t / P_(t-1)) − 1
- **Bonds Return** = (B_t / B_(t-1)) − 1
- **Cash Return** = FD Rate / 25200

---

##  Model Architecture

- **Algorithm:** Deep Q-Network (DQN)  
- **Input Layer:** 5 features  
- **Hidden Layer 1:** 64 neurons (ReLU)  
- **Hidden Layer 2:** 64 neurons (ReLU)  
- **Output Layer:** 3 actions (Linear)  
- **Target Network:** Stabilizes learning via periodic sync  
- **Loss Function:** Mean Squared Error (MSE)

---

##  Learning Phase

### **Reward:**
 **Reward = ln(new balance) − ln(old balance)**
Measures continuous compounded portfolio growth.

### **Core Components**
- **Replay Buffer** — Stores past experiences  
- **Random Batch Sampling** — Breaks correlation  
- **Bellman Update:**  
   **Q(s, a) = reward + gamma * max Q_target(next_state, all actions)**

Teaches the agent to value current + future opportunities.

---

##  Inside Each Training Loop

1. **Agent observes** current state (returns, weights).  
2. **Agent selects an action** using epsilon-greedy policy:
   - Explore (random action)  
   - Exploit (best predicted Q-value)
3. **Environment applies action** → portfolio rebalanced.  
4. **Market moves ahead one day** → reward + next state generated.  
5. **Experience stored** in replay memory.  
6. **If dataset not finished**, continue; else episode ends.

---

##  Configuration Parameters

### **Training**
- Episodes: 100  
- Learning Rate: 0.001  
- Gamma: 0.99  

### **Exploration**
- Epsilon-Greedy:  
  - Max ε = 1.0  
  - Min ε = 0.01  
  - Decay = 0.995  

### **Memory**
- Replay Buffer Size = 10,000  
- Batch Size = 64

---

##  Evaluation Metrics

- **Sharpe Ratio** ↑ — Measures risk-adjusted return  
- **Max Drawdown** ↓ — Measures downside protection  
- **Final Portfolio Balance** ↑  
- **Cumulative Log-Reward** → Overall performance signal  

The agent demonstrated:
- Defensive behavior during high-volatility phases  
- Growth-focused allocation during stable markets  
- Improved stability vs. static allocation strategies

---

##  ROI – Business Perspective

**Return on Investment (ROI)** indicates how effectively capital is deployed to generate returns.  
From a business viewpoint:

- ROI measures **value creation** achieved through intelligent rebalancing.  
- Shows **how efficiently** the model converts market signals into financial gains.  
- Acts as a **performance benchmark** for evaluating strategy success.  
- Higher ROI suggests **strategic financial efficiency** and disciplined risk control.  
- Helps investors allocate funds to the most profitable strategies.

In this project, a high ROI means the agent not only earns returns but does so **intelligently, consistently, and with reduced risk**.

---
 
