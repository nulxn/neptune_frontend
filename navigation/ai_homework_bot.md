---
layout: base
title: Poseidon Homework Bot
search_exclude: true
menu: nav/home.html
permalink: /ai_homework_bot/
---
<!-- Full-Page Poseidon Homework Bot -->
<div id="poseidon-bot-container" style="
    display: flex;
    flex-direction: column;
    height: 100vh; /* Full page height */
    background: linear-gradient(135deg, #4f91d8, #1c5faa); /* Full-page gradient */
    font-family: 'Arial', sans-serif;
    color: white;
">
    <!-- Header -->
    <div style="
        background-color: #0056b3; /* Deeper blue for the header */
        padding: 20px;
        text-align: center;
        font-size: 2rem;
        font-weight: bold;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    ">
        Poseidon Homework Bot
    </div>

    <!-- Chat Section -->
    <div id="chat-box" style="
        flex-grow: 1; /* Take up remaining space */
        padding: 20px;
        overflow-y: auto;
        background-color: rgba(209, 236, 245, 0.85); /* Semi-transparent aqua */
        border-top: 2px solid #0056b3;
        border-bottom: 2px solid #0056b3;
        box-shadow: inset 0 4px 8px rgba(0, 0, 0, 0.1);
        border-radius: 10px; /* Slightly rounded corners */
    ">
    </div>

    <!-- Input Section -->
    <div style="
        display: flex;
        padding: 20px;
        background-color: #0056b3; /* Same blue as the header */
        box-shadow: 0 -4px 8px rgba(0, 0, 0, 0.3);
    ">
        <input type="text" id="question" placeholder="Type your question here" style="
            flex-grow: 1; /* Take up as much space as possible */
            padding: 15px;
            border: none;
            border-radius: 5px;
            margin-right: 10px;
            font-size: 1rem;
            color: #0056b3; /* Dark text */
            background-color: white; /* Input background */
        ">
        <button onclick="sendQuestion()" style="
            padding: 15px 20px;
            background-color: white; /* Contrasting button background */
            color: #0056b3; /* Dark blue text */
            font-size: 1rem;
            font-weight: bold;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease, color 0.3s ease;
        ">
            Ask Poseidon
        </button>
        <button onclick="recordVoice()" style="
            padding: 15px 20px;
            background-color: white; /* Contrasting button background */
            color: #0056b3; /* Dark blue text */
            font-size: 1rem;
            font-weight: bold;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-left: 10px;
            transition: background-color 0.3s ease, color 0.3s ease;
        ">
            🎙️ Record
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

    // Chatbot Logic
    async function sendQuestion() {
        const question = document.getElementById("question").value;
        const chatBox = document.getElementById("chat-box");

        // Display the user's question
        chatBox.innerHTML += `
            <div style="margin-bottom: 20px; background: rgba(232, 244, 255, 0.85); padding: 15px; border-radius: 8px;">
                <strong style="color: #0056b3;">You:</strong> 
                <span style="color: #0056b3;">${question}</span>
            </div>`;

        // Send the question to the backend
        const response = await fetch(`${pythonURI}/api/ai/help`, {
            ...fetchOptions,
            method: "POST",
            body: JSON.stringify({ question })
        });

        // Display the AI's response
        const data = await response.json();
        const aiResponse = data.response || "Error: Unable to fetch response.";

        chatBox.innerHTML += `
            <div style="margin-bottom: 20px; background: #0077be; padding: 15px; border-radius: 8px; color: white;">
                <strong>Poseidon:</strong> ${aiResponse}
            </div>`;
        document.getElementById("question").value = ""; // Clear input
        chatBox.scrollTop = chatBox.scrollHeight; // Auto-scroll
    }

    // Record voice and convert to text
    function recordVoice() {
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.lang = 'en-US';
        recognition.interimResults = false;
        recognition.maxAlternatives = 1;

        recognition.start();

        recognition.onresult = (event) => {
            const transcript = event.results[0][0].transcript;
            alert(`You said: ${transcript}`);
    };

    recognition.onerror = (event) => {
        console.error(`Speech Recognition Error: ${event.error}`);
        alert(`Error occurred: ${event.error}`);
    };
}

    
</script>

<!-- Optional CSS -->
<style>
    /* Chatbox Scrollbar Styling */
    #chat-box::-webkit-scrollbar {
        width: 8px;
    }

    #chat-box::-webkit-scrollbar-thumb {
        background: #0056b3; /* Match header and footer */
        border-radius: 5px;
    }

    /* Button Hover Effect */
    button:hover {
        background-color: #0077be;
        color: white;
    }
</style>
