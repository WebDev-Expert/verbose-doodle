<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Take a Photo</title>
    <style>
        #video {
            width: 100%;
            max-width: 640px;
        }
        #photo {
            display: block;
            margin-top: 10px;
            max-width: 100%;
        }
    </style>
</head>
<body>
    <h1>Take a Photo</h1>
    <button id="take-photo">Take Photo</button>
    <video id="video" autoplay></video>
    <canvas id="canvas" style="display:none;"></canvas>
    <img id="photo" alt="Your Photo" />

    <script>
        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const photo = document.getElementById('photo');
        const takePhotoButton = document.getElementById('take-photo');
        const context = canvas.getContext('2d');

        // Access the webcam
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
                video.srcObject = stream;
                video.play();
            })
            .catch(err => {
                console.error("Error accessing the camera: ", err);
            });

        // Capture photo
        takePhotoButton.addEventListener('click', () => {
            // Set canvas size to video size
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            
            // Draw the video frame onto the canvas
            context.drawImage(video, 0, 0, canvas.width, canvas.height);
            
            // Get the data URL of the image
            const dataURL = canvas.toDataURL('image/png');
            
            // Set the data URL as the source of the image
            photo.setAttribute('src', dataURL);
        });
    </script>
</body>
</html>
