---
layout: ../../layouts/post.astro
title: Building a Fully Offline Habit Tracker with React and Dexie
description: In this blog, I share how I built a fully offline, privacy-first habit tracker app using React, Dexie.js, and Vite PWA. Explore the process of managing habit data locally, creating custom scheduling, and providing a seamless offline experience for users.
dateFormatted: Nov 12th, 2024
---

<div class="my-1"></div>

## The Why Behind the Habit Tracker

I created this habit tracker app to improve my skills while building a tool that respects user privacy by working fully offline and storing data locally. With React, Dexie.js, Next UI, and Vite PWA, I built a lightweight, flexible app that lets users track habits without relying on any external servers or storing data online.

## Dexie.js for Local Data Storage

Dexie.js, a wrapper around IndexedDB, was my go-to for data storage in this app. Dexie simplifies IndexedDB operations and provides a more developer-friendly API, making it easier to handle complex queries and work with multiple tables. I used three tables for the entire app: 

- **Habits**: Stores the list of habits, including their details (like frequency and progress). This table is essential for updating individual habit information.
- **TrackLogs**: Holds logs for the scheduled tracking days based on each habit’s frequency type. This table ensures the app knows when a habit should be tracked.
- **Logs**: Records entries when users update their progress on a habit. This table is critical for tracking actual progress and calculating how close users are to completing their goals.

By organizing data into these three tables, I could efficiently handle habit tracking and progress calculation while keeping everything local and private.

## Flexible Habit Scheduling

To make habit tracking adaptable to different lifestyles, I allowed users to set the frequency of each habit: daily, specific days of the week, or custom intervals (like “every 3 days starting Monday”). For handling custom dates, I used `chrono-node`, which parses natural language dates into actionable data. The frequency data allowed me to calculate the last, current, and next scheduled dates for each habit. 

## Efficient Habit Tracking and Displaying Current Day’s Habits

When users open the app, it automatically updates the scheduled dates for each habit that needs tracking for the day. So I show only the habits whose current scheduled date match the current date. This way, users only see the habits that require attention on that specific day. 

## Streaks and Progress Visualization

A key motivator in habit tracking is seeing tangible progress. I used the last scheduled date and last progress update date to calculate streaks. If the dates match, the app increments the user’s streak; otherwise, the streak resets, encouraging consistency. Additionally, I integrated Chart.js to present progress visually through engaging graphs, alongside a calendar view highlighting days with progress updates.

## Offline-First Experience with Vite PWA

One of my primary goals was to make the app fully functional offline. With Vite PWA, I transformed the app into a Progressive Web App (PWA), ensuring users can add it to their home screen and access it anytime, even without an internet connection. This setup allows users to seamlessly track their habits wherever they are. For a more in-depth look at making the app offline-ready, you can read my blog post on
<a href="https://google.com" target="_blank">using Vite PWA for offline capabilities</a>.

Beyond core habit tracking, the app includes customization options such as changing the theme, adding a profile picture, and changing the user’s name—both stored locally to maintain privacy.

This project deepened my understanding of offline storage, PWA, and UI design—all while building a tool that feels personal and private. I hope this inspires others to explore the power of local-first applications!
