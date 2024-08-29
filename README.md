# Discovering Creative Behaviours through DUPLEX: Diverse Universal Features for PoLicy EXploration

The ability to approach the same problem from different angles is a cornerstone of human intelligence that leads to robust solutions and effective adaptation to problem variations. In contrast, current RL methodologies tend to lead to policies that settle on a single solution to a given problem, making them brittle to problem variations. Replicating human flexibility in reinforcement learning agents is the challenge that we explore in this work. We tackle this challenge by extending state-of-the-art approaches to introduce DUPLEX, a method that explicitly defines a diversity objective with constraints and makes robust estimates of policies’ expected behavior through successor features. The trained agents can (i) learn a diverse set of near-optimal policies in complex highly-dynamic environments and (ii) exhibit competitive and diverse skills in out-of-distribution (OOD) contexts. Empirical results indicate that DUPLEX improves over previous methods and successfully learns competitive driving styles in a hyper-realistic simulator (i.e., GranTurismo™ 7) as well as diverse and effective policies in several multi-context robotics MuJoCo simulations with OOD gravity forces and height limits. To the best of our knowledge, our method is the first to achieve diverse solutions in complex driving simulators and OOD robotic contexts.

## Results in Mu.Jo.Co.

### Experiment Ant: Jump with low gravity
We evaluate Duplex in an out-of-distribution (OOD) multi-context MuJoCo.Ant scenario. Here the agent is tasked to jump forward while operating under a gravitional force 46% lower than the minimum gravity included in the in training scenarios. In such context, `Policy-0` uses three legs while jumping -- resulting in the slowest policy completing the task. Differently, `Policy-1` executes rotating jumps, while `Policy-2` converged to the the most effective behavior by using the front two legs to propel in the air.


<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/mujoco_ant_jump_0.mp4" type="video/mp4">
          </video><p style="text-align:center;">MuJoCo Ant (Jump)</p>
        </td>
    </tr>
</table>


### Experiment Ant: Walk forward with high gravity

In this experimentm, we evaluate a DUPLEX agent in another OOD multi-context MuJoCo.Ant scenario. Akin to the previous case, the agent is tasked to move forward as fast as possible but now it is forced to move under a gravitational force that is 40% higher than the maximum gravity considered at training time. We observe that agent diversifies the usage of legs influencing the walking behavior of the policies. In particular, two policies exploit two out of four legs: `Policy-0` uses legs 2 and 4 while `Policy-1` uses legs 1 and 3 respectively. `Policy-2` instead, learns to use legs 1, 2 and 3 while limping on leg 4 occasionally.

<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/mujoco_ant_walk_0.mp4" type="video/mp4">
          </video><p style="text-align:center;">MuJoCo Ant (Walk)</p>
        </td>
    </tr>
</table>


### Experiment Walker: Jump while moving forward with low gravity
Similarly to the Ant scenario, we evaluate Duplex while operating OOD with a different agent while operating under a gravitational force 46% lower than the minimum gravity in the training set. In this scenario, all policies can successfully move forward and jump without falling while diversifying the legs openings and postures in the air. In particular, we observe that from left to right videoclip, the policies adopt diverse strategies widening more and more their legs angle while raising the knees incrementally higher.


<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/mujoco_walker_jump_0.mp4" type="video/mp4">
          </video><p style="text-align:center;">MuJoCo Walker (Jump)</p>
        </td>
    </tr>
</table>

### Experiment Walker: Walk with stronger gravity and with a spatial constraint
In this experiment we evaluate DUPLEX while operating in OOD dynamics while being constraied to walk below an "invisible" horizontal. Specifically, the agent operates with a gravitational force that is 46% higher that the ones in the training set and with a wall height that is 20% lower than the minimum height experienced at training time. That invisible wall forces all policies to bend the head. This is the most challenging task limits exploration for diversity since the agent has to guarantee task execution with all the policies. Nevertheless, we observe that both `Policy-0` and `Policy-1` periodically use little jumps to keep moving forward and that `Policy-1` widens the agent legs more than `Policy-0`. `Policy-2` instead manages to make actual jumps and still move forward.

<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/mujoco_walker_walk_0.mp4" type="video/mp4">
          </video><p style="text-align:center;">MuJoCo Walker (Walk)</p>
        </td>
    </tr>
</table>

## Results in GranTurismo™ 7

### Near-optimal diversity

In GranTurismo7 (GT), we observe that policies manage to score similar lap-times while being different. In particular, the DUPLEX agent converges to diverse skill in using the brake and overtaking to guarantee diverisity. To highlight some examples, at mark 00:10 `Policy-2` showcases a different driving line while overtaking the pack of cars in front. Moreover, at mark 01:34 we observe that `Policy-0` and `Policy-1` also employ different strategies while facing similar situations. In fact, `Policy-1` performs simple overtaks exploiting the slipstream, while `Policy-0` executes a slingshot pass.

<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/gt_near_optimality_comparison.mp4" type="video/mp4">
          </video><p style="text-align:center;">Near-Optimality</p>
        </td>
    </tr>
</table>

### Relaxing the optimality constraint to foster diversity
Once the optimality constraint becomes more forgiving, we observer that the DUPLEX agent explores different driving styles that also affect the cars lap-times. For example, we observe different driving lines and different braking areas when approaching a turn. Moreover, driving style become more creative since the agent is allowed to compromise optimality, and thus, we see `Policy-4` (bottom-right) discovering a drifting behavior to complete turns (mark 1:00 onwards).

<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/gt_baseline_diversity.mp4" type="video/mp4">
          </video><p style="text-align:center;">Performance vs Diversity</p>
        </td>
    </tr>
</table>

### Discovering diversity on specific successor features: Aggressiveness
Here, we report an interesting result that showcases how DUPLEX can be used to target diveristy on particular Successor Features, and thus, to make strategic behavior (that might be difficult implement in a reward function) emerge. For example, in order to provide diversity on the aggressiveness levels of the agent, we can simply use the observation on how often cars are being hit by the agent. This translates in various policy strategies that range from very timid and cautions agents to very aggressive ones that intetionally hit other cars (if the collision is not expected to result in significant speed loss).

<table>
    <tr>
        <td>
          <video controls preload=metadata width="325" height="175">
            <source src="https://media.githubusercontent.com/media/SonyResearch/project_game_ai_duplex/main/gt_aggressiveness.mp4" type="video/mp4">
          </video><p style="text-align:center;">Aggressiveness descriptors test</p>
        </td>
    </tr>
</table>
