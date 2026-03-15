+++
date = '2026-03-15T11:24:14+01:00'
draft = false
title = 'Interaction in Virtual and Augmented Reality 2025/26'
+++

Implementing an interaction and locomotion project.

# Skateboard XR

## Introduction & Motivation

### Introduction
This blog post documents a project for the lecture *Interaction in Virtual and Augmented Reality* (2025/26).

In this lecture, we learned a lot about VR, AR, and XR, as well as how to create engaging projects in this field.
To receive a grade, we were required to design and implement a new locomotion technique and some kind of interaction task in VR.
Target platform is a Meta Quest headset. For developing the project, I received a Meta Quest 3 for the period of this lecture.

For this purpose, we were given a project containing a map and a simple example solution to serve as a reference and help us understand what we had to do.

{{< img src="images/parkour.png" alt="The parkour" >}}

The map shows a small city with a parkour that has to be completed using the new locomotion technique.

The parkour follows a street along which coins are placed that can be collected during the run. It contains a starting position, two checkpoints, and a finish line.

Whenever you reach a checkpoint or the finish line, you have to complete an interaction task. This task involves manipulating the position and rotation of a movable T-shape so that it fits inside a target T-shape.

{{< img src="images/tshapeTask.png" alt="The interaction task - T-shape manipulation" >}}

The parkour tracks several statistics during a run, including the time, the number of collected coins, and the accuracy of the interaction task.

This blog post serves as the documentation for my project. Its structure is similar to that of a research paper: it includes a problem statement, a proposed solution, implementation details, an evaluation, and a conclusion.

 

### Motivation

Virtual and augmented reality are fascinating fields that open up many new possibilities for games and other applications. They encourage players to use their whole body while interacting with a system.

If the whole body is already involved, why not extend the controller as well? External sensors can be used, or the controllers of a head-mounted display (HMD) can be attached to another object to create a more interesting input device.

This can increase the realism of the gameplay, especially in simulation-like applications.

These ideas inspired and motivated me to work on this project, and they are clearly reflected in my implementation.



## Problem Statement

I have to design and implement a new locomotion technique to complete a parkour and design an interaction task involving object manipulation in VR.

The given parkour and T-shape manipulation task are used, but the locomotion and interaction techniques are newly implemented.

My goal is to design an extended controller by holding at least one controller not directly in the hand, but instead mounting it onto another object that has to be used in real life.
This extended controller then has to be implemented in Unity to create a locomotion technique that resembles a simulation and feels realistic and fun.

The interaction task should also mainly be performed using the extended controller.

Ensuring safety during the use of the locomotion and interaction technique is also an important goal and should have high priority. I want to make adaptations wherever necessary to minimize the risk of injury.

#### Goals:

- use the existing project and implement new techniques for it
- create a creative extended controller
- develop a technique that feels realistic
- develop a technique that is fun to use
- make the technique safe and minimize the risk of injury



## Solution and Implementation Details

(This project was developed in Unity, version 6000.0.60f1. The code is written in C#. Most of the code for my implementation is contained in the `LocomotionTechnique` script. I made minor changes to other scripts and created a few additional ones.)

My project idea had to be something exciting. I did not want to simply use the controllers in the usual way. From the very beginning, my goal was to mount at least one of them onto an object in order to create a unique controller that is not just held in the hand. The real controller should only be used to track positions and rotations.

My first serious idea, inspired by my own interests, was to mount a controller onto a frisbee and use a throwing motion, or even an actual throw (although I have no idea how I could have made that safe for the hardware), to track and simulate the flight. Another variation of this idea was to keep holding the frisbee in the hand and control the flight path by moving and tilting it. However, this would have resulted in a teleportation technique, where the locomotion consists of teleporting to the landing point of the frisbee, which was not the kind of technique I wanted to use.

I still wanted to implement some kind of sport so that users could have fun in a way they enjoy, even during the winter months, when going outside is not always enjoyable or even possible for certain sports.

So, the idea I ultimately chose became a simulation: a simulation of something that requires the use of the whole body and an implementation that is quite unique as an input method for a game.

My final idea was to simulate riding through the streets on a skateboard by using a real skateboard.



### Steering

The most important part of my project is the steering.
To test this, I created a small initial prototype with a constant speed, where steering was done by tilting the left controller to the left or right.
I attached the controller to my skateboard and tested whether standing on the skateboard and steering in a realistic way would work well enough.
Once I saw that this approach worked well, I decided to pursue the skateboard idea further.

The core idea behind the steering remained the same in the final project.


{{< youtube lIeEEDyCFQc >}}

https://youtu.be/lIeEEDyCFQc

Computing the Steering:
```csharp
    private float ComputeSteerFromRollZ(Quaternion currentLocalRot)
    {
        Quaternion rel = Quaternion.Inverse(steerNeutralLocalRot) * currentLocalRot;
        float roll = NormalizeAngle(rel.eulerAngles.z);

        float raw = Mathf.Clamp(roll / Mathf.Max(0.01f, maxLeanDegrees), -1f, 1f);

        float abs = Mathf.Abs(raw);
        if (abs < steerDeadzone) return 0f;

        float sign = Mathf.Sign(raw);
        float t = (abs - steerDeadzone) / Mathf.Max(0.0001f, (1f - steerDeadzone));
        return sign * Mathf.Clamp01(t);
    }
```

### Modifying the Skateboard

Using a skateboard as an extended controller indoors required some modifications.

First, I did not want it to roll in real life; it had to remain stationary. I also did not want to damage the floor in my room. In addition, I needed to mount the controller onto the skateboard in a reasonable and functional way. Everything also had to be inexpensive.

I ended up removing the wheels from the skateboard and replacing them with tennis balls. This did not cost much, because I already had some old tennis balls. The skateboard no longer rolls, does not damage the floor, and can still be tilted normally. Other ideas would not have achieved all of these goals at the same time.

For my first prototype, I placed the controller in an old plastic box and secured it lightly with materials I already had at home.

Modified-Skateboard Prototype:
{{< img src="images/umbauproto.png" alt="Modified-Skateboard Prototype" >}}

I did not change much about this setup for the final version of the skateboard. I used some tape and cardboard to secure the tennis-ball wheels more firmly and to further protect the floor. I also taped the plastic box tightly to the skateboard and added some foam inside it to stabilize and protect the controller when it was placed inside. The controller itself had to be secured in the box with a piece of tape.

Other ideas would have been too expensive or would have required too much time, so I decided that this aspect was not important enough for this project, since it was not directly related to the lecture and I would not be using it in the future. One alternative I considered was designing and 3D-printing a custom case and mounting mechanism for attaching the controller to the skateboard.

Modified-Skateboard now:
{{< img src="images/skate.png" alt="Modified-Skateboard" >}}


### Acceleration and Braking
Another major design challenge, for which I explored many different concepts, was the question of how to implement acceleration.

At first, I wanted the acceleration to work in the same way as on a real skateboard. My idea was to reproduce a real pushing movement, similar to the motion used on an actual skateboard, but without applying the pressure and force to the ground that would normally move the rider forward. Since I had replaced the skateboard wheels with tennis balls, the board remained stationary, although with enough force it could still shift slightly.

The first problem with this approach was determining how to track the leg movement. I investigated whether foot and leg tracking would be possible in Unity with the hardware available to me. I also considered how to attach the controller that was not mounted on the skateboard to the leg without simply wrapping tape around it every time. I eventually came up with a solution involving two elastic straps (belts), one small and one large, as well as a carabiner hook.

The equipment:
{{< img src="images/belt.png" alt="Stuff" >}}


The larger strap was placed around the waist, with the carabiner attached to it. The wrist strap of the controller was then hooked onto the carabiner, and the controller itself was positioned on the leg. After that, the smaller strap was wrapped around the leg to secure the controller in place. This setup worked surprisingly well, even during larger movements.


The applied equipment:
{{< img src="images/beltfront.png" alt="The applied equipment - front" >}}
{{< img src="images/beltleft.png" alt="The applied equipment - side" >}}


However, before I implemented this acceleration method, I discovered another major problem with the technique. Unfortunately, this method required a swinging leg movement, which made balancing much more difficult for the user. During my first prototype tests, I had already learned that maintaining balance on a tilting skateboard while wearing a head-mounted display was far more difficult than expected, even when the passthrough view showing the real feet and skateboard was enabled.

Swinging one leg and repeatedly changing its position between the ground and the skateboard introduced additional physical movement, which greatly increased the difficulty of maintaining balance. However, balance alone was not the only challenge. The user also had to control the tilt angles of the skateboard precisely in order to steer and stay on the virtual street. Because of the increased danger and the more complicated controls, I had to discard this idea.

I also considered other concepts, such as arm-swinging movements similar to those used in skiing, but I was not satisfied with them.

My final solution was to simply use the right controller in the hand. The primary trigger is used to accelerate. Although this removes the feeling of physically pushing off the ground, it is somewhat similar to acceleration on an electric longboard or skateboard. The trigger can be pressed to different degrees in order to control the strength of the acceleration.

The skateboard slows down on its own, similar to how real friction would reduce speed. However, the user can also brake manually by using the secondary trigger.


```csharp

...

// Teile velocity in planar + vertical 
Vector3 vel = rb.linearVelocity;
Vector3 velPlanar = Vector3.ProjectOnPlane(vel, Vector3.up);
Vector3 velVertical = vel - velPlanar;

float newSpeed = velPlanar.magnitude;

// Beschleunigen mit primary trigger
float trig = Mathf.Clamp01(OVRInput.Get(OVRInput.RawAxis1D.RIndexTrigger));
bool throttleOn = trig > 0.001f;

if (throttleOn)
{
    float shaped = Mathf.Pow(trig, throttleExponent);
    float targetSpeed = Mathf.Lerp(minSpeedWhenPressed, maxSpeed, shaped);
    
    if (targetSpeed > newSpeed)
        newSpeed = Mathf.MoveTowards(newSpeed, targetSpeed, accel * dt);
}
else
{
    // Trigger losgelassen -> nur Reibung / Ausrollen
    newSpeed *= Mathf.Exp(-rollingDamping * dt);
}

// Drop in boost
if (pendingDropBoost > 0f)
{
    newSpeed += pendingDropBoost;
    pendingDropBoost = 0f;
}


// Bremsen mit secondary trigger
float gripRaw = Mathf.Clamp01(OVRInput.Get(OVRInput.RawAxis1D.RHandTrigger));
float grip = Mathf.InverseLerp(brakeDeadzone, 1f, gripRaw);
grip = Mathf.Clamp01(grip);

if (grip > 0f)
{
    float brakeFactor = Mathf.Pow(grip, brakeExponent);
    float brakeDecel = brakeFactor * maxBrakeDecel;
    newSpeed = Mathf.MoveTowards(newSpeed, 0f, brakeDecel * dt);
}

newSpeed = Mathf.Max(0f, newSpeed);


if (pendingJumpUp > 0f)
{
    velVertical.y = Mathf.Max(velVertical.y, pendingJumpUp);
    pendingJumpUp = 0f;
}


rb.linearVelocity = velVertical + forward * newSpeed;

...

```


### AR - A Window to the Real World

This project benefits from the passthrough capability of the Meta Quest 3. The possibility of adding a window to the virtual world through which the real world can be seen is especially useful when there is a potential risk of injury while using the application.

Because my project involves the use of a real skateboard, maintaining balance is very important. However, balancing on a skateboard is already not easy, and wearing a head-mounted display (HMD), which blocks the view of the real world, makes it even more difficult. This further increases the potential risk of injury. For this reason, I added a portal-like window beneath the player that shows the real world.


Real world window concept art:
{{< img src="images/ar_konzept.png" alt="Real world window concept art" >}}


The idea was that balancing would become easier if the user could see their own feet and the skateboard. After implementing it, however, I found that this did not help as much as expected. Normally, people do not look at their feet very often, and even when they do, standing on a skateboard while wearing a headset does not feel exactly the same as without one, even with passthrough enabled. I still decided to keep this feature in the project, because it does not disturb the feeling of virtual reality too much. In addition, even if it does not significantly improve balance, it helps in moments when the user might actually fall, and overall it made the experience feel safer. Also it is somehow useful for the object interaction task that is explained later in this blog post.

In-Game view:
{{< img src="images/ar_impl.png" alt="In-Game view" >}}


To implement this feature, I used a building block from the Meta XR Tools. I also wrote a short script so that it behaved exactly as I wanted: it always follows the position of the HMD and remains beneath the player.

The passthrough window in Unity:
{{< img src="images/ar_unity.png" alt="The passthrough window in Unity" >}}


Holding onto something while standing on the skateboard with the head-mounted display on is still strongly recommended.


### Object Interaction Task

My approach to the T-shape interaction task was relatively simple.

There is one movable T-shape object that has to be fitted as accurately as possible into another T-shape that is static and cannot be moved. After that, the offset between the two shapes is calculated, which determines the score for how well the task was completed. This functionality was already provided in the given project.

For my implementation, I mirrored the movable T-shape onto the controller that is mounted on the skateboard and mapped its position relative to the controller so that the T-shape is aligned with the skateboard. With this setup, the T-shape in VR corresponds to the skateboard in the real world. The goal of this implementation is to pick up the real skateboard from the ground and hold it in the air so that its position and rotation match those of the target T-shape. For picking up the skateboard, the passthrough window beneath the player is quite useful and makes it easier.

The position and rotation are confirmed by pressing a button on the controller that is not mounted on the skateboard. The task spawns and starts automatically when the corresponding area is entered before reaching each checkpoint.


{{< youtube KYWbXxtEOxs >}}

https://youtu.be/KYWbXxtEOxs


For the T-shape object interaction task, I used three scripts. I modified the existing `SelectionTaskMeasure` script and added one script for the left controller and one for the right controller specifically for this task.

The script for the right controller only contains the button input that is used to confirm the task result. The script for the left controller handles attaching the movable T-shape to the left controller so that the T-shape in VR moves according to the skateboard in real life.

The most important part is implemented in the `SelectionTaskMeasure` script. I changed the task so that it starts when the player enters a collider, meaning that it no longer has to be started manually. For this, I also needed to change the spawn point of the T-shape. When the collider is entered, the locomotion stops and becomes locked, the target T-shape spawns, and the movable T-shape is attached to the left controller so that it mirrors the real skateboard in VR.


```csharp
public void StartOneTask()
    {
        if (isTaskStart || isCountdown) return;
        
        isTaskStart = true;
        isTaskEnd = false;
        
        if (targetT != null) Destroy(targetT);
        if (objectT != null)
        {
            if (attacher != null) attacher.Detach(objectT);
            Destroy(objectT);
        }
        
        // Locomotion sperren
        if (parkourCounter != null && parkourCounter.locomotionTech != null)
            parkourCounter.locomotionTech.SetLocomotionLocked(true);
        
        taskTime = 0f;
        taskStartPanel.SetActive(false);
        donePanel.SetActive(true);
        
        // Spawn target vor dem Spieler in Reichweite
        Vector3 flatForward = Vector3.ProjectOnPlane(hmdTransform.forward, Vector3.up).normalized;
        if (flatForward.sqrMagnitude < 0.001f) flatForward = Vector3.forward;

        Vector3 right = Vector3.Cross(Vector3.up, flatForward).normalized;
        Vector3 origin = hmdTransform.position + Vector3.up * targetHeightFromEye;
        Vector3 targetPos = origin + flatForward * targetDistance + right * targetSide;

        targetT = Instantiate(targetTPrefab, targetPos, Random.rotation);
        
        // Attach T-Shape an Controller
        objectT = Instantiate(objectTPrefab, attacher.leftAttachPoint.position, attacher.leftAttachPoint.rotation);
        attacher.Attach(objectT);
    }
```

### Drop-In Boost and Jump

The last feature I added as a bonus was a boost that simulates dropping in on a ramp with a skateboard, as well as a jump mechanic.

To perform a drop-in, you shift your body weight to the tail, the back part of the skateboard, so that the nose, the front part, lifts up. Then you shift your weight back toward the middle or front, creating a rapid movement that brings the nose back down. Real skateboarders use this movement on ramps to drop in with a lot of speed. In my project, I use this movement to provide a speed boost on flat ground.

Drop-In-Boost:
{{< youtube 18q2Tj87-ig >}}
https://youtu.be/18q2Tj87-ig

The jump is implemented the same way. To jump, the user also has to move the nose up and down quickly. The difference lies in the current speed: if the skateboard is stationary or moving very slowly, this movement results in a boost. If the skateboard is already moving, it triggers a jump instead.

Jump:
{{< youtube TSjUHZE-M6s >}}
https://youtu.be/TSjUHZE-M6s

I implemented this behavior using a small state machine that detects when the nose, the front part of the skateboard, is raised.


```csharp
// Tail Drop und Jump detecten
if (hasNeutral  && Time.time >= ignoreTailUntil)
{
    float pitch = GetTailPitchDeg(currentLocalRot);
    if (invertTailPitch) pitch = -pitch;

    if (!hasPrevTailPitch)
    {
        prevTailPitchDeg = pitch;
        hasPrevTailPitch = true;
    }

    float pitchRate = (pitch - prevTailPitchDeg) / dt;
    prevTailPitchDeg = pitch;

    // aktueller Fahr-Speed
    float planarSpeed = Vector3.ProjectOnPlane(rb.linearVelocity, Vector3.up).magnitude;

    // Steering deaktivieren solange Nose hoch/Bewegung aktiv
    if (Mathf.Abs(pitch) >= steerLockAngleDeg)
        steerLockedUntil = Mathf.Max(steerLockedUntil, Time.time + steerLockHoldTime);

    bool cooldownOk = (Time.time - lastTailActionTime) >= tailCooldown;

    // Arm: nur wenn Nose hoch genug
    if (!tailArmed && cooldownOk && pitch >= tailLiftAngleDeg)
    {
        tailArmed = true;
    }

    // Trigger: wenn armed + Nose runter + schnell genug
    if (tailArmed)
    {
        bool released = pitch <= tailReleaseAngleDeg;
        bool droppingFast = pitchRate <= -tailMinDownRateDegPerSec;

        if (released && droppingFast)
        {
            if (planarSpeed <= planarSpeedBorder)
                pendingDropBoost += dropBoostDeltaV;     // Drop-In (planar)
            else
                pendingJumpUp += jumpUpDeltaV;          // Jump (vertical)

            lastTailActionTime = Time.time;
            tailArmed = false;

            // kurz Steering locken nach dem Snap
            steerLockedUntil = Mathf.Max(steerLockedUntil, Time.time + steerLockHoldTime);
        }

        // disarm
        if (pitch < 0.5f && Mathf.Abs(pitchRate) < 20f)
            tailArmed = false;
    }
    
}
```

At the moment, both features are still somewhat unreliable and can also cause calibration problems in some cases.


## Evaluation

### Method and Setup
The evaluation of this *Skateboard VR* project consists of three parts and combines both qualitative and quantitative aspects.

First, every participant received an introduction to the project, an explanation of the controls, and a demonstration of the parkour course and its different elements. I recommended not using the drop-in boost or the jump, since these features can be somewhat dangerous and do not always trigger as intended, for example when the movement is performed too slowly. I also reduced the maximum speed beforehand, since it improves the usability for inexperienced participants a lot.

After that, each participant had a few attempts to complete the course. No one needed more than three attempts to finish a full run. Sometimes they got stuck or fell of the map in the beginning.
After getting used to the locomotion technique, everything went well.

The course automatically tracks the completion time, collected coins during the locomotion parts and the score of the interaction tasks. This data was recorded for every participant and formed the first part of the evaluation.

After completing the course, each participant filled out a short questionnaire.

The questionnaire consisted of two parts. The first part contained eight questions and used a 7-point Likert scale in order to obtain more precise feedback. Some of the questions were inspired by the NASA-TLX, since this questionnaire is well suited for evaluating a new locomotion technique. To keep the survey short, I decided to focus only on the aspects of physical and mental demand from the NASA-TLX.

> Ich fand die Steuerung körperlich anstrengend.
> The controls were physically demanding.
> (likert-7)

> Ich fand die Steuerung mental anstrengend.
> The controls were mentally demanding.
> (likert-7)


I added some further questions that were more specifically related to the VR aspect of the application.

> Ich habe bereits Erfahrung mit VR/AR.
> I already have experience with VR/AR.
> (likert-7)


> Mir wurde dabei unwohl. (Motion Sickness)
> It made me feel unwell. (Motion Sickness)
> (likert-7)


In addition, I asked more directly about the controls, since the locomotion technique was the central part of this project.

> Ich hatte das Gefühl, die Bewegung gut zu kontrollieren.
> I felt I had good control over the movement.
> (likert-7)

> Die Steuerung fühlte sich natürlich/realistisch an.
> The controls felt natural/realistic.
> (likert-7)


Because the risk of injury was one of my main concerns during the development phase, and also motivated me to add extra safety-related features so that the experience would feel safer, I considered it necessary to include a question about this aspect as well.

> Ich habe mich während der Nutzung sicher gefühlt.
> I felt safe while using it.
> (likert-7)


Last but not least, I wanted to know whether the experience was fun, since this is an important part of the overall user experience.

> Es hat Spaß gemacht.
> It was fun.
> (likert-7)


At the end, the survey also contained three free-text questions in which the participants could provide more specific feedback. I asked what felt good, what caused the biggest problems, and what they would change.

> Was hat sich gut angefühlt?
> What felt good? 
> (free-text response)

> Was war am schwierigsten?
> What was the most difficult part? 
> (free-text response)

> Was würdest du verändern?
> What would you change? 
> (free-text response)


All questions, both the Likert-scale items and the free-text responses, were mandatory except for the final question about what the participants would change.

The participants consisted of myself and four friends, resulting in a total of five participants. Although this sample size is not highly informative, it is sufficient for this lecture project. The minimum requirement for the lecture was the creator plus two additional participants.

### Results for the First Eight Questions

{{< img src="images/survey.png" alt="The Survey" >}}

First, the participants’ prior experience differed: only two of the five people had previously used VR or AR to a significant extent.

Three participants experienced some motion sickness, but this did not appear to depend on prior experience.

The level of control over movement was rated differently by the participants, but always positively. The natural and realistic feeling of the locomotion technique was rated even better.

The physical demand was rated as very low, and no participant considered the experience physically exhausting. The mental demand was rated more differently, but overall it was also considered rather low.

The question of feeling safe was answered very positively. The only more critical response was my own, since I considered the technique to be somewhat dangerous. However, this may be biased because I created the project.
(This is also the reason why the creator should normally not be part of their own evaluation. For the sake of transparency, these were my answers: 6, 5, 5, 5, 2, 3, 3, 7.)


### Short Summary of the Text-Based Answers

#### What felt good?
The answers mentioned the steering (left and right) five times, meaning that every participant referred to it. The T-shape task involving lifting the skateboard was mentioned once, by me. One person liked the speed boost (drop-in boost), and one person mentioned the immersion in VR.

#### What was difficult?
Sharp turns were initially difficult to perform, and collisions with objects also caused problems. Both of these points were mentioned once.

Calibration issues and placing the skateboard in the correct position, especially after the T-shape task, were mentioned four times.

It was also noted that putting the skateboard down after the T-shape task unlocks the steering too early. As a result, the player may start spinning while still holding the skateboard in their hands, which can lead to motion sickness.

#### What can be changed?
One suggestion was to add acceleration in bursts to create a more realistic feeling. (Tapping the acceleration trigger instead of holding it already gives this effect.)

The calibration could also be improved. In addition, the steering after the T-shape task should not be re-enabled immediately, as already mentioned in the answers about difficulties.

The confirmation process in the T-shape task was also criticized. It was suggested that the result should be accepted automatically once the position is accurate enough. (I used the already given method which does not do this and therefore did not implement this idea beforehand even though I thought about it aswell.)


### Statistics of the Parkour

{{< img src="images/stats.png" alt="The Survey" >}}

The parkour statistics for each run look quite good. Everyone had a few attempts to become familiar with the controls. I only recorded the final or best score for each participant.

Everyone managed to complete the parkour in under 100 seconds. Most participants were not very interested in collecting all of the coins and instead wanted to enjoy the feeling of virtual skateboarding. The interaction task also went well. My own statistics are shown on the right side. My time is the fastest because of the experience I gained while developing the project.

After getting used to the controls, everyone had fun and was able to achieve similar scores.


### Interpretation of the Results

Overall, the feedback was quite positive and the recorded statistics looked good. The steering works well, and using the locomotion technique is very fun.

Some motion sickness was reported, but the physical and mental demand were both low. The participants felt surprisingly safe, even without prior VR experience and despite standing on a skateboard while wearing a head-mounted display.

There are still some problems with the calibration, which could be improved. In addition, the steering is re-enabled too early after the T-shape task, which may lead to motion sickness due to uncontrollable spinning and requires recalibration afterward.

Overall, everyone had a fun experience and liked the project.


## Conclusion

The implementation of *Skateboard XR* as a new locomotion technique was quite successful. I was able to create an interesting and enjoyable locomotion technique with an unusual control style. The overall feeling is quite similar to riding a real skateboard, although there are still some minor differences. Nevertheless, it felt natural overall.

My concerns about safety turned out to be less critical than expected. The participants generally felt safe, and nobody was injured during either the evaluation or the development phase. However, nobody was able to resist holding onto something to maintain their balance, which shows that some compromises are still necessary. For this reason, I think that not implementing the real leg-swing motion that I had originally planned was a reasonable decision in terms of safety and usability.

There is still room for improvement, especially with regard to reducing motion sickness.

Overall, I am happy with the result and believe that I achieved the goals stated in the problem statement.

I learned a lot about developing VR and AR applications. Some aspects were easier than I had expected, such as implementing a passthrough window using Meta Building Blocks. Other aspects were completely new to me, for example working with controller sensors to track their rotations in Unity.

Implementing this project was a very enjoyable and very different experience.



## Video + Controls
The following video shows my complete run through the given parkour course using the *Skateboard XR* locomotion technique and the implemented interaction method described above.

{{< youtube IhXeS8tZ1xg >}}
https://youtu.be/IhXeS8tZ1xg


The controls are explained below:
{{< img src="images/controlls.png" alt="Controls" >}}


## AI

I used ChatGPT in this project to discuss my ideas and explore different methods for implementing them.

I used it as an alternative to reading documentation and searching the internet for answers.

In addition, I used it to help identify mistakes in my code, and I let it generate code aswell, which I then reviewed and refined myself.

The code generated by ChatGPT was sometimes useful, but it often required additional changes before it worked properly.

In particular, fine-tuning values had to be done manually. However, ChatGPT was able to suggest parameter ranges that were likely to be suitable.

This blog post was written manually, but it was slightly improved with the help of AI, since English is not my first language.







