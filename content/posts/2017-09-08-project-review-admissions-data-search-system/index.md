---
title: "Project Review: Admissions Data Search System"
date: 2017-09-08
tags:
- Project
- PHP
- MySQL
slug: project-review-admissions-data-search-system
aliases: /en/project-review-admissions-data-search-system.html
---

{{< figure src="images/WelcomeMain.png" title="The Main Page" class="figure-center" >}}

## What This Project Is And Why I Did It

This is my first complete project. Its purpose is providing admissions data to potential new students.

I did this project as a competency test when I joined Hongmantang Studio. At the time some third-party websites provided admissions data to help potential new students decide whether they should apply to a university. Those sites tried to collect admissions data from as many schools as possible. Thus, they did not have enough details for each college. Our studio wanted to create a system that could show enough details about the admissions data for our university.

<!--more-->

We decided to use PHP as the programming language and MySQL as the database because that’s the only production environment our studio had at that time. This limited our choices, so we switched to the Linux and Docker environment later. That gave us more options.

{{< figure src="images/WelcomeResult.png" title="A Search Result" class="figure-center" >}}

## How I Did The Project

This project was a big challenge for me because, as I mentioned in another post, I only knew some very fundamental knowledge about PHP such as how to write ```<?php echo "Hello World"; ?>```, and I did not know the database and specifically, MySQL.

One of my mentors, SpecialCyCi, introduced me a framework called ThinkPHP, which is an object-oriented framework for PHP that supports MySQL as a database. He promised me that it would be easier to use a framework instead of beginning from scratch since a framework already finishes many details for us, making it possible for us to focus on our project. I spent about one week reading the whole documents of ThinkPHP and following the tutorials to write some simple code.

It was July, and the results of the National College Entrance Examination (NCEE) would come out soon. Since new students would begin to search admission data and apply for universities, I did not have enough time to finish the project.

I downloaded data in Excel format from the admission office website and imported it to the database by using `phpMyAdmin`. I knew I should write a user-friendly interface for importing so that future developers could import the data directly instead of using third-party tools. However, I didn’t have the knowledge at the time. I fixed this problem the next year when we needed to reuse the system.

The next challenge was the algorithm. I wanted to extract all the colleges and majors from the data, find their connections, and combine similar majors. This is not difficult for me now. A nest iteration and some regular expression, or even a third-party library can solve this problem. But four years ago, before I even knew the definition of the algorithm, the problem brought me a lot of trouble. The loop never stopped. The results did not format in the way I had wanted. Too many variables make me crazy. At the end of the day, I decided to write down all of my thoughts on a piece of paper and run the code by hand first. That helped me a lot.

I finished the project one day before the NCEE scores released. Thanks to our fantastic market team, the new tool became famous to students who were planning to apply to the university. In the next few days, I received much feedback and fixed some bugs.

## What I Learned From This Project

**Try to use a framework.** A useful framework can handle all the trivial details so that we only need to focus on the functions themselves. For example, the framework that I used, ThinkPHP, could manage database connections, request routing, interface rendering, etc. Thus, I could begin to code immediately instead of spending a couple of hours to figure out how to connect the database. A good framework can also adapt to different development or production environments, so we do not need to worry about moving our application from one production environment to another.

**Try to read documents.** I know there are many tutorials that can help us begin to program in less than ten minutes. However, reading the official documents is still essential because tutorials will not show you everything. Reading documents at the beginning is better than struggling with something first and then finding the needed documents. Once, I had written a very complicated authorization system before I realized that ThinkPHP had built-in authorization system.

**Try to learn algorithms.** I did not realize the importance of algorithms before I took the algorithms course. I always thought that if the computer was fast enough, it could solve every problem by itself. (This is still true for most case.) However, there were a couple of times I realized my programs were running too slow (I mean more than one second), but I did not know how to fix them. After learning algorithms, I knew that a nested loop was much slower than a single loop. And there were serval methods that I could use to convert a nested loop to a single loop.

**Try not to rely on the frameworks.** Sometimes, a language’s built-in function is easier than a framework’s function. So we should not rely on the frameworks all the time. Doing so may also make us feel uncomfortable when we need to move to a new framework. In this project, I learned the ThinkPHP’s syntaxes before I learned PHP’s syntaxes, even though some of them are same. Thus, I relied on ThinkPHP and felt uncomfortable to move to another framework. And later when I used another framework called Laravel, I could not help but used some syntaxes from ThinkPHP. This bad behavior caused lots of problems, and I spent a couple of weeks to get rid of it.