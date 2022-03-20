---
categories: rage
---

# Introduction
Welcome to the third sprint! Although exams and homework from other classes made me very busy, I am still quite proud of the progress that I have made. 

# Down Kick
*1.5 hours*

The down kick is an attack maneuver feature that has been delayed for three sprints because of other backend issues (I briefly discussed this in the last blog). With the changes to the character movement system done, I can now finally implement down kick. As described by the documentation from the design department, the down kick should activate when the player is airborne during a jump, and they simultaneously trigger the down and attack button.

I created another attack type using the existing attacking system, introduced a new animation trigger, and mapped this new kick to a new set of sprite names. The set of sprites that we intended to use for the down kick was still not available when I completed implementing this feature, hence I just used the regular attack animation as a placeholder. But as of the writing of this blog, the animation has been created and replaced by the art department, so here is a picture of the final result:

| ![down-kick]({{"/assets/images/posts/2022-03-20-big-progress-made/down-kick.png" | relative_url}}) |
| :------------------------------------------------------------------------------: |
|                              *Down kick in action*                               |

# Score System
*2 hours*

The next tasks that I was assigned were related to the level completion screen, along with the score displays and score system. Naturally, I started working on the backend, which is the score system. I added methods that collect the various information such as player hit count and health, process them into storable formats, and calculate a total score at the end of a level. The equation used for the score calculation, which consisted of a series of multiplications, is provided by the design department. Along with the numerical score, the system also generates an overall grade, choosing a letter between S, A, B, C, D, and F. Although this formula work for now, from my testings it was quite difficult to achieve a reasonably decent score and get a grade above F, even if I did a pretty good job at beating the bad guys.

# Score UI
*3.5 hours*

To display all of the information gathered and calculated by the score system, a UI window appears at the end of a level to show the players their performance. As always, the layout of the window is already provided by the design department. I first disabled the now deprecated old level completion window, placed the appropriate texts and labels, and finally decorated the screen with some buttons. I used Unity's `Horizontal Layout Group` component to configure the positioning of the labels, which automatically works for any arbitrary number of players. Rather than having multiple `TextMeshPro - Text` components for each player, I utilized their Rich Text features to allow one component to display a variety of information using different styles. This allows for easier access through code (managing one component vs. seven components) and also improved performance. 

| ![score-ui]({{"/assets/images/posts/2022-03-20-big-progress-made/score-ui.png" | relative_url}}) |
| :----------------------------------------------------------------------------: |
|                     *Score user interface for one player*                      |

# Fixes
## UI `RectTransform`s
*1 hour (unlisted on Jira)*

Before working on the score UI, I have seen Crystal, our design lead, posting in the Discord server about converting all of the `RectTransform`s in our `UI Package` to use anchors. While it seemed strange at first, I immediately understood her reasons when I began browsing our existing UI hierarchy. Although all of the objects are placed at their correct orientation, their `RectTransform` had arbitrary positional values with weird pivot and anchor placements. Any manipulation to the size of a parent object will cause the positioning of its children to go haywire. 

This kind of occurrence has been a common theme since I joined the Studio a couple of months ago. Many of the features or objectives were seemed to have been achieved with a sense of rush. They work today, but tomorrow they probably would not. Hence, I totally agreed with Crystal on redoing all of the handlings of the `RectTransform`s. However, she simply converted all of the position values to the anchors, which make sense for some objects but not others. The position, anchors, and pivot of a `RectTransform` have different purposes, and they should be used in combination with each other. Unity has a fantastic [page](https://docs.unity3d.com/Packages/com.unity.ugui@1.0/manual/UIBasicLayout.html) about how to correctly layout the `RectTransform`s, and I believe anyone who does UI should have a quick read on it.

## Camera Assertion Bug
*3 hours (unlisted on Jira)*

During this sprint, someone in the Discord server (I forgot who :( I am sorry) reported that an assertion in the `CameraController` would fail if we are doing multiplayer and the two players are really far from each other. They said that it might be because the code is trying to create a `Bounds2D` with the min larger than the max, which does not make sense, so an assertion fails. I attached the C# debugger to Unity and noted that is indeed what is causing the error. Usually, finding why a bug occurs is more difficult than resolving it. This statement is especially true for this bug; fixing it took only several minutes. While I was looking around chasing this bug, I also found that whoever wrote the code that assigns the `Border` of the `CameraController` made a minor mistake: since we do not want any vertical camera movement at all, the `Bounds2D` assigned to `Border` should have a height of zero. Rather than annoying the original author, I corrected the code and reassigned the border values in the Unity scene (because the serialized data were lost).

## Console Command System
*1 hour (unlisted on Jira)*

The score UI window is only activated when the player completes an entire level. But as a developer, I do not want to play and fight all of the enemies every time I need to test this window. Nikhil suggested I use the console command system included in the Studio general library. He said I only need to create a class that inherits from `ConsoleCommand`, and it will automatically show up in our developer console to be used for testing. After checking out other existing commands, I implemented the `RaiseLevelCompleteCommand` class that activates everything related to the completion of a level. 

| ![commands]({{"/assets/images/posts/2022-03-20-big-progress-made/commands.png" | relative_url}}) |
| :----------------------------------------------------------------------------: |
|                 *Different commands in the developer console*                  |

I always loved some [reflection](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/reflection) code, so I could not stop myself from peeking at the lines from the console system that searches for every command that exists. I found a medium-large method (20 lines or so) that contained multiple for loops and different if statements. These searching and filtering methods can always be significantly shortened into several lines of elegant [LINQ](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/) code, so that is exactly what I did.

| ![linq]({{"/assets/images/posts/2022-03-20-big-progress-made/linq.png" | relative_url}}) |
| :--------------------------------------------------------------------: |
|                        *Beautiful LINQ method*                         |

# Meetings & Logistics
*6 hours*

As the project becomes larger, our weekly meetings begin to use the full two hours. We have more subjects to talk about, more questions to ask, and more tasks to be assigned. Devon, our studio director, also changed up the format of our meetings to increase the number of interactions between different departments. As always, writing this blog took wayyyy more than the two-hour time-period recommended by Professor Yarger, our faculty instructor, but there are so many topics to be covered!

# Conclusion
Here is my time breakdown for this sprint: `1.5 + 2 + 3.5 + 1 + 3 + 1 + 6 = 18`.