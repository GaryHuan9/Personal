---
categories: rage
---

# Introduction
We made it past the first sprint (two weeks)! This sprint is only one week long because of spring break (woohoo). Nonetheless, I was still very productive so let's get started with the blog.

# Up Kick
*1.5 hours*

First I took some time to familiarize myself with the code and learn how the existing combat system works. To be honest, the project is quite a mess right now. Although everything works, if the studio wants to attempt anything with a bit of significant scale, this way of coding just will not do it. There is barely any [consistency](https://blog.devgenius.io/why-code-consistency-is-important-9d95bdebcef4) with style, naming, control, or code and file placement, which makes things very difficult to find. And because of the common occurrence of duplicated code, sometimes giant methods can be easily simplified to just several lines. I had a talk with Nikhil about this problem, and I understood that this is because everyone on the team is from different backgrounds with different levels of experience. Nevertheless, this is really not how a professional studio should work. Next semester we really need to come up with a series of tutorials and styling guides to ensure that team members learn the most proper ways of employing solutions in our development environment.

I worked with the existing system and incorporated up kick fairly easily. The code just detects if the player is currently airborne and have an upward velocity, then we perform an up kick rather than a regular punch. Similarly I also added down kick, but later on I realized that down kick has some significant differences with up kick, and required some major rewrites.

# Animation Correction
*1 hour (unlisted on Jira)*

While when I was having a conversation with Grace (from our art department) on Discord, she mentioned how our current idle and running animation looks a bit strange. After some investigation, I noticed that the animation with two exchanging sprites is seven frame long, meaning that one sprite is displayed one frame longer than the other.

| ![uneven]({{"/assets/images/posts/2022-03-06-rewriting-code/uneven-frames.png" | relative_url}}) |
| :----------------------------------------------------------------------------: |
|  *Unity animation window showing an animation with a length of seven frames*   |

I remembered that when I import animation in my project (which is 3D), all the animations end exactly on the last key, not on the frame that is one after the last key. This behavior is rather strange, however I was able to create a solution after a few bits of twiddling. 

| ![even]({{"/assets/images/posts/2022-03-06-rewriting-code/even-frames.png" | relative_url}}) |
| :------------------------------------------------------------------------: |
|        *Now the animation is showing both sprites for three frames*        |

Now the two sprites that we have for idling are both displayed for three frames exactly, and it looks much better. However, because we separated upper and lower body movements, we still have to find a way to ensure that their animation are in sync (i.e. blob up and down simultaneously).

# Down Kick
*0.5 hour*

Originally I thought a down kick was the same as an up kick, with the only difference being that the player has to be actively moving down rather than up while kicking. So I also implemented it, but since the art assets were missing, I could not really test them out.

Later on our design lead Crystal reminded me that in order for the down kick to activate, the player must explicitly press down the down and attack keys at the same time. However, currently the down key still controls the player's movement while airborne, which means there is a key conflict between down kick and moving in the 'depth' axis. 

Furthermore, Crystal said that the player should not be able to control their movement after they have left the ground, so we have to switch to a momentum based jumping system. The player must first accelerates to a certain velocity, then jump (adding a vertical velocity), and during which the original horizontal velocity is kept constant.

# Rewrite
*3 hours*

As I have said in a previous section, our current code based only barely supports exactly what it supports originally, and new features are 'patched' onto the existing ones. In order to switch to this new momentum based movement system, the entire `CharacterMovement.cs` file has to be completely rewrote. I gave it `CurrentPosition` and `CurrentVelocity` properties to allow for external access. Several files that were previously using the `CharacterMovement` class also had to be modified to resolve their corresponding compiler errors.

With the new system in place, we can easily implement the required features outlined by our design department. Unfortunately, I did not have time this sprint to complete all of them, but everything should be setup nicely for the next sprint!

# Meetings & Logistics
*3 hours*

Because of spring break, we only had one studio meeting this sprint, which was about an hour long. On top of that, writing this blog consumed another two hours out of my time budget. Nevertheless, I think my blog this time is better than the last one, which means improvement!