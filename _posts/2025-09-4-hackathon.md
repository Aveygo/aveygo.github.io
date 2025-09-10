---
title: How a dentist helped me win my first AI Hackathon  
date: 2025-09-05
categories: [Machine Learning, Automated Decisions]
tags: [AI, Hackathon]
description: My experience at the 2025 Decision Advantage AI Hackathon hosted by UTS, Chaos1, and Australian Army Battle Lab
---

In case you don't know me, I'm a python dev that loves solving problems, and while making projects and solving problems in your own time can be fun, hackathons take that learning experience and crank it up to 11.

I was fortunate enough to be part of the 2025 Decision Advantage AI Hackathon in Sydney which was hosted by Chaos1 as part of UTS Techfest which allowed me to work in a team and show off my skills.

![Indoor crowd](assets/media/hackathon/crowd.jpg){:style="display:block; margin-left:auto; margin-right:auto; width: 50%; max-height: 600px;"}

This blog post is primarily intended to be an archive of my experience during the two day hackathon but I hope to provide some insight into what I've learned so that it can help anyone else following in my footsteps.

# Timeline

## Day 1

### 10:00: **Enter Sandman**

![Google Earth](assets/media/hackathon/google_earth.jpg){:style="display:block; margin-left:auto; margin-right:auto; width: 50%; max-height: 600px;"}

The day started with a small meet and greet with the other participants. For this hackathon I was fortunate enough to have my group pre-formed from a previous university course and so we had a good idea of each other's skills.

I also met up with Chaos1, who gave us the rundown & a quick safety briefing.

### 10:30: **Mission Brief**

After getting comfortable and setting in, the hackathon immediately started with a challenge statement:

![Presentation slide](assets/media/hackathon/challenge.jpg){:style="display:block; margin-left:auto; margin-right:auto; width: 70%;"}

Now, us programmers are great at getting excited and creating solutions in our head but it's very easy to fall into the trap of creating 'incorrect' solutions, and this was no exception. Immediately I was planning out a list of different technologies & techniques that I've learned over the years - but it's this type of bias that makes it very easy to not see something better.

### 11:00: **Planning**

In hindsight it was very difficult to narrow down on something that we believed wholeheartedly was a winning solution. Looking back at our own experience (as well as what the other groups created) I want to share the following graph that encapsulates the main problem that we all faced and why I wanted to make this blog post in the first place:

![Diagram](assets/media/hackathon/diagram.svg){:style="display:block; margin-left:auto; margin-right:auto; width: 70%;"}

Most groups fell into the pit of creating overly complex solutions, especially by those that wanted to develop their own model. To be fair there is a very strong appeal to designing, training, and deploying one - after all, it's an AI hackathon, it comes with the turf.

Our group also initially fell into this trap but we managed to crawl our way out of it over the course of the next 48 hours.

### 13:00: **The Tenth Man**

At this stage of the hackathon our group narrowed down on several ideas that we thought could help us win.

![Groups at tables](assets/media/hackathon/us_working_labeled.png){:style="display:block; margin-left:auto; margin-right:auto; width: 50%; max-height: 600px;"}

But now we had another problem: which solution is the best? This is where we really had to dig in and find a way to balance risk with reward. Our group ultimately came up with "the 10th dentist" method, inspired by [a certain movie](https://www.youtube.com/watch?v=W_A5j3RuWHM).

We assigned one person in our group to be 'the 10th dentist'. It was this person's job to play devils advocate and **always** give a reason why the solution would not work.

> If all of us look at the same information and arrive to the same conclusion, it is the duty of the tenth man to disagree.

This requires a lot of creativity and technical knowledge as this person *must* find problems even if they are outland-ish or unrealistic. 

![Dentist meme](assets/media/hackathon/dentist_meme.jpg){:style="display:block; margin-left:auto; margin-right:auto; width: 50%; max-height: 600px;"}

This allowed us to foresee and completely avoid some problems that other groups found themselves struggling with:
1. Training a model takes a lot of time and effort
2. Getting GPUs to work properly is a pain (drivers, training time, memory)
3. Design a novel AI pipeline is complex and requires a lot of iterations
4. 50/50 chance of the model giving good & reliable results for a live demo

By critiquing every possible aspect of the last 2 hours of work we ended up trimming our selection down to one idea that none of us could fault: FIRA. 

### 16:30: **Closing Brief**

At this point we've setup our github and had a very rough cli program that we could present on in a worse case scenario. During the closing session we chatted with some other groups about their progress; which was a good way to check if we were on the right track.

Finally, after 7 hours of socializing, thinking, and eating, it was time to spend an all-nighter developing the final production release and making it look presentable.

## Day 2

### 10:00: **Day Start**

After being freshly caffeinated our group immediately reorganized and started testing our program. As a bit of a side quest we also spent this time deploying our project on a JetsonNano.

#### NVIDIA sucks

![JetsonNano](assets/media/hackathon/jetson.jpeg){:style="display:block; margin-left:auto; margin-right:auto; width: 50%;"}

As a tangent to the Hackathon, I want to spend a moment to complain about the JetsonNano. In principle it sounds like it's just a raspberry pi with a gpu, but [poor driver support on NVIDIA](https://forums.developer.nvidia.com/t/ollama-support-for-jetson-nano/328687) makes it as useable as a TV remote. If there's anyone else out there planning to use one of these things; unless you know what you're doing, **don't**. 

Even after hours of manually compiling libraries it ended up being not fast enough for our demo. To make it worthwhile, we ended up using it as a 'proof of concept' rather than a central part of our presentation. Anyways, back the the hackathon.

### 13:00: **Judgement Day**

![3 people presenting](assets/media/hackathon/presenting.png){:style="display:block; margin-left:auto; margin-right:auto; width: 50%; max-height: 600px;"}

After many hours of work this part was the most scary **and** the most rewarding. Being able to communicate under pressure, handling difficult questions, all while remaining confident and showcasing the features of your work is one of the experiences of all time.

Looking back there are certainly a couple moments that I wish I could've been more prepared for, and I would summarize them into three lessons that I learned:
1. Make your slides as minimal as possible; try to sell your idea, rather than explain it.
2. Keep it short and sweet
3. Run rehearsals until you know the slides backwards.

But at the end of the day you can get a lot done with some faith, courage, and caffeine. 

### 16:30: **Closing Brief**

![Our group holding a cheque award, with event hosts](assets/media/hackathon/chaos1_first_place.jpg){:style="display:block; margin-left:auto; margin-right:auto; width: 70%;"}

![UTS Techfest awards night](assets/media/hackathon/awards_night.jpeg){:style="display:block; margin-left:auto; margin-right:auto; width: 70%;"}

Despite these challenges we managed to pull through to first place. This wasn't an easy task and it certainly taught us a lot about problem solving as much as it did about communication. 

For anyone following in my footsteps and about to do their own hackathon and want some quick takeaways:

1. Effective communication is a must have skill.
2. Innovate, innovate, and innovate. This is a rare opportunity to create something new.
3. Have fun. Making mistakes is part of the journey.


# Final Thoughts

Thank you for [UTS Techfest](https://www.uts.edu.au/events/uts-tech-festival-future-students), [Chaos1](https://www.linkedin.com/company/chaos1/), and the [Australian Army Battle Lab](https://www.facebook.com/BattleLabAustralianArmy/) for the opportunity to create and showcase our solution. I would also like to extend my congratulations to PROXI (2nd place) and FITCOM (3rd place), as well as to all the other Hackathon participants who all put in the effort and hopefully learned something new.

This hackathon was a unique experience that really tested my teams' skills, and I wouldn't skip a heartbeat to do it again.
