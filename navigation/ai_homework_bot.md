---
layout: base
title: Poseidon Homework Bot
search_exclude: true
permalink: /ai_homework_bot/
---

<!-- Full-Page Poseidon Homework Bot -->
<div id="poseidon-bot-container" style="
    display: flex;
    flex-direction: column;
    height: 100vh; /* Full page height */
    background-color: #0077be; /* Ocean blue background */
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
        background-color: #e0f7fa; /* Light sea blue for the chat background */
        border-top: 2px solid #0056b3;
        border-bottom: 2px solid #0056b3;
        box-shadow: inset 0 4px 8px rgba(0, 0, 0, 0.1);
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
            color: #0056b3; /* Text color */
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
            <div style="margin-bottom: 20px; background: #e8f4ff; padding: 15px; border-radius: 8px;">
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
</script>

<!-- Optional CSS -->
<style>
    /* Chatbox Scrollbar Styling */
    #chat-box::-webkit-scrollbar {
        width: 8px;
    }

    #chat-box::-webkit-scrollbar-thumb {
        background: #0077be;
        border-radius: 5px;
    }

    /* Button Hover Effect */
    button:hover {
        background-color: #0077be;
        color: white;
    }
</style>
