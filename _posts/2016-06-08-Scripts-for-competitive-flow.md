---
layout: post
title: CompetitiveFlow
summary: Scripts for CodeForces competitive programming platform.
category: schemes
---

## Insane habit of copy-pasting tests

If you have done any competitive programming on codeforces you may have noticed that in those intense moments when the clock is ticking away and some new insight into problem strikes you, you start copy-pasting test problems into terminal. And if you do it for a few times it becomes an insane habit that can ruin your train of thought that may lead to a solution.

So after a few hours of tweaking I have made this pair of scripts that will allow you to test your solutions more easily. Check it out on [Github](https://github.com/dulex123/competitive-flow)

<iframe width="640" height="400" src="https://www.youtube.com/embed/vFjqoGfHRBk" frameborder="0" allowfullscreen></iframe>



## Update:

After using competitive flow for a few rounds, I've got some new ideas that would be worth developing for even faster coding.

Features:

- Program connected to browser via extension with native support - this will enable the program to gather problem info without copying data

- Terminal-gui ([Ncurses lib](https://en.wikipedia.org/wiki/Ncurses)) where a,b,c,d,e,f selects problem mode and 1,2,3,4.. runs particular test on it

- Run all tests / Submit command

- Init - initialize folder with separate files for each problem filled with boilerplate code
