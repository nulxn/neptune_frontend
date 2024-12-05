---
layout: base
title: Choose Your Classes
search_exclude: true
permalink: /posts/
---

{% raw %}
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Simple Chatroom</title>

<style>
  body, html {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    font-family: Arial, sans-serif;
    background-color: #222;
    color: white;
    display: flex;
    flex-direction: column;
  }

  /* Chatroom Container */
  .chatroom {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100vh;
    width: 100%;
  }

  /* Chat Messages */
  .chat-container {
    flex: 1;
    overflow-y: auto;
    padding: 10px;
  }

  .message {
    max-width: 70%;
    margin: 10px 0;
    padding: 10px;
    border-radius: 5px;
    word-wrap: break-word;
  }

  .message.left {
    background-color: #444;
    color: white;
    align-self: flex-start;
  }

  .message.right {
    background-color: #007bff;
    color: white;
    align-self: flex-end;
  }

  /* Input Section */
  .input-container {
    display: flex;
    padding: 10px;
    background: #333;
    border-top: 1px solid #444;
    display:none;
  }

  input {
    flex: 1;
    padding: 10px;
    font-size: 14px;
    border: none;
    border-radius: 5px;
    margin-right: 10px;
    outline: none;
    color: black;
  }

  button {
    background-color: #007bff;
    color: white;
    padding: 10px;
    font-size: 14px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>

<div class="chatroom">
  <!-- Chat Messages -->
  <div class="chat-container" id="chat-container">
    <div class="message left">Welcome to the chatroom!</div>
  
  <div>
  <button class="Button" onClick="OpenChat('Maths')">Maths</button>
   <button class="Button" onClick="OpenChat('Science')">Science</button>
    <button class="Button" onClick="OpenChat('CS')">Computer Science</button>
     <button class="Button" onClick="OpenChat('History')">History</button>
      <button class="Button" onClick="OpenChat('English')">English</button>
  </div>
   <div class="message left" id="chatsubject">Click on any subject above to open subject chat!</div>
   <div id="chatbox1" style="
        display:none;
        padding: 10px; 
        height: 300px; 
        overflow-y: auto; 
        background:#1A2830; /* Very light sea blue */
        border-bottom: 2px solid #0077be;
    ">
    </div>
    
  <div class="input-container" id="chatinput">
     <input type="text" id="message-input" placeholder="Type your message here...">
    <button id="send-button">Send</button>
  </div>
  
</div>
  

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
    background-color: #f0faff; /* Light background */
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
        background-color: #e0f7ff; /* Very light sea blue */
        border-bottom: 2px solid #0077be;
    ">
    </div>
    <div style="display: flex; flex-direction: column; padding: 10px; gap: 10px; background-color: #0077be;">
        <input type="text" id="question" placeholder="Type your question here" style="
            width: 100%; 
            padding: 10px; 
            border: none; 
            border-radius: 5px;
            background-color: white; /* Light box background */
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

async function OpenChat(subject) {
  const chatBox = document.getElementById("chatbox1");
  chatBox.style.display = "block";
  const chatinput = document.getElementById("chatinput");
  chatinput.style.display = "block";
  const chatsubject = document.getElementById("chatsubject");
  chatsubject.textContent = subject;
   
}
    // Chatbot Logic
    async function sendQuestion() {
        const question = document.getElementById("question").value;
        const chatBox = document.getElementById("chat-box");

        // Show the user's question in the chat
        chatBox.innerHTML += `
            <div style="margin-bottom: 10px; background: #e8f4ff; padding: 10px; border-radius: 8px;">
                <strong style="color: #0056b3;">You:</strong> 
                <span style="color: #0056b3;">${question}</span>
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
  });
{% endraw %}
