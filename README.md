<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IKIY GASIMA - Fast Downloader</title>
    <style>
        :root { --primary: #00f2ea; --secondary: #ff0050; --dark: #121212; --card: #1e1e1e; }
        body { background: var(--dark); color: white; font-family: 'Segoe UI', Tahoma, sans-serif; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; padding: 20px; box-sizing: border-box; }
        .container { background: var(--card); padding: 30px; border-radius: 25px; width: 100%; max-width: 450px; text-align: center; border: 1px solid #333; box-shadow: 0 20px 50px rgba(0,0,0,0.8); }
        h2 { margin-bottom: 10px; font-size: 28px; font-weight: 800; background: linear-gradient(to right, var(--primary), var(--secondary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        p.subtitle { font-size: 12px; opacity: 0.6; margin-bottom: 25px; }
        .input-group { position: relative; margin-bottom: 20px; }
        input { width: 100%; padding: 18px; border-radius: 15px; border: 2px solid #333; background: #252525; color: white; outline: none; box-sizing: border-box; font-size: 14px; transition: 0.3s; }
        input:focus { border-color: var(--primary); box-shadow: 0 0 10px rgba(0,242,234,0.2); }
        button { width: 100%; padding: 16px; border-radius: 15px; border: none; background: linear-gradient(45deg, var(--primary), var(--secondary)); color: white; font-weight: bold; font-size: 16px; cursor: pointer; transition: 0.3s; text-transform: uppercase; letter-spacing: 1px; }
        button:active { transform: scale(0.98); }
        button:disabled { opacity: 0.6; cursor: not-allowed; }
        #result { display: none; margin-top: 30px; padding-top: 20px; border-top: 1px solid #333; animation: slideUp 0.5s ease; }
        .video-title { font-size: 14px; color: #ccc; margin-bottom: 20px; line-height: 1.4; font-style: italic; }
        .dl-btn { display: block; text-decoration: none; padding: 15px; margin: 10px 0; border-radius: 12px; font-size: 14px; font-weight: bold; color: white; background: #2a2a2a; border: 1px solid #444; transition: 0.3s; }
        .dl-btn:hover { background: #333; border-color: var(--primary); }
        .wm-footer { margin-top: 40px; font-size: 10px; opacity: 0.4; letter-spacing: 4px; font-weight: bold; color: #fff; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

<div class="container">
    <h2>IKIY DOWNLOADER</h2>
    <p class="subtitle">Download Video TikTok Tanpa Watermark 100% Work</p>
    
    <div class="input-group">
        <input type="text" id="videoUrl" placeholder="Tempel link video di sini...">
    </div>
    
    <button id="getBtn" onclick="processDownload()">MENGUNDUH VIDEO</button>

    <div id="result">
        <div id="videoTitle" class="video-title"></div>
        <a id="hdLink" class="dl-btn" style="background: rgba(0,242,234,0.1); border-color: var(--primary);" href="#" target="_blank">📥 DOWNLOAD HD (NO WM)</a>
        <a id="sdLink" class="dl-btn" href="#" target="_blank">📥 DOWNLOAD SERVER 2</a>
        <a id="musicLink" class="dl-btn" style="background: rgba(255,0,80,0.1); border-color: var(--secondary);" href="#" target="_blank">🎵 DOWNLOAD AUDIO (MP3)</a>
    </div>

    <div class="wm-footer">DOWNLOADER BY IKIY GASIMA</div>
</div>

<script>
    async function processDownload() {
        const urlInput = document.getElementById('videoUrl').value.trim();
        const btn = document.getElementById('getBtn');
        const resultDiv = document.getElementById('result');

        if (!urlInput) {
            alert("Kiy, linknya diisi dulu dong!");
            return;
        }

        btn.disabled = true;
        btn.innerText = "SEDANG MEMPROSES...";
        resultDiv.style.display = "none";

        try {
            // Menggunakan API TikWM Gratis
            const apiUrl = `https://www.tikwm.com/api/?url=${encodeURIComponent(urlInput)}`;
            const response = await fetch(apiUrl);
            const resData = await response.json();

            if (resData.code === 0 && resData.data) {
                const videoData = resData.data;
                const base = "https://www.tikwm.com";

                // Menampilkan Judul
                document.getElementById('videoTitle').innerText = videoData.title || "Video berhasil ditemukan!";

                // Menyiapkan Link Download dengan perbaikan "Double Link"
                const fixLink = (link) => {
                    if (!link) return "#";
                    return link.startsWith('http') ? link : base + link;
                };

                document.getElementById('hdLink').href = fixLink(videoData.hdplay || videoData.play);
                document.getElementById('sdLink').href = fixLink(videoData.play);
                document.getElementById('musicLink').href = fixLink(videoData.music);

                // Tampilkan Hasil
                resultDiv.style.display = "block";
                btn.innerText = "BERHASIL DISEDIAKAN!";
            } else {
                alert("Gagal ambil data! Pastikan link TikTok benar.");
                btn.innerText = "MENGUNDUH VIDEO";
            }
        } catch (error) {
            console.error(error);
            alert("Terjadi masalah koneksi ke server API!");
            btn.innerText = "MENGUNDUH VIDEO";
        } finally {
            btn.disabled = false;
        }
    }
</script>

</body>
</html>

