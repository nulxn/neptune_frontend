---
layout: post
title: Profile Settings
permalink: /profile
menu: nav/home.html
search_exclude: true
show_reading_time: false
---

<style>
  /* Center the modal content */
  .profile-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding: 20px;
  }

  .card {
    background-color: #fff;
    border-radius: 8px;
    padding: 30px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 600px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* Make the profile picture large and centered */
  #profileImageBox {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }

  #profileImageBox img {
    width: 200px; /* Increase the size */
    height: 200px; /* Increase the size */
    border-radius: 50%; /* Makes the image circular */
    object-fit: cover; /* Ensures the image maintains aspect ratio */
    border: 3px solid #ccc; /* Optional border */
  }

  /* Form elements styling */
  form {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 15px;
    align-items: center; /* Center align text boxes */
  }

  input {
    width: 80%; /* Adjust width */
    padding: 10px;
    border-radius: 4px;
    border: 1px solid #ccc;
  }

  label {
    font-size: 1rem;
    margin-bottom: 5px;
    width: 80%; /* Align with input fields */
    text-align: left; /* Left-align the label text */
  }

  /* Styling for the file input and icon */
  .file-icon {
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 15px;
  }

  .file-icon i {
    margin-left: 5px;
  }

  #profile-message {
    color: red;
  }
</style>

<div class="profile-container">
  <div class="card">
    <!-- Profile Picture -->
    <div id="profileImageBox">
      <img src="/path/to/placeholder-image.png" alt="Profile Picture">
    </div>
    <!-- Form -->
    <form>
      <div>
        <label for="newUid">Enter New UID:</label>
        <input type="text" id="newUid" placeholder="New UID">
      </div>
      <div>
        <label for="newName">Enter New Name:</label>
        <input type="text" id="newName" placeholder="New Name">
      </div>
      <div>
        <label for="newPassword">Enter New Password:</label>
        <input type="text" id="newPassword" placeholder="New Password">
      </div>
      <label for="profilePicture" class="file-icon">
        Upload Profile Picture <i class="fas fa-upload"></i>
      </label>
      <input type="file" id="profilePicture" accept="image/*">
      <p id="profile-message" style="color: red;"></p>
    </form>
  </div>
</div>
<script type="module">
  // Import fetchOptions from config.js
  import { pythonURI, fetchOptions } from '{{site.baseurl}}/assets/js/api/config.js';
  // Import functions from config.js
  import { putUpdate, postUpdate, deleteData, logoutUser } from "{{site.baseurl}}/assets/js/api/profile.js";
  // Function to fetch user profile data
// Function to fetch user profile data
async function fetchUserProfile() {
  const URL = pythonURI + "/api/id/pfp"; // Adjusted endpoint to fetch user profile data
  try {
    const response = await fetch(URL, fetchOptions);
    if (!response.ok) {
      throw new Error(`Failed to fetch user profile: ${response.status}`);
    }
    const profileData = await response.json();
    displayUserProfile(profileData);
  } catch (error) {
    console.error("Error fetching user profile:", error.message);
    document.getElementById('profile-message').textContent = 'Error loading profile data. Please try again.';
  }
}
// Function to display user profile data
function displayUserProfile(profileData) {
  const profileImageBox = document.getElementById('profileImageBox');
  profileImageBox.innerHTML = ''; // Clear existing content
  if (profileData.pfp) {
    const img = document.createElement('img');
    img.src = `data:image/jpeg;base64,${profileData.pfp}`; // Render Base64 image
    img.alt = 'Profile Picture';
    profileImageBox.appendChild(img);
  } else {
    // Display default placeholder image
    const placeholder = document.createElement('img');
    placeholder.src = '/path/to/placeholder-image.png'; // Replace with actual placeholder image path
    placeholder.alt = 'Default Profile Picture';
    profileImageBox.appendChild(placeholder);
  }
}
  // Function to save profile picture
  window.saveProfilePicture = async function () {
    const fileInput = document.getElementById('profilePicture');
    const file = fileInput.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = function() {
        const profileImageBox = document.getElementById('profileImageBox');
        profileImageBox.innerHTML = `<img src="${reader.result}" alt="Profile Picture">`;
      };
      reader.readAsDataURL(file);
    }
    if (!file) return;
    try {
      const base64String = await convertToBase64(file);
      await sendProfilePicture(base64String);
      console.log('Profile picture uploaded successfully!');
    } catch (error) {
      console.error('Error uploading profile picture:', error.message);
    }
  }
  // Function to convert file to base64
  async function convertToBase64(file) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = () => resolve(reader.result.split(',')[1]); // Remove the prefix part of the result
      reader.onerror = error => reject(error);
      reader.readAsDataURL(file);
    });
  }
  // Function to send profile picture to server
  async function sendProfilePicture(base64String) {
    const URL = pythonURI + "/api/id/pfp"; // Adjust endpoint as needed
    const options = {
      URL,
      body: { pfp: base64String },
      message: 'profile-message',
      callback: () => {
        console.log('Profile picture uploaded successfully!');
      }
    };
    try {
      await putUpdate(options);
    } catch (error) {
      console.error('Error uploading profile picture:', error.message);
      document.getElementById('profile-message').textContent = 'Error uploading profile picture: ' + error.message;
    }
  }
  // Call the function to fetch user profile on page load
  window.onload = fetchUserProfile;
</script>
