---
categories: rage
---

# Introduction
Welcome to Pre-Beta II! There are only a few weeks before the semester is over and the game is completed!

# CEO Boss
## Boss Controller
*2.5 hours*

One of the major goal for this sprint is to make some big progress on the Boss of the game. Named *THE CEO*, the Boss "rules their city with a neodymium fist" and pose as a final challenge for the player. Before I started working on the parts that I was assigned, I reviewed the design document and saw an elaborate node graph that represented all of the interconnected states of the Boss intelligence. Unfortunately, I did not see where it was being realized in the project repository, so I assumed that it was not adopted by previous Boss programmers. Once I thought I had an adequate understanding of how the Boss worked, I began reading through the main `BossController.cs` file that we had for controlling the Boss. With the `enum` named `BossState` and many `switch` statements dotted across the entire file, it was quite clear the controller used a state machine to direct the movement and actions of the Boss. As per usual, after making sure that others were not currently work on the file, I cleaned it up and refactored the controller. 

| ![boss]({{"/assets/images/posts/2022-04-03-bosses-and-fixes/boss.png" | relative_url}}) |
| :-------------------------------------------------------------------: |
|                               *THE CEO*                               |

While I was doing that, I thought about how I approached the intelligences in my personal game. Rather than state machines, I selected to use another system that is commonly seen among recent games, the behavior trees. They outline decisions that can be made by the characters in a clear and efficient tree pattern, allowing designers more control and maintainability. Behavior trees were often described by the various [GDC](https://gdconf.com/) talks I watched to be more beneficial than state machines when creating more intricate and complex virtual minds. I fully understand that we are way too deep into this project to switch to a completely new intelligence system, and that state machines are much simpler to initially construct on the programming side. Nevertheless, I believe the Studio should try to adapt the more comprehensive behavior trees for advanced intelligences in future projects.

| ![trees]({{"/assets/images/posts/2022-04-03-bosses-and-fixes/trees.png" | relative_url}}) |
| :---------------------------------------------------------------------: |
|                    *Behavior tree in my game, REDO*                     |

## Boss Attacks
*2 hours*

The two attacks that I was tasked to implement are the punch combo and the clap. The punch combo is the primary attack manuever of the Boss, during which its two metallic arms wave around and attempt to punch the player. Because the sprite asset only included animation for one punch, but the design documents requested for the swing of two punches (a combo), I rerouted the animator controller of the Boss to replay the animation a second time in a fitting way. Additionally, I also reduced the windup time of the attack before the impact by a bit, since I thought it was too easy for the player to dodge.

With the clap attack, the Boss retracts its two arms back, then suddenly claps them together in an attempt to squash the player if they are within range. Although this attack was supposed to cause AOE (area of effect) damage, our internal attacking code was already prepared for this type of action and did not require much modification. After the attacks were added, I slightly tweaked the probability at which all of them activate to make the Boss encounter more interesting.

| ![clap]({{"/assets/images/posts/2022-04-03-bosses-and-fixes/clap.png" | relative_url}}) |
| :-------------------------------------------------------------------: |
|                          *The Boss clapping*                          |

# Camera
## Not a Number
*1.5 hours*

After the sprint just began, our project lead Nikhil courteously told me that the camera breaks if a transition from one level to another occurs. As I was trying to reproduce this issue, I noticed that the velocity of the camera becomes `float.NaN` anytime when there is an interruption to time. For example, during pause screens or level end screens, time is set to zero to disable the primary update loop of the game. Attaching the debugger and briefly roaming around in the codebase let me realize that the Unity `Mathf.SmoothDamp` method produces `float.NaN` errors when `Time.deltaTime` is zero; a behavior that is different from the custom damping method that I was used to use. Fixing this is as simple as adding an `if` statement to prevent invocation of the `Mathf.SmoothDamp` method if `Time.deltaTime` is zero.

| ![nan]({{"/assets/images/posts/2022-04-03-bosses-and-fixes/nan.png" | relative_url}}) |
| :-----------------------------------------------------------------: |
|               *Invalid velocity on camera controller*               |

## Screen Independent Bounds
*3.5 hours*

Since we restrict the movement of the player with the boundary of the camera, the size of this boundary is essential to gameplay. Previously, we only had an artificial padding around the entire screen which changes with different screen resolutions and aspect ratios. This is certainly inadequate because we do not want players with a larger monitor to be able to go outside of the map while smaller monitor players get restrained from all the available content. Thus, I modified the code around this issue and made the collision boundary to be independent of the monitor resolution.

# Other Bugs
*2.5 hours*

Finally, there were also a bunch of bugs that I found and fixed in this sprint. For example, the Boss's hit color was not the desired white flash, but rather a dull black color. A quick investigation let me to locate an incorrect shader selection. Many other similar bugs like this were also discovered and alleviated.

# Meetings & Logistics
*6 hours*

As per usual, all of the logistics, meetings, and communications actually take quite a big chunk of time. We had a separate group chat with just the Boss programmers and had conversations about different implementation details throughout the weeks. This was really helpful to avoid clustering the main server while still knowing what others have worked on. Finally, I am not sure how much time other members of the Studio spend on these blog posts, but all of the official blog posts that I made took me way more than two hours.

# Conclusion
Here is my time breakdown for this sprint: `2.5 + 2 + 1.5 + 3.5 + 2.5 + 6 = 18`.