# Simulation Framework
## Environment
The environment contains n+1 agents. N of them are humans controlled by certain unknown
policy. The other is robot and it's controlled by one known policy.
The environment is built on top of OpenAI gym library, and has implemented two abstract methods.
* reset(): the environment will reset positions for all the agents and return observation 
for robot. Observation for one agent is the observable states of all other agents.
* step(action): taking action of the robot as input, the environment computes observation
for each agent and call agent.act(observation) to get actions of agents. Then environment detects
whether there is a collision between agents. If not, the states of agents will be updated. Then 
observation, reward, done will be returned.


## Agent
Agent is a base class, and has two derived class of human and robot. Agent class holds
all the physical properties of an agent, including position, velocity, orientation, policy and etc.
* visibility: humans are always visible, but robot can be set to be visible or invisible
* sensor: can be either visual input or coordinate input
* kinematics: can be either holonomic (move in any direction) or unicycle (has rotation constraints)
* act(observation): transform observation to state and pass it to policy


## Policy
Policy takes state as input and output an action. Current available policies:
* ORCA: compute collision-free velocity under the reciprocal assumption
* CADRL: learn a value network to predict the value of a state and during inference it predicts action for the most important human
* LSTM-RL: use lstm to encode the human states into one fixed-length vector
* SARL: use pairwise interaction module to model human-robot interaction and use self-attention to aggregate humans' information
* OM-SARL: extend SARL by encoding intra-human interaction with a local map


## State
There are multiple definition of states in different cases. The state of an agent representing all
the knowledge of environments is defined as JointState, and it's different from the state of the whole environment.
* ObservableState: position, velocity, radius of one agent
* FullState: position, velocity, radius, goal position, preferred velocity, rotation
* DualState: concatenation of one agent's full state and one another agent's observable state
* JoinState: concatenation of one agent's full state and all other agents' observable states 


## Action
There are two types of actions depending on what kinematics constraint the agent has.
* ActionXY: (vx, vy) if kinematics == 'holonomic'
* ActionRot: (velocity, rotation angle) if kinematics == 'unicycle'仿真框架
##
##环境
环境包含n+1个代理。他们中的人是由某些未知政策控制的人类。另一个是机器人，它受到一项已知政策的控制。该环境建立在Openai Gym库的顶部，并实施了两种抽象方法。

* RESET（）：环境将重置所有代理的位置，并为机器人返回观察。一个代理的观察是所有其他代理的可观察状态。
* step(action)：代理的动作作为输入，环境为每个代理计算观测并且调用agent.act(observation)让agent采取动作。然后环境检测到代理之间是否存在碰撞。如果没有，将更新代理状态。然后将返回观察，奖励，done。

##agent
代理是基类，具有两个派生的人类和机器人类。代理类拥有代理的所有物理属性，包括位置，速度，方向，策略等。

可见性：人类总是可见的，但机器人可以被设置为可见或看不见的
传感器：可以是视觉输入或坐标输入
运动学：可以是自动化（朝任何方向移动）或独轮车（具有旋转约束）
ACT（观察）：将观察转换为陈述并将其传递给政策
##政策
策略将状态视为输入并输出动作。当前可用政策：

ORCA：在reciprocal假设下计算无碰撞速度
CADRL：学习一个价值网络，以预测状态的价值，并在推断期间预测最重要人类的行动
LSTM-RL：使用LSTM将人类状态编码为一个固定长度矢量
SARL：使用成对交互模块对人类机器人的相互作用进行建模并使用自我注意力来汇总人类的信息
OM-SARL：通过与本地地图编码人类内部相互作用来扩展SARL

##状态
在不同情况下，状态有多个定义。代表环境所有知识的代理的状态被定义为JointState，它与整个环境的状态不同。

ObservableState：位置，速度，一个agent的半径
FullState：位置，速度，半径，目标位置，首选速度，旋转
DualState：一个代理的完整状态和彼此可观察的状态的串联
JointState：一个代理的完整状态和所有其他代理可观察状态的串联

##动作
有两种类型的动作，具体取决于代理的限制的运动学。
ActionXy ：（ VX，VY）如果运动学=='holonomic'
ActionRot ：（速度，旋转角）如果运动学=='Unicycle'