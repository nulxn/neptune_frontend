---
layout: base
title: Choose Your Classes
search_exclude: true
permalink: /choose_classes/
---

<!-- Include AI Homework Bot -->
<link rel="stylesheet" href="/path-to/ai-homework-bot.css">
<script src="/path-to/ai-homework-bot.js"></script>

<style>
  .header-container {
    display: flex;
    align-items: center; /* Ensures vertical alignment between logo and title */
    justify-content: center;
    margin-bottom: 30px;
  }

  .logo-container {
    margin-right: 25px; /* Adds space between the logo and text */
  }

  .logo-container img {
    height: 110px; /* Increased logo size */
    width: auto;
  }

  h1 {
    font-size: 3.5rem; /* Keeps heading size proportional to the logo */
    color: white;
    margin: 0;
    display: flex;
    align-items: center; /* Aligns text with the center of the logo */
  }

  .dropdown-container {
    text-align: center;
    color: white;
  }

  .dropdown {
    background: linear-gradient(135deg, #4e4e4e, #1a1a1a);
    color: white;
    font-size: 2rem; /* Larger font size */
    border: none;
    border-radius: 30px;
    padding: 25px 40px; /* Increased padding for larger button size */
    width: 350px; /* Increased width */
    text-align: left;
    cursor: pointer;
    margin: 20px auto;
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3);
    transition: transform 0.2s ease, background 0.3s ease, margin 0.2s ease, box-shadow 0.3s ease;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .dropdown:hover {
    background: linear-gradient(135deg, #6a6a6a, #2d2d2d);
    transform: scale(1.1); /* Slightly larger hover effect */
    margin-bottom: 30px; /* Increased spacing on hover */
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
  }

  .dropdown:focus {
    outline: none;
  }

  .dropdown::after {
    content: "▼";
    font-size: 1.5rem; /* Increased arrow size */
    margin-left: 10px;
  }
</style>

<!-- Header with Logo and Title -->
<div class="header-container">
  <div class="logo-container">
    <img src="{{site.baseurl}}/navigation/images/neptune.png" alt="Logo">
  </div>
  <h1>Choose Your Classes</h1>
</div>

<!-- Dropdown Menu -->
<div class="dropdown-container">
  <button class="dropdown">Period 1</button>
  <button class="dropdown">Period 2</button>
  <button class="dropdown">Period 3</button>
  <button class="dropdown">Period 4</button>
  <button class="dropdown">Period 5</button>
</div>
