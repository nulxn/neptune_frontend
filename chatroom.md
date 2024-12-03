---
layout: post
title: Chatroom
search_exclude: true
description: Discuss your classes with your classmates  
hide: true
menu: nav/home.html
---

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Galaxy Chatroom</title>
  <style>
    body, html {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      font-family: Arial, sans-serif;
      color: white;
    }

    #galaxy {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
    }

    .chatroom {
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 100vh;
      padding: 20px;
    }

    .chat-container {
      flex: 1;
      overflow-y: auto;
      margin-bottom: 10px;
    }

    .message {
      max-width: 60%;
      margin: 10px 0;
      padding: 10px 15px;
      border-radius: 20px;
    }

    .message.left {
      background-color: rgba(255, 255, 255, 0.1);
      margin-left: 0;
    }

    .message.right {
      background-color: rgba(0, 150, 255, 0.7);
      margin-left: auto;
    }

    .input-container {
      display: flex;
      align-items: center;
    }

    input {
      flex: 1;
      padding: 10px;
      border: none;
      border-radius: 20px;
      margin-right: 10px;
    }

    button {
      background-color: #007bff;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 20px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="chatroom">
    <div class="chat-container">
      <div class="message left">Hello!</div>
      <div class="message right">Hi, how are you?</div>
      <div class="message left">I'm good, thanks!</div>
    </div>
    <div class="input-container">
      <input type="text" placeholder="Type your message here" />
      <button>&#10148;</button>
    </div>
  </div>
  <canvas id="galaxy"></canvas>
  <script>
    const canvas = document.getElementById("galaxy");
    const ctx = canvas.getContext("2d");

    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const stars = Array(200).fill().map(() => ({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      size: Math.random() * 2,
      speed: Math.random() * 0.5 + 0.2,
    }));

    function drawGalaxy() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      stars.forEach(star => {
        ctx.beginPath();
        ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2);
        ctx.fillStyle = "white";
        ctx.fill();
      });
    }

    function animateGalaxy() {
      stars.forEach(star => {
        star.y += star.speed;
        if (star.y > canvas.height) {
          star.y = 0;
          star.x = Math.random() * canvas.width;
        }
      });
      drawGalaxy();
      requestAnimationFrame(animateGalaxy);
    }

    animateGalaxy();
  </script>
</body>


<!-- Poseidon Homework Bot Floating Button -->
<div id="ai-bot-button" style="
    position: fixed;
    bottom: 20px;
    right: 20px;
    background-color: #0056b3; /* Deeper ocean blue */
    color: white;
    padding: 15px;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    z-index: 1000;
    font-size: 20px;
    text-align: center;
">
    🔱 <!-- Trident Icon -->
</div>

<!-- Poseidon Homework Bot Popup Modal -->
<div id="ai-bot-popup" style="
    display: none;
    position: fixed;
    bottom: 80px;
    right: 20px;
    width: 320px;
    background-color: #cce7ff; /* Soft ocean blue */
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    z-index: 1000;
    overflow: hidden;
    font-family: 'Arial', sans-serif;
">
    <div style="
        background-color: #0077be; /* Ocean blue */
        color: white;
        padding: 10px;
        font-size: 18px;
        text-align: center;
    ">
        Poseidon Homework Bot
        <span id="close-bot" style="float: right; cursor: pointer;">&times;</span>
    </div>
    <div id="chat-box" style=" 
        padding: 10px; 
        height: 300px; 
        overflow-y: auto; 
        background-color: #f0faff; /* Very light sea blue */
        border-bottom: 2px solid #0077be;
    ">
    </div>
    <div style="display: flex; flex-direction: column; padding: 10px; gap: 10px; background-color: #cce7ff;">
        <input type="text" id="question" placeholder="Type your question here" style="
            width: 100%; 
            padding: 10px; 
            border: 1px solid #ddd; 
            border-radius: 5px;
            background-color: #e8f4ff; /* Light aqua */
            color: #0056b3; /* Deep blue text */
        ">
        <button onclick="sendQuestion()" style="
            width: 100%; 
            padding: 10px; 
            background-color: #0056b3; /* Darker blue for the button */
            color: white; 
            border: none; 
            border-radius: 5px; 
            cursor: pointer;
            font-weight: bold;
        ">
            Ask Poseidon
        </button>
    </div>
</div>

<!-- JavaScript for Chatbot -->
<script>
    var pythonURI;
    if (location.hostname === "localhost") {
        pythonURI = "http://localhost:8887";
    } else if (location.hostname === "127.0.0.1") {
        pythonURI = "http://127.0.0.1:8887";
    } else {
        pythonURI = "https://flocker.nighthawkcodingsociety.com";
    }

    const fetchOptions = {
        method: 'GET',
        mode: 'cors',
        cache: 'default',
        credentials: 'include',
        headers: {
            'Content-Type': 'application/json',
            'X-Origin': 'client'
        },
    };

    // Toggle Popup
    const botButton = document.getElementById('ai-bot-button');
    const botPopup = document.getElementById('ai-bot-popup');
    const closeBot = document.getElementById('close-bot');

    botButton.addEventListener('click', () => {
        botPopup.style.display = 'block';
    });

    closeBot.addEventListener('click', () => {
        botPopup.style.display = 'none';
    });

    // Chatbot Logic
    async function sendQuestion() {
        const question = document.getElementById("question").value;
        const chatBox = document.getElementById("chat-box");

        // Show the user's question in the chat
        chatBox.innerHTML += `
            <div style="margin-bottom: 10px; background: #e8f4ff; padding: 10px; border-radius: 8px;">
                <strong style="color: #0056b3;">You:</strong> ${question}
            </div>`;

        // Send the question to the backend
        const response = await fetch(`${pythonURI}/api/ai/help`, {
            ...fetchOptions,
            method: "POST",
            body: JSON.stringify({ question })
        });

        // Get and display the AI's response
        const data = await response.json();
        const aiResponse = data.response || "Error: Unable to fetch response.";

        chatBox.innerHTML += `
            <div style="margin-bottom: 10px; background: #0077be; padding: 10px; border-radius: 8px; color: white;">
                <strong>Poseidon:</strong> ${aiResponse}
            </div>`;
        document.getElementById("question").value = ""; // Clear input
        chatBox.scrollTop = chatBox.scrollHeight; // Auto-scroll
    }
</script>

<!-- Optional CSS -->
<style>
    /* Button Hover Effect */
    #ai-bot-button:hover {
        background-color: #003f6b;
    }

    /* Chatbox Scrollbar */
    #chat-box::-webkit-scrollbar {
        width: 5px;
    }

    #chat-box::-webkit-scrollbar-thumb {
        background: #0077be;
        border-radius: 5px;
    }
</style>