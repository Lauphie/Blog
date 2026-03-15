+++
date = '2025-11-20T11:24:14+01:00'
draft = false
title = 'Interaction in Virtual and Augmented Reality 2025/26'
+++

Implementing an interaction and locomotion project.

# Skateboard XR

## 1 Introduction & Motivation

### Introduction
This blog is a documentation for an project in the lecture Interaction in Virtual and Augmented Reality 2025/26.
In the lecture we learned a lot about VR/AR/XR and what you can do to make pleasing projects in this field.
To get a grade we were requiered to develop and implement a new locomotion technique and some kind of an interaction Task.
For this we got an project with a map and an easy example Solution to have a little reference and see what we have to do.

todo Bild

The map shows a little city with an parkour that has to be completed with the new locomotion technique.
The parkour follows a street on with are coins placed that you can collect on your run. The parkour contains a starting position and twoo checkpoints and an finish line.
When you are about the reach a checkpoint or the finish line you have to do an interaction Task. Some form of manipulating the position and rotation of a given, moveable T-Shape to fit inside an target T-Shape.

todo Bild

The parkour tracks some statistics during a run. It tracks time, amount of collected coins and the accuracy of the interaction task.

This blog is the documentation for my project. The structure is similar to a research paper. We have a problem statement, a solution, implementation details, an evaluation and a conclusion.

### Motivation

A virtual or even augmented reality is an interesting field and opens a lot of new approaches for games or other applications. It motivates you to use your whole body while playing. And if you are using your whole body already, why not make an extended controller? Use some external sensors or just mount the controllers of an head mounted display (HMD) on to something and make an interessting controller. This can increase the realism of your gameplay, especially when your making a kind of an simulation. This is what inspired and motivated me to to this project. And you can see exactly those things in my implementation. 
 


## 2 Problem Statement

I have to design and implement a new locomotion technique to absolve a parcour and complete some kind of an interaction task with object manipulation in VR.
The given parcour and T-Shape manipulation are used, but the locomotion and interaction technique are newly implemented.
For this my goal is to design an extended controller, holding at least one controller not in the hand and mounting it on something else which has to be interacted in the real life with instead.
For this the extended controller has to be implemented in Unity to create an locomotion technique that is a kind of an simulation and feels realisitc and fun.
The interaction task should also mainly be using the extended controller.
Feeling safe during the usage of the locomotion and interaction technique is also a goal which should have a high priority. I want adaptions to minimize the possibility of hurting yourself where they are needed.

#### Goals:

- using the existing project and implementing new techniques for it
- making a creative, extended controller
- making a technique that feels realistic
- making a technique that is fun to use
- making the technique safe and minimize the possibility of hurting yourself  

## 3 Solution
(Include early versions, sketches, and the final implementation)
## 4 Implementation Details
(Focus on complex challenges you solved, not basic steps)
## 5 Evaluation
(Explain your method, participants, and results)
## 6 Conclusion
(Did your system solve the problem? What did you learn?)
## 7 Video
(Record yourself using your techniques and completing the parkour at least once)

Here can you see what it looks like to complete a run of on the given parkour with the here explained Skateboard XR locomotion technique and the implemented interaction method.


{{< youtube IhXeS8tZ1xg >}}

## 8 AI
If you used ai, we would like you to write one post about that as well. If you don't feel confident to post it on your website, please just send me and Jan an email containing that section. There will be no disadvantages for using ai. If you want, add parts of your chats. (Benefits, problems, did it work well?)


{{< img src="images/controlls.png" alt="Mein Bild" >}}


Hier ist mein C#-Beispiel:

```csharp
public class Player
{
    public string Name { get; set; }

    public Player(string name)
    {
        Name = name;
    }
}