# Instructions & Insights for Playing

## Substrates and Scenarios

> | Substrate                              | Scenarios                                |
> |----------------------------------------|------------------------------------------|
> | allelopathic_harvest__open             | allelopathic_harvest__open_0             |
> |                                        | allelopathic_harvest__open_1             |
> |                                        | allelopathic_harvest__open_2             |
> | clean_up                               | clean_up_2                               |
> |                                        | clean_up_3                               |
> |                                        | clean_up_4                               |
> |                                        | clean_up_5                               |
> |                                        | clean_up_6                               |
> |                                        | clean_up_7                               |
> |                                        | clean_up_8                               |
> | prisoners_dilemma_in_the_matrix__arena | prisoners_dilemma_in_the_matrix__arena_0 |
> |                                        | prisoners_dilemma_in_the_matrix__arena_1 |
> |                                        | prisoners_dilemma_in_the_matrix__arena_2 |
> |                                        | prisoners_dilemma_in_the_matrix__arena_3 |
> |                                        | prisoners_dilemma_in_the_matrix__arena_4 |
> |                                        | prisoners_dilemma_in_the_matrix__arena_5 |
> | territory__rooms                       | territory__rooms_0                       |
> |                                        | territory__rooms_1                       |
> |                                        | territory__rooms_2                       |
> |                                        | territory__rooms_3                       |

### Prisoner's Dilemma in the Matrix.

#### Abstract

[![Substrate video](https://img.youtube.com/vi/bQkEKc1zNuE/0.jpg)](https://youtu.be/bQkEKc1zNuE)

See _Running with Scissors in the Matrix_ for a general description of the game
dynamics. Here the payoff matrix represents the Prisoner's Dilemma game. `K = 2`
resources represent "cooperate" and "defect" pure strategies.

Players have the default `11 x 11` (off center) observation window.

**省流：**

<!-- TODO: 待验证，可能不对 -->
1. 8 人游戏
2. 每个 agent 只有 `11 x 11` (off center) observation window
3. agents 可以选择蓝块 or 红块（触碰），选择后出现白帽作为标记
4. agent_0 可以和其他 agents_n 猜拳（通过激光照射，AOE）
   1. 进入猜拳状态后，agent 会被标记上 
   
B-B
2.6875-2.6875
B-R
1.33333-3.41667
R-B
3.25-1.16667
R-R
2.1-1.27
#### Score

对于每个 agent：

| Event | Rreward |
| ----- | ------- |


#### 动作空间

```python
a = ['W', 'A', 'S', 'D', 'Q', 'E', 'SPACE']
```

| 按键 | 动作 |
| ---- | ---- |


#### 环境

#### 策略（直觉）

1. 




### Allelopathic Harvest.

#### Abstract

[![Substrate video](https://img.youtube.com/vi/ESugMMdKLxI/0.jpg)](https://youtu.be/ESugMMdKLxI)

This substrate contains three different varieties of berry (red, green, & blue) and a fixed number of berry patches, which could be replanted to grow any color variety of berry. The growth rate of each berry variety depends linearly on the fraction that that color comprises of the total. Players have three planting actions with which they can replant berries in their chosen color. All players prefer to eat red berries (reward of 2 per red berry they eat versus a reward of 1 per other colored berry). Players can achieve higher return by selecting just one single color of berry to plant, but which one to pick is, in principle, difficult to coordinate (start-up problem) -- though in this case all prefer red berries, suggesting a globally rational chioce. They also always prefer to eat berries over spending time planting (free-rider problem).

**省流：**

1. 有 3 种浆果：红、绿、蓝
2. agent 可以种任何果
3. 某浆果占比越多，其生成速度越快，二者线性正相关
4. agent 可以吃任何果（浆果成熟后）获得奖励
5. agent 可以杀任何其他 agents

#### Score

对于每个 agent：

| Event        | Rreward |
| ------------ | ------- |
| 吃红果       | 2       |
| 吃绿 or 蓝果 | 1       |
| 被杀         | -10     |

#### 动作空间

```python
a = ['W', 'A', 'S', 'D', 'Q', 'E', '1', '2', '3', 'SPACE']
```

| 按键                  | 动作                                                    |
| --------------------- | ------------------------------------------------------- |
| `W` / `A` / `S` / `D` | 移动（碰到果自动吃）                                    |
| `Q` / `E`             | 左/右转                                                 |
| `1` / `2` / `3`       | 种下红/绿/蓝果                                   |
| `SPACE`               | 发射 zapper（AOE），第一次索敌（会标记上X），第二次击杀 |
| `TAB`                 | 换人                                                    |

#### 环境

1. `W` / `A` / `S` / `D` & `SPACE` & `1` / `2` / `3` 可能会被其他 agents 阻挡
2. 被击杀的 agent 会在随机位置复活

#### 策略（直觉）

1. start-up problem
2. free-rider problem
3. 尽量避免击杀，扣分太多
4. 守着自己的果园，不要去别人的果园？
   
### Clean Up.

#### Abstract

[![Substrate video](https://img.youtube.com/vi/jOeIunFtTS0/0.jpg)](https://youtu.be/jOeIunFtTS0)

Clean Up is a seven player game. Players are rewarded for collecting apples. In Clean Up, apples grow in an orchard at a rate inversely related to the cleanliness of a nearby river. The river accumulates pollution at a constant rate. The apple growth rate in the orchard drops to zero once the pollution accumulates past a threshold value. Players have an additional action allowing them to clean a small amount of pollution from the river in front of themselves. They must physically leave the apple orchard to clean the river. Thus, players must maintain a public good of high orchard regrowth rate through effortful contributions. This is a public good provision problem because the benefit of a healthy orchard is shared by all, but the costs incurred to ensure it exists are born by individuals.

Players are also able to zap others with a beam that removes any player hit by it from the game for 50 steps.

**省流：**

1. 7 人游戏，目标是收集苹果
2. 果园中苹果的生长速度与附近河流的清洁度成反比
3. 河流以恒定速度积累污染，污染超过阈值后，苹果停止生长
4. agent 可以清理河流（AOE）
5. agent 可以击杀其他 agents（AOE），被击杀的 agent 50 步后复活
   > TODO: 谁的 50 步？击杀者的 50 步？击杀者的 50 步后，被击杀者复活在哪里？

#### Score

对于每个 agent：

| Event  | Rreward |
| ------ | ------- |
| 收苹果 | ?       |

> TODO: 收苹果的 reward 是多少？

#### 动作空间

```python
a = ['W', 'A', 'S', 'D', 'Q', 'E', '1', '2']
```

| 按键                  | 动作                  |
| --------------------- | --------------------- |
| `W` / `A` / `S` / `D` | 移动（碰到果自动收）  |
| `Q` / `E`             | 左/右转               |
| `1`                   | 一击必杀 agent（AOE） |
| `2`                   | 清理河流（AOE）       |
| `TAB`                 | 换人                  |


#### 环境

同上

#### 策略（直觉）

1. public good provision problem
2. 清理河流工作量很大，但是收益是共享的
3. 主要是合作，要合理分配清理和收果
4. 避免击杀：地方大，人多，击杀的收益不大


### Territory: Rooms.

[![Substrate video](https://img.youtube.com/vi/u0YOiShqzA4/0.jpg)](https://youtu.be/u0YOiShqzA4)

#### Abstract

See _Territory: Open_ for the general description of the mechanics at play in
this substrate.

In this substrate, _Territory: Rooms_, individuals start in segregated rooms
that strongly suggest a partition individuals could adhere to. They can break
down the walls of these regions and invade each other's "natural territory", but
the destroyed resources are lost forever. A peaceful partition is possible at
the start of the episode, and the policy to achieve it is easy to implement. But
if any agent gets too greedy and invades, it buys itself a chance of large
rewards, but also chances inflicting significant chaos and deadweight loss on
everyone if its actions spark wider conflict. The reason it can spiral out of
control is that once an agent's neighbor has left their natural territory then
it becomes rational to invade the space, leaving one's own territory undefended,
creating more opportunity for mischief by others.

**省流：**

1. 认领领地，碰到墙自动认领资源，100 timesteps 后资源激活，以每时间步 0.01 的比率随机向领取资源的玩家提供奖励
2. agent 可以拆墙（AOE），拆墙后墙消失，墙上的资源消失
3. agent 可以击杀其他 agents（AOE），被击杀的 agent 永久消失
   
#### Score

对于每个 agent：

| Event      | Rreward                                           |
| ---------- | ------------------------------------------------- |
| 资源被激活 | 每时间步 0.01 的比率随机向资源所属 agent 提供奖励 |
| 被击杀     | 清零                                              |

#### 动作空间

```python
a = ['W', 'A', 'S', 'D', 'Q', 'E', 'SPACE']
```

| 按键                  | 动作                                              |
| --------------------- | ------------------------------------------------- |
| `W` / `A` / `S` / `D` | 移动                                              |
| `Q` / `E`             | 左/右转                                           |
| `SPACE`               | 攻击（AOE），类似 allenlopathic_harvest，两步操作 |
| `TAB`                 | 换人                                              |

#### 环境

#### 策略（直觉）

1. 一旦有人拆墙，就会引发连锁反应，大家都会拆墙
2. 拆墙后，资源被激活，但是资源是随机分配的，所以拆墙后，资源可能被其他人拿走

## Evaluation Metric

参赛者提交待评估的种群，每个底物一个。然后，对该底物的所有情景进行评估。对于每个情景，我们计算焦点人均奖励。

为了使不同底物之间的评估保持一致，根据MP2.0的基准范围对焦点人均奖励进行归一化处理，其中在某种情景中表现最差的基准被分配一个得分为 0，表现最好的基准被分配一个得分为1。

如果参赛者的某种情景得分在此范围之外，表示他们的表现比MP2.0中最差的基准更差或比最好的基准更好。该归一化过程产生了“情景得分”。随后，底物的种群得分被计算为其情景得分的平均值。考虑到每个底物由不同数量的情景组成，对于每个底物，得分都是平均的。

最终得分，决定参与者的排名，是通过对所有四个底物的得分进行平均计算得到的。

## Instructions

### 环境设置

```shell
conda activate mpc_main \
&& cd ~/Projects/Melting-Pot-Contest-2023 \
&& substrate=(pd_arena al_harvest clean_up territory_rooms) \
&& path=(./results/torch/${substrate[0]}/PPO_meltingpot_a0cbb_00000_0_2023-10-31_14-20-46 \
./results/torch/${substrate[1]}/PPO_meltingpot_b833f_00000_0_2023-10-31_14-50-03 \
./results/torch/${substrate[2]}/PPO_meltingpot_086de_00000_0_2023-10-31_15-06-37 \
./results/torch/${substrate[3]}/PPO_meltingpot_18d19_00000_0_2023-10-31_15-07-04) \
&& n=0
```

| n   | Substrate       |
| --- | --------------- |
| 0   | pd_arena        |
| 1   | al_harvest      |
| 2   | clean_up        |
| 3   | territory_rooms |

### Play!

```shell
# allelopathic_harvest__open
python meltingpot/human_players/play_allelopathic_harvest.py

# clean_up
python meltingpot/human_players/play_clean_up.py

# prisoners_dilemma_in_the_matrix__arena
python meltingpot/human_players/play_anything_in_the_matrix.py --level_name=prisoners_dilemma_in_the_matrix__arena

# territory__rooms
python meltingpot/human_players/play_territory.py
```

### Run Training

```shell
python baselines/train/run_ray_train.py --exp=${substrate[$n]}
# python baselines/train/run_ray_train.py --exp=${substrate[$n]} --as-test=True
```

> Note: 默认使用远程服务器

### Run Evaluation

```shell
python baselines/evaluation/evaluate.py \
--config_dir=${path[$n]} \
--policies_dir=${path[$n]}/checkpoint_000001/policies
```

#### 无法创建场景视频（未解决）

```shell
# --create_videos=True --video_dir='./results/videos' 报错：
OpenCV: FFMPEG: tag 0x30397076/'vp90' is not supported with codec id 167 and format 'webm / WebM'
```

#### Evaluation Results

```shell
# prisoners_dilemma_in_the_matrix__arena
Results for prisoners_dilemma_in_the_matrix__arena: 
   background_per_capita_return background_player_names  \
0                           NaN                      ()   
1                           NaN                      ()   

  background_player_returns  focal_per_capita_return  \
0                        []                 2.614202   
1                        []                 3.378513   

                                  focal_player_names  \
0  [agent_6, agent_1, agent_7, agent_2, agent_6, ...   
1  [agent_6, agent_2, agent_4, agent_7, agent_7, ...   

                                focal_player_returns  
0  [3.776470588235294, 5.721153846153847, 3.19230...  
1  [8.648529411764706, 4.811111111111112, 4.05000...
```

```shell
# allelopathic_harvest__open
Results for allelopathic_harvest__open: 
   background_per_capita_return background_player_names  \
0                           NaN                      ()   
1                           NaN                      ()   

  background_player_returns  focal_per_capita_return  \
0                        []                  -22.500   
1                        []                  -12.375   

                                  focal_player_names  \
0  [agent_2, agent_4, agent_0, agent_0, agent_5, ...   
1  [agent_3, agent_2, agent_2, agent_4, agent_4, ...   

                                focal_player_returns  
0  [-17.0, -21.0, -67.0, 18.0, -50.0, -31.0, -36....  
1  [3.0, -10.0, -6.0, -15.0, -14.0, 10.0, -2.0, -... 
```

```shell
# clean_up
Results for clean_up: 
   background_per_capita_return background_player_names  \
0                           NaN                      ()   
1                           NaN                      ()   

  background_player_returns  focal_per_capita_return  \
0                        []                      0.0   
1                        []                      0.0   

                                  focal_player_names  \
0  [agent_3, agent_0, agent_5, agent_6, agent_0, ...   
1  [agent_1, agent_4, agent_5, agent_6, agent_1, ...   

                  focal_player_returns  
0  [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]  
1  [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
```

```shell
# territory__rooms
Results for territory__rooms: 
   background_per_capita_return background_player_names  \
0                           NaN                      ()   
1                           NaN                      ()   

  background_player_returns  focal_per_capita_return  \
0                        []                74.666667   
1                        []                90.444444   

                                  focal_player_names  \
0  [agent_3, agent_1, agent_4, agent_5, agent_0, ...   
1  [agent_2, agent_5, agent_4, agent_2, agent_6, ...   

                                focal_player_returns  
0  [40.0, 100.0, 93.0, 131.0, 42.0, 24.0, 103.0, ...  
1  [144.0, 15.0, 29.0, 19.0, 145.0, 106.0, 37.0, ...  
```

### Visualization

```shell
python baselines/train/render_models.py \
--config_dir=${path[$n]} \
--policies_dir=${path[$n]}/checkpoint_000001/policies
```
