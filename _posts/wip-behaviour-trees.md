# Behavior Tree Design
* [Behavior Trees: Breaking the Cycle of Misuse](https://takinginitiative.wordpress.com/2020/01/07/behavior-trees-breaking-the-cycle-of-misuse/) and Bobby Anguelov's portion of [AI Arborist: Proper Cultivation and Care for Your Behavior Trees](https://youtu.be/Qq_xX1JCreI?t=1160).
    * Core thesis is that BTs are good at executing plans but bad at cyclic behaviours. I call this the brain-body divide. You want your AI to have two aspects: the brain and the body. The brain sets goals and the body executes them. The body requires logic to execute these goals: aiming, firing, navigating, taking cover, etc. You don't want one BT that handles both tasks. In the past, I thought two BTs would work well, but Anguelov suggests using a Hierarchical Finite State Machine (HFSM) for the brain and a behaviour tree for the body.

# Behavior Tree Implementation
Some resources that go over increasingly complex aspects of Behavior Tree implementation.

* [The Behavior Tree Starter Kit](https://www.gameaipro.com/GameAIPro/GameAIPro_Chapter06_The_Behavior_Tree_Starter_Kit.pdf) ([source code](https://github.com/aigamedev/btsk))
    * Describes the basic nodes of a behaviour tree and their implementation. Goes through first generation (naive) and second generation (optimizations for space and time) implementations.
* [Understanding the Second Generation of Behavior Trees](https://www.youtube.com/watch?v=n4aREFb3SsU)
    * Video talk version of the second generation of behavior trees discussed in The Behavior Tree Starter Kit.
* Mika Vehkala's portion of [AI Arborist: Proper Cultivation and Care for Your Behavior Trees](https://youtu.be/Qq_xX1JCreI).
    * I forget what this was about. Mostly tips for better implementation and visualization?
* [Synchronized Behavior Trees](https://takinginitiative.wordpress.com/2014/02/17/synchronized-behavior-trees/)
    * Alternative to event-driven behaviour trees. Somewhat similar, but you re-evaluate the tree when monitors detect a change. Also, you have global state to make organizing groups of agents easier.

