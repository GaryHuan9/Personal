---
categories: rage
---

# Introduction
Welcome to the fifth and final blog post of Project Rage! As the semester is coming to an end, we are executing the last stages of development for this game.

# Bug Fixes
*2 hours (unlisted on Jira) + 0.5 hour*

As we are slowly closing in towards the project deadline, everybody in the studio started focusing on polishing the game. In the past two weeks, I fixed a lot of bugs involving many different game systems. Some of the bugs are relatively simple, such as the `NullReferenceException`s reminding us that certain variables were not assigned properly. There were also issues that were not necessarily bugs, but still required significant time to resolve. For example, the level complete screen that I made a few weeks ago had been slightly modified by other members to add an animation to the score value display. Rather than simply displaying the final score, the new screen shows a number ticking up until it hits the actual value, giving the player a moment of anticipation. However, the old implementation is not particularly clean (to say the least...). As per usual, I did some refactoring using a custom math function adapted from my [CodeHelpers](https://github.com/GaryHuan9/CodeHelpers) to recreate a more robust version of the gentle animation. 

| ![code]({{"/assets/images/posts/2022-04-17-the-finale/code.png" | relative_url}}) |
| :----------------------------------------------------------: |
|                *Custom smooth math function*                 |

While I was improving the animation, I found a problem with the score system. Because our second level is comprised of two Unity scenes, one scene for the city and another for the boss fight, the player's statistics are all reset to default when the scene transition happens. These statistic data are critical to the system's calculation of individual player's final scores, so I made sure to persist them using a static variable when we unload the city scene and restores the data when the boss scene is loaded.

# Camera Changes
## Border Redefinition
*2.5 hours*

The `Border` property on `CameraController.cs` is a nullable `Bounds2D?` that controls whether the camera should be restricted by a border. It is mostly used by the scene to indicate where encounters will occur and how the camera should be bounded to limit the player's ability to run away from encounters. Originally, this property is defined by the center of the camera, meaning that it only forces the camera's center to not go beyond the assigned `Bounds2D`. But as development continued, we noticed that although this is easy to achieve with code, it is often difficult to use when designers adjust them in the Unity editor inspector. Thus, I changed the property to bound the edge of the camera, rather than only the center. One caveat to consider is the desired behavior when the `Border` assigned is smaller than the camera itself. I decided that it would be the most convenient to use if the camera just stayed in place, which is what I ultimately implemented.

| ![camera]({{"/assets/images/posts/2022-04-17-the-finale/bounds.png" | relative_url}}) |
| :----------------------------------------------------------------: |
|                       *Camera bounds gizmos*                       |

## Snapping Issue
*2.5 hours (unlisted on Jira)*

While play testing the game, we noticed that the camera occasionally suddenly change its position when triggering an encounter. Since I have been working on the camera for basically the entirety of the development, I picked up this bug and began working on it. I believe it was caused by the `Border` property forcing the camera forward when an counter is triggered. Testing with the debugger confirmed my hypothesis and I decided to change the behavior of the `Border` property once again. Rather than essentially directly assigning the position of the camera, the `Border` now only disallow the camera to move away from the direction of the bounds. With this modification was finally added, instead of immediately snapping forward, the camera smoothly advanced, solving the issue.

# Heavy Enemy Improvements
*2.5 hours*

There was a bug regarding the heavy enemies and their movements. When they roam before attacking the player, they might randomly travel outside of the viewable region of the screen, meaning the player can no longer see them. Once I spent some time understanding the code for controlling the heavy enemies and located where this issue stemmed from, I was able to easily fix it. While I was there, I also patched another problem with the heavy enemies' attack sequence. When one heavy enemy decides to attack, it first tries to move near the player, then pauses to prime its charge, and finally dashes to cause as much damage as possible. Previous iterations of this behavior did not worry about the Z distance of between the heavy enemy and the player, thus their attacks often missed because of the large difference in depth. After some modifications to the code, the heavy enemies now only prime its dash when their Z depth is similar to the player's, making the attacks much more effective.

| ![heavy]({{"/assets/images/posts/2022-04-17-the-finale/heavy.png" | relative_url}}) |
| :---------------------------------------------------------------: |
|                     *The heavy enemy dashing*                     |

# Meetings & Logistics & Play Testing
*8 hours*

Since these are the final few Studio meetings for this semester, they are also getting more and more intense. First, we selected *Twin Blades' Vengeance* to be the official name of this game. We then talked about progress, scope, deployment, bugs, and many other things that are related to the final steps for a completed game. I even went to a few play testing sessions to better understand the different aspects of the game that required polishing. These sessions were much more interesting than how I imagined they would be. All the suggestions made by the testers were really helpful; they noted issues, suggested improvements, and proposed potential fixes. Furthermore, watching other people play the game that I have participated in developing felt rewarding and satisfying. 

# Conclusion
That is it! We have reached the conclusion of this series of blogs and the end of this Studio project! Working in the Studio this semester has been a remarkable experience. There are several organization and programming logistics that certainly need future improvements, but I have really enjoyed participating in a large team, advancing towards a common goal, and completing a finished final product. Unfortunately, I do not believe my course schedule will allow me time to join the Studio once again next semester. Nevertheless, I will continue to work on my own [projects](https://github.com/GaryHuan9/EchoRenderer) as always, enjoying my journey along the way.

Time breakdown: `2.5 + 2.5 + 2 + 0.5 + 2.5 + 8 = 18`.