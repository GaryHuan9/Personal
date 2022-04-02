---
categories: rage
---

# Introduction

# CEO Boss
## Boss Controller
*2.5 hours*

One of the major goal for this sprint is to make some big progress on the Boss of the game. Named *THE CEO*, the Boss "rules their city with a neodymium fist" and pose as a final challenge for the player. Before I started working on the parts of the Boss that I was assigned, I reviewed the design document and saw an elaborate node graph that represents all of the interconnected states of the Boss intelligence. Unfortunately, I did not see where it was being realized in the project repository, so I assumed that it was not adapted by previous Boss programmers. Once I thought I had an adequate understanding of how the Boss worked, I began reading through the main `BossController.cs` file that we had for controlling the Boss. With the `enum` named `BossState` and many `switch` statements dotted across the entire file, it was quite clear the controller used a state machine to direct the movement and actions of the Boss. As per usual, after making sure that others were not currently work on the file, I cleaned it up and refactored the controller. 

While I was doing that, I thought about how I approached the intelligences in my personal game. Rather than a state machine, I selected to use another system that is commonly seen among games, the behavior trees. They outline decisions that can be made by the characters in a clear and efficient way, allowing designers more control and maintainability. Behavior trees were often described by the various [GDC](https://gdconf.com/) talks I watched to be more beneficial than state machines when designing more intricate and complex virtual minds. I fully understand that we are way too deep into this project to switch to a completely new intelligence system, and that state machines are much simpler to initially construct on the programming side. Nevertheless, I believe the Studio should try to adapt the more comprehensive behavior trees for advanced intelligences in future projects.

## Boss Attacks
*2 hours*

# Camera
## Not a Number
*1.5 hours*

## Screen Independent Bounds
*3.5 hours*

# Other Bugs
*2.5 hours*


# Meetings & Logistics
*6 hours*

As per usual, all of the logistics, meetings, and communications actually take quite a big chunk of time. We had a separate group chat with just the Boss programmers and had conversations about different implementation details throughout the weeks. This was really helpful to avoid clustering the main server while still knowing what others have worked on. Finally, I am not sure how much time other members of the Studio spend on these blog posts, but all of the official blog posts that I made took me more than two hours.

# Conclusion
Here is my time breakdown for this sprint: `2.5 + 2 + 1.5 + 3.5 + 2.5 + 6 = 18`.