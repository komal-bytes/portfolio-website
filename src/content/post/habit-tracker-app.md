---
layout: ../../layouts/post.astro
title: Building a Fully Offline Habit Tracker with React and Dexie
description: In this blog, I share how I built a fully offline, privacy-first habit tracker app using React, Dexie.js, and Vite PWA. Explore the process of managing habit data locally, creating custom scheduling, and providing a seamless offline experience for users.
dateFormatted: Nov 12th, 2024
---

<div class="my-1"></div>

## The Why Behind the Habit Tracker

I created this habit tracker app to help myself build a better tool while learning and improving my development skills. The goal was to make something that respects user privacy by being fully offline and storing data locally—no servers or online databases. With a tech stack that includes React, Dexie.js, Next UI, and Vite PWA, I was able to build a lightweight, flexible app that helps users track their habits and keeps everything private.

## Dexie.js for Local Data Storage

For the data storage, I turned to Dexie.js, which is a really handy wrapper around IndexedDB. It simplifies a lot of the complexities of working with IndexedDB and provides a much friendlier API for handling things like complex queries. The app is built around three key tables:

- **Habits**: Stores the habits the user is tracking, including their frequency and progress. This table is essential for keeping track of each habit’s details.
- **TrackLogs**: Holds logs for the scheduled tracking days based on each habit’s frequency type. This table ensures the app knows when a habit should be tracked.
- **Logs**: Records entries when users update their progress on a habit. This table is critical for tracking actual progress and calculating how close users are to completing their goals.

By organizing data into these three tables, I could efficiently handle habit tracking and progress calculation while keeping everything local and private.

## Flexible Habit Scheduling

One of the coolest features I added was allowing users to choose how often they want to track their habits. Whether it’s daily, specific days of the week, or custom intervals like “every 3 days starting Monday,” there’s a lot of flexibility. For the custom intervals, I used chrono-node, which makes parsing natural language dates into usable data really easy. With this, I can calculate the last, current, and next scheduled dates for each habit.

## Efficient Habit Tracking and Displaying Current Day’s Habits

When users open the app, it automatically updates the scheduled dates for each habit that needs tracking for the day. So I show only the habits whose current scheduled date match the current date. This way, users only see the habits that require attention on that specific day. 

## Streaks and Progress Visualization

Seeing progress is one of the best motivators in habit tracking, so I wanted to make it as visualy exciting. To calculate streaks, I compare the last scheduled date and the last progress update date. If they match, the streak goes up; if not, it resets. This keeps users motivated to stay consistent. I also integrated Chart.js to display the progress in visually engaging graphs, as well as a calendar that highlights the days when progress was made.

## Offline-First Experience with Vite PWA

One of the most important goals for me was to make the app work offline. With Vite PWA, I was able to turn the app into a Progressive Web App (PWA). This means users can install it on their home screens and use it anytime, even without an internet connection. It’s perfect for tracking habits on the go, without worrying about connectivity. If you’re interested in how I made the app fully offline-ready, you can check out my separate blog post on <a href="https://google.com" target="_blank">using Vite PWA for offline capabilities</a>.

Beyond habit tracking, I added some customization options like changing the theme, uploading a profile picture, and updating the user’s name—all of which are stored locally to maintain privacy.

This project deepened my understanding of offline storage, PWA, and UI design—all while building a tool that feels personal and private. I hope this inspires you to explore the power of local-first applications!