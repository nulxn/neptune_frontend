---
layout: page
title: Login
permalink: /login
search_exclude: true
menu: nav/home.html
show_reading_time: false
---

<div class="login-container">
  <div class="login-card">
    <h1 id="pythonTitle">Login</h1>
    <p>If you already have an account</p>
    <form id="pythonForm" onsubmit="pythonLogin(); return false;">
      <p>
        <label>
          Username:
          <input type="text" name="uid" id="uid" required />
        </label>
      </p>
      <p>
        <label>
          Password:
          <input type="password" name="password" id="password" required />
        </label>
      </p>
      <p>
        <button type="submit">Login</button>
      </p>
      <p id="message" style="color: red"></p>
    </form>
  </div>
  <div class="signup-card">
    <h1 id="signupTitle">Sign Up</h1>
    <p>Or if you don't, create one</p>
    <form id="signupForm" onsubmit="signup(); return false;">
      <p>
        <label>
          Name:
          <input type="text" name="name" id="name" required />
        </label>
      </p>
      <p>
        <label>
          Username:
          <input type="text" name="signupUid" id="signupUid" required />
        </label>
      </p>
      <p>
        <label>
          Password:
          <input
            type="password"
            name="signupPassword"
            id="signupPassword"
            required
          />
        </label>
      </p>
      <p>
        <button type="submit">Sign Up</button>
      </p>
      <p id="signupMessage" style="color: green"></p>
    </form>
  </div>
</div>

<script type="module">
  import {
    login,
    pythonURI,
    fetchOptions,
  } from "{{site.baseurl}}/assets/js/api/config.js";

  window.pythonLogin = function () {
    const options = {
      URL: `${pythonURI}/api/authenticate`,
      callback: pythonDatabase,
      message: "message",
      method: "POST",
      cache: "no-cache",
      body: {
        uid: document.getElementById("uid").value,
        password: document.getElementById("password").value,
      },
    };
    login(options);
  };

  window.signup = function () {
    const signupButton = document.querySelector(".signup-card button");
    signupButton.disabled = true;
    signupButton.style.backgroundColor = "#d3d3d3";

    const signupOptions = {
      URL: `${pythonURI}/api/user`,
      method: "POST",
      cache: "no-cache",
      body: {
        name: document.getElementById("name").value,
        uid: document.getElementById("signupUid").value,
        password: document.getElementById("signupPassword").value,
      },
    };

    fetch(signupOptions.URL, {
      method: signupOptions.method,
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(signupOptions.body),
    })
      .then((response) => {
        if (!response.ok) {
          throw new Error(`Signup failed: ${response.status}`);
        }
        return response.json();
      })
      .then((data) => {
        document.getElementById("signupMessage").textContent =
          "Signup successful!";
      })
      .catch((error) => {
        console.error("Signup Error:", error);
        document.getElementById(
          "signupMessage"
        ).textContent = `Signup Error: ${error.message}`;
        signupButton.disabled = false;
        signupButton.style.backgroundColor = "";
      });
  };

  function pythonDatabase() {
    const URL = `${pythonURI}/api/user`;

    fetch(URL, fetchOptions)
      .then((response) => {
        if (!response.ok) {
          throw new Error(`Flask server response: ${response.status}`);
        }
        return response.json();
      })
      .then((data) => {
        window.location.href = "{{site.baseurl}}/profile";
      })
      .catch((error) => {
        console.error("Python Database Error:", error);
        const errorMsg = `Python Database Error: ${error.message}`;
      });
  }

  window.onload = function () {
    const isAuthenticated = document.cookie.includes("auth_token");
    if (isAuthenticated) {
      pythonDatabase();
    } else {
      console.log("Not authenticated");
    }
  };
</script>

<style>
  .login-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .signup-card,
  .login-card {
    width: 300px;
    height: 300px;
    margin: 20px;
    padding: 20px;
    border: 1px solid #d3d3d3;
    border-radius: 5px;
  }
</style>
