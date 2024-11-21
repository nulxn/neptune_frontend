---
layout: base 
title: Ai Homework Bot 
search_exclude: true
permalink: /ai_homework_bot/
---
# AI Homework Helper

<div id="chat-box" style="border: 1px solid #ddd; padding: 10px; height: 300px; overflow-y: auto; background-color: #f9f9f9;"></div>

<input type="text" id="question" placeholder="Type your question here" style="width: 80%; padding: 10px; margin-top: 10px;">
<button onclick="sendQuestion()" style="padding: 10px; background-color: #007bff; color: white; border: none; cursor: pointer;">Ask</button>

<script>
    async function sendQuestion() {
        const question = document.getElementById("question").value;
        const chatBox = document.getElementById("chat-box");

        // Show the user's question in the chat
        chatBox.innerHTML += `<div><strong>You:</strong> ${question}</div>`;

        // Send the question to the backend
        const response = await fetch("/api/ai/help", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({ question })
        });

        // Get and display the AI's response
        const data = await response.json();
        const aiResponse = data.response || "Error: Unable to fetch response.";

        chatBox.innerHTML += `<div><strong>AI:</strong> ${aiResponse}</div>`;
        document.getElementById("question").value = ""; // Clear input
        chatBox.scrollTop = chatBox.scrollHeight; // Auto-scroll
    }
</script>
