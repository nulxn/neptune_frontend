---
layout: base
title: Ai Homework Bot
search_exclude: true
permalink: /ai_homework_bot/
---
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
    background-color: #e0f7fa; /* Light sea blue */
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
        background-color: #03ecfc; /* tealish color */
    ">
    </div>
    <div style="display: flex; flex-direction: column; padding: 10px; gap: 10px; background-color: #e0f7fa;">
        <input type="text" id="question" placeholder="Type your question here" style="
            width: 100%; 
            padding: 10px; 
            border: 1px solid #ddd; 
            border-radius: 5px;
            background-color: #f0ffff; /* Light aqua */
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
        method: 'GET', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
        credentials: 'include', // include, same-origin, omit
        headers: {
            'Content-Type': 'application/json',
            'X-Origin': 'client' // New custom header to identify source
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
        chatBox.innerHTML += `<div style="margin-bottom: 10px;"><strong>You:</strong> ${question}</div>`;

        // Send the question to the backend
        const response = await fetch(`${pythonURI}/api/ai/help`, {
            ...fetchOptions,
            method: "POST",
            body: JSON.stringify({ question })
        });

        // Get and display the AI's response
        const data = await response.json();
        const aiResponse = data.response || "Error: Unable to fetch response.";

        chatBox.innerHTML += `<div style="margin-bottom: 10px; color: #0077be;"><strong>Poseidon:</strong> ${aiResponse}</div>`;
        document.getElementById("question").value = ""; // Clear input
        chatBox.scrollTop = chatBox.scrollHeight; // Auto-scroll
    }
</script>

<!-- Optional CSS (Inline or Move to a Global CSS File) -->
<style>
    /* Button Hover Effect */
    #ai-bot-button:hover {
        background-color: #003f6b;
        /* Even darker blue for hover */
    }

    /* Chatbox Scrollbar */
    #chat-box::-webkit-scrollbar {
        width: 5px;
    }

    #chat-box::-webkit-scrollbar-thumb {
        background: #0077be;
        /* Ocean blue */
        border-radius: 5px;
    }
</style>