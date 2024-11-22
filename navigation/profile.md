--
layout: post
title: Profile
search_exclude: true
permalink: /profile/
author: Kanhay Patil
---

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Set Profile Picture</title>
  <style>
    /* Basic styling for the modal */
    .modal {
      display: none; /* Hidden by default */
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.8); /* Semi-transparent black background */
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
    .modal-content {
      background: blue;
      padding: 20px;
      border-radius: 8px;
      text-align: center;
      width: 90%;
      max-width: 600px;
    }
    video {
      width: 100%;
      max-width: 100%;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-bottom: 20px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 10px;
    }
    img {
      width: 100%;
      max-width: 100%;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <h1>Set Your Profile Picture</h1>
  <button id="OpenOptionsModal">Set Profile Picture</button>

  <!-- Options Modal -->
  <div id="optionsModal" class="modal">
    <div class="modal-content">
      <h2>Choose an Option</h2>
      <button id="UploadPhoto">Upload a Photo</button>
      <button id="TakePictureOption">Take a Picture</button>
      <button id="CloseOptionsModal">Cancel</button>
    </div>
  </div>

  <!-- Webcam Modal -->
  <div id="webcamModal" class="modal">
    <div class="modal-content">
      <video id="webcam" autoplay playsinline></video>
      <button id="CloseWebcam">Close Webcam</button>
      <button id="TakePicture">Take the Picture</button>
      <button id="UsePicture" style="display: none;">Use Picture</button>
      <button id="RetakePhoto" style="display: none;">Retake Photo</button>
      <div id="ImagePreviewContainer"></div>
    </div>
  </div>

  <script>
    const openOptionsModalButton = document.getElementById('OpenOptionsModal');
    const optionsModal = document.getElementById('optionsModal');
    const uploadPhotoButton = document.getElementById('UploadPhoto');
    const takePictureOptionButton = document.getElementById('TakePictureOption');
    const closeOptionsModalButton = document.getElementById('CloseOptionsModal');

    const webcamModal = document.getElementById('webcamModal');
    const openWebcamButton = document.getElementById('OpenWebcam');
    const closeWebcamButton = document.getElementById('CloseWebcam');
    const videoElement = document.getElementById('webcam');
    const takePictureButton = document.getElementById('TakePicture');
    const usePictureButton = document.getElementById('UsePicture');
    const retakePhotoButton = document.getElementById('RetakePhoto');

    let webcamStream = null;
    let capturedImage = null; // Variable to store the captured image

    // Open the options modal
    openOptionsModalButton.addEventListener('click', () => {
      optionsModal.style.display = 'flex';
    });

    // Close the options modal
    closeOptionsModalButton.addEventListener('click', () => {
      optionsModal.style.display = 'none';
    });

    // Choose "Upload a Photo" option
uploadPhotoButton.addEventListener('click', () => {
  const input = document.createElement('input');
  input.type = 'file';
  input.accept = 'image/*';
  input.onchange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        capturedImage = e.target.result; // Save the uploaded image as Base64
        alert("Picture Successfully Uploaded");
        console.log(capturedImage); // Debugging
      };
      reader.readAsDataURL(file);
    }
  };
  input.click();
  optionsModal.style.display = 'none'; // Close the options modal
});


    // Choose "Take a Picture" option
    takePictureOptionButton.addEventListener('click', async () => {
      optionsModal.style.display = 'none'; // Close the options modal
      try {
        webcamStream = await navigator.mediaDevices.getUserMedia({ video: true });
        videoElement.srcObject = webcamStream;
        webcamModal.style.display = 'flex'; // Open the webcam modal
      } catch (error) {
        alert('Error accessing webcam: ' + error.message);
      }
    });

    // Close the webcam modal
    closeWebcamButton.addEventListener('click', () => {
      if (webcamStream) {
        const tracks = webcamStream.getTracks();
        tracks.forEach(track => track.stop()); // Stop all tracks
        webcamStream = null;
      }
      videoElement.srcObject = null; // Clear the video element
      webcamModal.style.display = 'none';
      resetModal();
    });

    // Take picture
    takePictureButton.addEventListener('click', () => {
      const canvas = document.createElement('canvas');
      canvas.width = videoElement.videoWidth;
      canvas.height = videoElement.videoHeight;

      const context = canvas.getContext('2d');
      context.drawImage(videoElement, 0, 0, canvas.width, canvas.height);

      capturedImage = canvas.toDataURL('image/png');
      const imagePreview = document.createElement('img');
      imagePreview.src = capturedImage;

      videoElement.style.display = 'none';
      videoElement.insertAdjacentElement('afterend', imagePreview);

      stopWebcamStream();
      takePictureButton.style.display = 'none';
      usePictureButton.style.display = 'inline-block';
      retakePhotoButton.style.display = 'inline-block';
    });

    // Use the captured picture
    usePictureButton.addEventListener('click', () => {
      alert('Picture used!');
      console.log(capturedImage);
      webcamModal.style.display = 'none';
    });

    // Retake the photo
    retakePhotoButton.addEventListener('click', async () => {
      const imagePreview = document.querySelector('img');
      if (imagePreview) imagePreview.remove();

      webcamStream = await navigator.mediaDevices.getUserMedia({ video: true });
      videoElement.srcObject = webcamStream;

      takePictureButton.style.display = 'inline-block';
      usePictureButton.style.display = 'none';
      retakePhotoButton.style.display = 'none';

      videoElement.style.display = 'block';
    });

    // Reset the webcam modal
    function resetModal() {
      takePictureButton.style.display = 'inline-block';
      usePictureButton.style.display = 'none';
      retakePhotoButton.style.display = 'none';
      videoElement.style.display = 'block';
    }

    // Stop the webcam stream
    function stopWebcamStream() {
      if (webcamStream) {
        const tracks = webcamStream.getTracks();
        tracks.forEach(track => track.stop());
        webcamStream = null;
      }
    }
    usePictureButton.addEventListener('click', () => {
  if (!capturedImage) {
    alert('No picture selected or captured!');
    return;
  }
fetch('/upload', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ image: capturedImage }),
})
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response.json();
  })
  .then((data) => {
    console.log('Upload successful:', data);
    alert('Image uploaded successfully!');
  })
  .catch((error) => {
    console.error('Error uploading image:', error.message);
    alert('Error uploading image. Please try again.');
  });


  </script>
</body>
</html>
