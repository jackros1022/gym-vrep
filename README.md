OpenAI Gym 入门与提高（一） Gym环境构建与最简单的RL agent
Openai gym是一个用于开发和比较RL算法的工具包，与其他的数值计算库兼容，如tensorflow或者theano库。现在主要支持的是python语言，以后将支持其他语言。gym文档在https://gym.openai.com/docs。

Openai gym包含2部分：

1、gym开源库：包含一个测试问题集，每个问题成为环境（environment），可以用于自己的RL算法开发。这些环境有共享的接口，允许用户设计通用的算法。其包含了deep mind 使用的Atari游戏测试床。

2、Openai gym服务：提供一个站点和api允许用户对他们训练的算法进行性能比较。

总之，openai gym 是一个RL算法的测试床（testbed）。

在增强学习中有2个基本概念，一个是环境（environment），称为外部世界，另一个为智能体agent（写的算法）。agent发送action至environment，environment返回观察和回报。

gym的核心接口是Env，作为统一的环境接口。Env包含下面几个核心方法：

1、reset(self):重置环境的状态，返回观察。

2、step(self,action):推进一个时间步长，返回observation，reward，done，info

3、render(self,mode=’human’,close=False):重绘环境的一帧。默认模式一般比较友好，如弹出一个窗口。

more…..

了解更多内容请下载下面的pdf文档：

# gym-vrep (WORK IN PROGRESS)
OpenAI gym environment for V-REP. This environment should provide a serviceable baseline for any reinforcement learning ventures involving robots in V-REP. But you'll probably still have to configure the environment to your own needs, and potentially extend the base environment to encompass more V-REP functionality.

![Imgur](http://i.imgur.com/dXMaIRp.png)

This is far from a finished product, so keep that in mind if you try to follow along.

## Installation
You'll need to install [V-REP](http://www.coppeliarobotics.com/downloads.html). Also the included scenes/models are pretty rudimentary so if you have a specific robot in mind you should have that already built in V-REP.
```
git clone https://github.com/bhyang/gym-vrep.git
cd gym-vrep
pip install -e .
```
If you're running Linux, then you're done with installation. Otherwise, you need to navigate to your V-REP folder and go to `programming/remoteAPIBindings/lib/lib/`, 32-bit or 64-bit depending on your system, and copy and paste that file into the repo at `/gym_vrep/envs/`. You should also delete `remoteApi.so`.

## Usage
There are a few caveats when it comes to running this. For one thing, you need to have V-REP open already whenever you execute a script. You also have to have the scene files you plan on using in the same directory as your script (if you want to use the sample scenes, you can either move them or run your script in `gym_vrep/scenes/`. If you've been using V-REP for some time, you may have changed the remote API port, in which case you should modify `vrep_env.py` to reflect that change.
```
import gym, gym_vrep
env = gym.make('vrep-pioneer-v0[or whatever your custom environment is]')
for _ in range(1000):
    if _ != 0:
        env.reset()
    action = (0, 0)
    ob, reward, done, diagnostic = env.step(action)
    # [insert revolutionary machine learning magic here]
    # No need to render since V-REP should already be open    
```
As of now the `vrep_env.py` base class only supports getting velocity and position data, as well as setting the velocity of a target joint. Once I have time I'll probably extend the API, but if you'd like extending it should be super easy (the remote API functions for Python are well-documented [here](http://www.coppeliarobotics.com/helpFiles/en/remoteApiFunctionsPython.htm)). As it stands, new classes also need to have all handles given upon initialization.

## To Do
1. A lot
2. Extending the remote function API
3. Running a new temporary instance of V-REP from within the package
4. Implementing a real reward system/task for the examples
5. Fetching handles real-time with a cache
6. Better visualizations

