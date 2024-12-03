---
layout: base
title: Choose Your Classes
search_exclude: true
permalink: /posts/
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

