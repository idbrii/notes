I should write an article about ticketing.


> I've noticed that no one ever mentiones ai directors/attack slot system/enemy manager when talking about ai even though it's a fairly important part of pretty much every game so I thought it might be worth noting.
> 
> This is more on the game design side of things than technical and it's an area which really depends on the type of combat featured in your game so it might be a fairly simple system or a very complex one.
> 
> The basics are: you never want every enemy that's currently fighting the player to attack at once. You should always limit the number of enemies that can attack because it is frustrating when you die because you missed one dodge/parry but since every enemy attacked at once their damage stacked and you died.
> 
> Now you might be thinking but what about the most hardcore games like Dark Souls? They have lots of enemies and you die easily, so they clearly don't use this, right? Wrong. 
> 
> Dark Souls is one of the best examples of this system, try running into as many enemies as possible and watch that only 2/3 are attacking you while the rest is idely standing by in close proximity waiting for you to get closer to them.
> 
> More complex versions of this system can be based on tokens (I know that Hellblade Senua's Sacrifice uses this). Where enemies have special attacks which cost certain amount of tokens and there can only be so many tokens in any given fight. 
> 
> Then you have a game like Doom 2016 where enemies who are standing in front of you have a higher priority for attacks than enemies who are behind you.
-- [DareCZ](https://www.reddit.com/r/gamedev/comments/oksnt9/does_anyone_know_any_good_resources_that_explains/h5bgwmc/)



> There's a pretty big issue with this area of game design since the name varies between games, Valve came up with ai directors for Left 4 Dead 2 but their implemenation was more complex and it effects more aspects of enemy behaviour than just when do they attack. 
> 
> If you're interested in ai directors the best video to begin with would be [this](https://www.youtube.com/watch?v=Mnt5zxb8W0Y&ab_channel=AIandGames).
> 
> If you're more interested in how different games manage enemies during combat than these 3 Gamasutra articles can help you ([Intelligent Brawling](https://www.gamasutra.com/view/feature/3931/intelligent_brawling.php), [Enemy Design and Enemy AI for Melee Combat Systems](https://www.gamasutra.com/blogs/BartVossen/20150504/242543/Enemy_design_and_enemy_AI_for_melee_combat_systems.php), [The Discomfort Zone: The Hidden Potential Of Valve's AI Director](https://www.gamasutra.com/blogs/BenServiss/20130207/186193/The_Discomfort_Zone_The_Hidden_Potential_of_Valves_AI_Director.php)). 
> 
> [This](https://www.youtube.com/watch?v=9bbhJi0NBkk&ab_channel=GameMaker%27sToolkit) video sort of skirts around the idea of but it's a pretty great talk about AI in general.
> 
> The [GDC talk about Doom 2016](https://www.youtube.com/watch?v=2KQNpQD8Ayo&ab_channel=GDC) mentions their version of the system but it's more focused on combat in general. 
> 
> And I remember reading an article from Eurogame.net about Hellblade which had some screenshots from the debugging version of their combat and it mentioned their token based implementation but I can't find it.

-- [DareCZ](https://www.reddit.com/r/gamedev/comments/oksnt9/does_anyone_know_any_good_resources_that_explains/h5diox1/)
