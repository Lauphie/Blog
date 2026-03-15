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

{{< img src="images/parkour.png" alt="The parkour" >}}

The map shows a little city with an parkour that has to be completed with the new locomotion technique.
The parkour follows a street on with are coins placed that you can collect on your run. The parkour contains a starting position and twoo checkpoints and an finish line.
When you are about the reach a checkpoint or the finish line you have to do an interaction Task. Some form of manipulating the position and rotation of a given, moveable T-Shape to fit inside an target T-Shape.

{{< img src="images/tshapeTask.png" alt="The interaction Task - T-Shape manipulation" >}}

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

### Method and Setup
The evaluation for this Skateboard VR project consists of three parts and combines some qualitative and quantitative aspects.

First every participant got an introduction to the project an explanation of the controlls and an demonstration of the parcour and its different parts.
I recommended to not use the Drop-In-Boost or the Jump, since they can be a bit dangerous and trigger not always as aspected (for example when the movement is done to slow).

Then everyone had a few trys (not more than 3 where needed) to do a complete run of the parcour.

The parcour tracks time and score on its own. This data has been noted for every participant. This is the first part of the evaluation.

After completing the parcour everyone filled in a little questionnaire.
The questionnaire consists of two parts. The first part consists of eight questions and uses a likert-7 scale to get a precise feedback.
Some of the questions are inspired by the NASA-TLX since this questionaire fits the testing of a new locomotion technique very well. To keep this Survey short I decided to just set on the points of physical and mental demand of the NASA-TLX.

> Ich fand die Steuerung körperlich anstrengend.
> The controls were physically demanding.
> (likert-7)

> Ich fand die Steuerung mental anstrengend.
> The controls were mentally demanding.
> (likert-7)


I added some other questions that are directed more to the VR aspect of this application.

> Ich habe bereits Erfahrung mit VR/AR.
> I already have experience with VR/AR.
> (likert-7)


> Mir wurde dabei unwohl. (Motion Sickness)
> It made me feel unwell. (Motion Sickness)
> (likert-7)


Also I asked more directly about the controlls since the main part of this project was the locomotion technique.

> Ich hatte das Gefühl, die Bewegung gut zu kontrollieren.
> I felt I had good control over the movement.
> (likert-7)

> Die Steuerung fühlte sich natürlich/realistisch an.
> The controls felt natural/realistic.
> (likert-7)


Since the danger of hurting yourself with my implementation of the locomotion technique was a big concern of mine during the development phase, which made me also add extra features, so at least I feel more safe, i had to ask that aswell.

> Ich habe mich während der Nutzung sicher gefühlt.
> I felt safe while using it.
> (likert-7)


And last but not least I wanted to know if it made fun which is quite important for the overall experience.

> Es hat Spaß gemacht.
> It was fun.
> (likert-7)


Add the end the survey contained three free text questions where the participants could write some more spcific answers.
I asked about what felt good, what gave them the biggest problems, and what they would change. 

> Was hat sich gut angefühlt?
> What felt good? 
> (free-text response)

> Was war am schwierigsten?
> What was the most difficult part? 
> (free-text response)

> Was würdest du verändern?
> What would you change? 
> (free-text response)


All questions (likert-7 and free-text response) except the last one about what the participants would change were mandatory.

The particapants where me and four friends, so five people in total. This is not very informative, but it is sufficient for this project related to the lecture. The for the lecture required minimum were the creator and two more participants.

### Results for the first eight questions

{{< img src="images/survey.png" alt="The Survey" >}}

First, the experiences differ, only two out of the five people have realy used VR/AR before.
Three people felt some motion sickness, but it did not depend on prior experience.
The controll over the movement was rated differently, but always good. The natural/realistic feal was rated even better.
The physically demand was rated very low with no one thinking it was physically exhausting. The mentaly demand was rated differently but also rather not so exhausting.
The question of feeling safe was answered very positiv with only my own opinion of thinking that it may be a bit dangereous, but that can be biased because I created it. 
(Thats the reason why the creator should normaly not be a part of this own evaluation. For more openness are here my answers: 6, 5, 5, 5, 2, 3, 3, 7)


### Short summary of the text-based Answers
#### What felt good?
The answers included the steering (left-right) five times (everyone mentioned it). The T-Shape task with lifting up the skateboard was mentioned once (by me). One person liked the Speedboost (Drop-In-Boost) and one person mentioned the immersion in VR.

#### What was difficult?
Sharp turns can be hard to do at first and colliding with objects led to problems. Both was mentioned once.
There where calibration issues and putting the skateboard in the correct position, especially after the T-Shape Task. This was mentioned four times.
It was added that putting the skateboard down after the T-Shape Task unlocks the steering to early, so you might spin while having the skateboard in the hands which leads to motion sickness. 

#### What can be changed?
Adding velocity in spurts to get a more realistic feeling.
The calibration could be better. The steering after the T-Shape Task should not be enabled rigth after, like mentioned in the difficulty answers.
Also the logging in the T-Shape method was critiqued, it should be accepted automatically when the position is good enough.

### Interpretation of the results

Overall the feedback was quite positive. The steering is good and using the locomotion technique is very fun. Some motion sickness were felt but the physicall and mentally demand where low. The participants felt suprisingly safe, even without prior VR experience and even with the HMD on a skateboard on. There are some problems with the calibration, which could be different/better. The unlocking of the steering after the T-Shape Task happens to early which may lead t osome motion sickness and requires calibrating new every time.
Everyone had a fun experience and liked the project.  




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