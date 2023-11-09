# Substrate and scenario details

## Details of specific substrates

## Allelopathic Harvest.

[![Substrate video](https://img.youtube.com/vi/ESugMMdKLxI/0.jpg)](https://youtu.be/ESugMMdKLxI)

This substrate contains three different varieties of berry (red, green, & blue)
and a fixed number of berry patches, which could be replanted to grow any color
variety of berry. The growth rate of each berry variety depends linearly on the
fraction that that color comprises of the total. Players have three planting
actions with which they can replant berries in their chosen color. All players
prefer to eat red berries (reward of 2 per red berry they eat versus a reward of
1 per other colored berry). Players can achieve higher return by selecting just
one single color of berry to plant, but which one to pick is, in principle,
difficult to coordinate (start-up problem) -- though in this case all prefer red
berries, suggesting a globally rational chioce. They also always prefer to eat
berries over spending time planting (free-rider problem).

Allelopathic Harvest was first described in Koster et al. (2020).

Köster, R., McKee, K.R., Everett, R., Weidinger, L., Isaac, W.S., Hughes, E.,
Duenez-Guzman, E.A., Graepel, T., Botvinick, M. and Leibo, J.Z., 2020.
Model-free conventions in multi-agent reinforcement learning with heterogeneous
preferences. arXiv preprint arXiv:2010.09054.

## Clean Up.

[![Substrate video](https://img.youtube.com/vi/jOeIunFtTS0/0.jpg)](https://youtu.be/jOeIunFtTS0)

Clean Up is a seven player game. Players are rewarded for collecting apples. In
Clean Up, apples grow in an orchard at a rate inversely related to the
cleanliness of a nearby river. The river accumulates pollution at a constant
rate. The apple growth rate in the orchard drops to zero once the pollution
accumulates past a threshold value. Players have an additional action allowing
them to clean a small amount of pollution from the river in front of themselves.
They must physically leave the apple orchard to clean the river. Thus, players
must maintain a public good of high orchard regrowth rate through effortful
contributions. This is a public good provision problem because the benefit of a
healthy orchard is shared by all, but the costs incurred to ensure it exists are
born by individuals.

Players are also able to zap others with a beam that removes any player hit by
it from the game for 50 steps.

Clean Up was first described in Hughes et al. (2018).

Hughes, E., Leibo, J.Z., Phillips, M., Tuyls, K., Duenez-Guzman, E., Castaneda,
A.G., Dunning, I., Zhu, T., McKee, K., Koster, R. and Roff, H., 2018, Inequity
aversion improves cooperation in intertemporal social dilemmas. In Proceedings
of the 32nd International Conference on Neural Information Processing Systems
(pp. 3330-3340).

## Prisoner's Dilemma in the Matrix.

[![Substrate video](https://img.youtube.com/vi/bQkEKc1zNuE/0.jpg)](https://youtu.be/bQkEKc1zNuE)

See _Running with Scissors in the Matrix_ for a general description of the game
dynamics. Here the payoff matrix represents the Prisoner's Dilemma game. `K = 2`
resources represent "cooperate" and "defect" pure strategies.

Players have the default `11 x 11` (off center) observation window.

## Territory: Rooms.

[![Substrate video](https://img.youtube.com/vi/u0YOiShqzA4/0.jpg)](https://youtu.be/u0YOiShqzA4)

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

## Scenario details

### Allelopathic Harvest

1.  _Focals are resident and a visitor prefers green._ This is a resident-mode
    scenario with a background population of A3C bots. The focal population must
    recognize that a single visitor bot has joined the game and is persisting in
    replanting berry patches in a color the resident population does not prefer
    (green instead of their preferred red). The focal agents should zap the
    visitor to prevent them from planting too much.

1.  _Visiting a green preferring population._ This is a visitor-mode scenario
    with a background population of A3C bots. Four focal agents join twelve from
    the background population. In this case the background population strongly
    prefers green colored berries. They plant assiduously so it would be very
    difficult to stop them from making the whole map green. The four focal
    agents prefer red berries but need to recognize that in this case they need
    to give up on planting berries according to their preferred convention and
    instead join the resident population in following their dominant convention
    of green. Otherwise, if the focal agents persist in planting their preferred
    color then they can succeed only in making everyone worse off.

1.  _Universalization_. Agents face themselves in this setting. As a
    consequence, all players have congruent incentives alleviating the conflict
    between groups that desire different berry colors. This test also exposes
    free rider agents that rely on others to do the work of replanting.

### Clean Up

1.  _Visiting an altruistic population._ This is a visitor-mode scenario where
    three focal agents join four from the background population. The background
    bots are all interested primarily in cleaning the river. They rarely consume
    apples themselves. The right choice for agents of the focal population is to
    consume apples, letting the background population do the work of maintaining
    the public good.

1.  _Focals are resident and visitors free ride._ This is a resident-mode
    scenario where four focal agents are joined by three from the background
    population. The background bots will only consume apples and never clean the
    river. It tests if the focal population is robust to having a substantial
    minority of the players (three out of seven) be _defectors_ who never
    contribute to the public good. Focal populations that learned too brittle of
    a division of labor strategy, depending on the presence of specific focal
    players who do all the cleaning, are unlikely to do well in this scenario.

1.  _Visiting a turn-taking population that cleans first._ This is a
    visitor-mode scenario where three focal agents join four agents from the
    background population. The background bots implement a strategy that
    alternates cleaning with eating every 250 steps. These agents clean in the
    first 250 steps. Focal agents that condition an unchangeable choice of
    whether to clean or eat based on what their coplayers do in the beginning of
    the episode (a reasonable strategy that self-play may learn) will perceive
    an opening to eat in this case. A better strategy would be to take turns
    with the background bots: clean when they eat and eat when they clean.

1.  _Visiting a turn-taking population that eats first._ This is a visitor-mode
    scenario where three focal agents join four agents from the background
    population. As in scenario `3`, the background bots implement a strategy
    that alternates cleaning with eating every 250 steps. However, in this case
    they start out by eating in the first 250 steps. If the focal population
    learned a policy resembling a _grim trigger_ (cooperate until defected upon,
    then defect forever after), then such agents will immediately think they
    have been defected upon and retaliate. A much better strategy, as in SC2, is
    to alternate cleaning and eating out of phase with the background
    population.

1.  _Focals are visited by one reciprocator._ This is a resident-mode scenario
    where six focal agents are joined by one from the background population. In
    this case the background bot is a conditional cooperator. That is, it will
    clean the river as long as at least one other agent also cleans. Otherwise
    it will (try to) eat. Focal populations with at least one agent that
    reliably cleans at all times will be successful. It need not be the same
    cleaner all the time, and indeed the solution is more equal if all take
    turns.

1.  _Focals are visited by two suspicious reciprocators._ This is resident-mode
    scenario where five focal agents are joined by two conditional cooperator
    agents from the background population. Unlike the conditional cooperator in
    scenario `5`, these conditional cooperators have a more stringent condition:
    they will only clean if at least two other agents are cleaning. Otherwise
    they will eat. Thus, two cleaning agents from the focal population are
    needed at the outset in order to get the background bots to start cleaning.
    Once they have started then one of the two focal agents can stop cleaning.
    This is because the two background bots will continue to clean as long as,
    from each agent's perspective, there are at least two other agents cleaning.
    If any turn taking occurs among the focal population then they must be
    careful not to leave a temporal space when no focal agents are cleaning lest
    that cause the background bots to stop cooperating, after which they could
    only be induced to clean again by sending two focal agents to the river to
    clean

1.  _Focals are visited by one suspicious reciprocator._ This is a resident-mode
    scenario where six focal agents are joined by one conditional cooperator
    agent from the background population. As in scenario `6`, this conditional
    cooperator requires at least two other agents to be cleaning the river
    before it will join in and clean itself. The result is that the focal
    population must spare two agents to clean at all times, otherwise the
    background bot will not help. In that situation, the background bot joins as
    a third cleaner. But the dynamics of the substrate are such that it is
    usually more efficient if only two agents clean at once. Focal populations
    where agents have learned to monitor the number of other agents cleaning at
    any given time and return to the apple patch if more than two are present
    will have trouble in this scenario because once one of those agents leaves
    the river then so too will the background bot, dropping the total number
    cleaning from three down to one, which is even more suboptimal since a
    single agent cannot clean the river effectively enough to keep up a high
    rate of apple growth. The solution is that the focal population must notice
    the behavior of the conditional cooperating background bot and accept that
    there is no way to achieve the optimal number of cleaners (two), and instead
    opt for the least bad possibility where three agents (including the
    background conditional cooperator) all clean together.

1.  _Universalization_. This test exposes both free riders and overly altruistic
    policies. Both will get low scores here.

### Prisoner's Dilemma in the Matrix

Players are identifiable by color (randomized between episodes but maintained for each episode's duration).

1.  _Visiting unconditional cooperators._ This is a visitor-mode scenario where
    one agent sampled from the focal population joins seven agents from a
    background population. In this case all the background bots will play
    cooperative strategies (mostly collecting _cooperate_ resources and rarely
    collecting _defect_). The objective of a rational focal visitor is to
    exploit them by collecting _defect_ resources.

1.  _Focals are resident and visitors are unconditional cooperators._ This is a
    resident-mode scenario where six focal agents are joined by two agents from
    the background population. The background bots play cooperative strategies,
    as in scenario `1`. A focal population will do well by defecting against
    these unfamiliar cooperators, but it should take care to identify them
    specifically. If they defect against all players regardless of identity then
    they will end up defecting on the focal population as well, lowering the
    overall score achieved.

1.  _Focals are resident and visitors defect._ This is a resident-mode scenario
    where six focal agents are joined by two agents from the background
    population. The background bots play defecting strategies (mostly collecting
    _defect_ resources). A focal population will do well by avoiding interaction
    with them and instead preferentially interacting with one another, and
    cooperating.

1.  _Meeting gullible bots._ This is a half-and-half-mode scenario where four
    focal agents join four background bots. In this case the background
    population bots trained alongside (mostly) pure cooperator and defector
    bots, but were not themselves given any non-standard pseudoreward scheme.
    Thus they became bots that try to preferentially interact with partners who
    they have seen collecting cooperate resources. The best strategy against
    them is probably for the focal players to trick them into thinking they are
    defecting so they won't try to force interactions. This creates the
    necessary space to allow the focal population to preferentially interact
    (and cooperate) with one another.

1.  _Visiting a population of grim reciprocators._ This is a visitor mode
    scenario where one focal agent joins seven from the background population.
    The background population bots are conditional cooperators. They will
    collect cooperate resources and attempt to play cooperate in all
    interactions until they have twice been defected upon by their partners.
    After that, they try to retaliate by collecting defect resources and aiming
    to defect in all remaining encounters till the end of the episode. Once they
    start defecting they do so indiscriminately. They do not try to specifically
    punish the players who defected on them. This sparks a wave of recrimination
    that ends up causing all the background bots to defect after a short while.
    Since episodes always last 1000 steps, the best strategy for the focal agent
    is probably to cooperate up until near the end of the episode and then
    defect once there is no remaining time for retaliation.

1.  _Visiting a population of hair-trigger grim reciprocators._ Just like
    scenario `5` but with background bot conditional cooperators operating on
    more of a hair trigger. That is, unlike scenario `5` where the bots will
    forgive one defection, the bots here will not forgive any defection at all.
    They begin defecting indiscriminately after the first time any agent defects
    on them.

1.  _Universalization_. Cooperation biased agents will do well while defection
    biased agents will do poorly here. If an agent learned a tit-for-tat style
    policy then it would also do well here.

### Territory: Rooms

1.  _Focals are resident and visited by an aggressor._ This is a resident-mode
    scenario where eight agents from the focal population are joined by one
    agent sampled from a background population. The background population
    consists of agents that, although they did not themselves experience any
    special reward shaping, they trained alongside other agents that were
    incentivized to be extremely aggressive and destructive in their zapping
    behavior. As a result, they are fairly aggressive themselves while still
    being interested primarily in claiming territory. A focal population where
    agents have not forgotten how to defend themselves from attack will do much
    better in this scenario than a focal population that implements the same
    fair division of the resources but forgot self-defense.

1.  _Visiting a population of aggressors._ This is a visitor-mode scenario where
    one focal agent joins eight from a background population. The background
    population is the same as in scenario `1`. The focal visitor agent must be
    forever on its guard since neighboring agents may attack their territory at
    any time, permanently destroying resources on the way in. Since the larger
    set of background bots are so likely to fight among themselves, the optimal
    strategy for the focal visitor agent is often to invade after a battle and
    claim extra territory in areas where other agents have been zapped and
    removed.

1.  _Universalization_. This test exposes overly aggressive policies. They will
    perform very poorly.
