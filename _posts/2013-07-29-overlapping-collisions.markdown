---
layout: post
title: Overlapping collisions
date: 2013-07-29 16:23:34 +0530
category: Collision
tags:
    - Physics
    - Vectors
    - Game
author: Ajoy Oommen
published: true
---
A bug in the collision game is the overlap of two colliding balls. This happens when the balls are too fast and by the time the collision detection has started, the balls are partially rendered within each other. I took some time to figure out how exactly this problem occurs and how it can be solved.

**The problem**

If two balls are colliding, resolve_colliding() gives them new velocity vectors.

![Colliding bodies](https://theturingblog.files.wordpress.com/2013/07/resolve_colliding1.png)

But due to the depth of the overlap, the new ball coordinates are also such that the balls are colliding and overlapping.

![New vectors](https://theturingblog.files.wordpress.com/2013/07/resolve_colliding2.png)

This invokes another resolve_colliding() function, which again assigns new vectors. But the balls are still overlapping. And this repeats again and again

**The solutions**

**Solution 1**

The first solution I tried was to find the midpoint of the collisions, and displace the two colliding balls along the line of their centers, away from each other. But this did not work. Here is the code that I used :

    function uncollide(x1, y1, x2, y2)
    {
        /*
         * Two balls at (x1 y1) and (x2 y2)
         *
         * Find their midpoint (h, k) : Find Ax + By + C = 0
         * through their centers. Find the two intersection
         * points on line by the circle C(h, k) - radius ball.w
         *
         * General equation of line through their centers :
         * (2x2 - 2x1)x + (2y2 - 2y1)y + (x1^2 + y1^2 - x2^2 - y2^2) = 0
         * Ax + By + C = 0
         *
         * h = (x1 + x2) / 2
         * k = (y1 + y2) / 2
         *
         * line Ax + By + C = 0 cuts circle with center at (h,k) and radius r = ball.w
         * at (x_a, y_a) and (x_b, y_b)
         *
         * where, r is the distance from point h, k to the required point (x, y)
         *
         */

        var A = 2*x2 - 2*x1;
        var B = 2*y2 - 2*y1;
        var C = square(x1) + square(y1) - square(x2) - square(y2);

        var h = (x1 + x2) / 2;
        var k = (y1 + y2) / 2;

        var r = sprites[0].w;

        var sqrtAB = Math.sqrt(square(A) + square(B)); // (A^2 + B^2)^1/2
        var a = A / sqrtAB;
        var b = B / sqrtAB;
        var d = (A*h + B*k + C) / sqrtAB;
        var sqrtRD = Math.sqrt(square(r) - square(d)); // (r ^2 - d ^2)^1/2
        var x_a = h - a*d + b*sqrtRD;
        var x_b = h - a*d - b*sqrtRD;
        var y_a = k - b*d + a*sqrtRD;
        var y_b = k - b*d - a*sqrtRD;

        return [x_a, y_a, x_b, y_b ];
    }

**Solution 2**

The next solution that I have tried is as follows. Although this solution has worked, I still need to verify all possible cases. But I am very sure that it is logically secure. Here is the solution :

    function checkOverlap (b1, b2)
    {
        /* For two colliding and overlapping balls,
         * set their x y coordinates such that the
         * balls just touch
         */
        b1.setx(b1.cx() + b1.vx * b1.sp);
        b1.sety(b1.cy() + b1.vy * b1.sp);
        b2.setx(b2.cx() + b2.vx * b2.sp);
        b2.sety(b2.cy() + b2.vy * b2.sp);

        if(ball_dist(b1, b2) <= (b1.w / 2))
        {
    	checkOverlap(b1, b2);
        }
        else
        {
    	return;
        }
    }

I have added this function before and after calculating the vectors. This ensures two things :

1. If the two balls are overlapping when the resolve_colliding() function is called, the function will reverse the balls so that they just touch.
2. If the two balls are overlapping after they are assigned new vectors, the balls are traced on the new vectors so that they just touch.

In the first case, the balls won’t overlap and would directly go in the new vector directions, without further ado. In the second case, the balls have already been assigned the vectors and the function ensures that they are not overlapping before continuing the game.