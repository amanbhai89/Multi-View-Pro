<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multi View Pro</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" integrity="sha512-Fo3rlrZj/k7ujTnHg4CGR2D7kSs0v4LLanw2qksYuRlEzO+tcaEPQogQ0KaoGN26/zrn20ImR1DfuLWnOo7aBA==" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <style>
        :root {
            --primary-color: #3B82F6; --primary-hover-color: #2563EB;
            --secondary-color: #F59E0B; --secondary-hover-color: #D97706;
            --danger-color: #EF4444; --danger-hover-color: #DC2626;
            --success-color: #10B981; --success-hover-color: #059669;
            --neutral-bg: #F9FAFB; --container-bg: #FFFFFF;
            --text-primary: #1F2937; --text-secondary: #6B7280;
            --border-color: #E5E7EB; --input-bg: #F3F4F6;
            --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
            --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --border-radius: 8px;
            --youtube-color: #FF0000; --instagram-color: #E4405F; 
        }

        *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: 'Inter', sans-serif; background-color: var(--neutral-bg);
            color: var(--text-primary); line-height: 1.6; padding: 15px;
            display: flex; flex-direction: column; align-items: center; min-height: 100vh;
            position: relative; 
        }

        #rgb-sound-border {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            pointer-events: none; z-index: 9999; 
            border: 4px solid transparent;
            box-sizing: border-box; display: none; 
            animation: rgb-border-animation 5s linear infinite;
        }

        @keyframes rgb-border-animation {
            0%   { border-color: hsl(0, 100%, 50%); } 16%  { border-color: hsl(60, 100%, 50%); }
            33%  { border-color: hsl(120, 100%, 50%); } 50%  { border-color: hsl(180, 100%, 50%); }
            66%  { border-color: hsl(240, 100%, 50%); } 83%  { border-color: hsl(300, 100%, 50%); }
            100% { border-color: hsl(360, 100%, 50%); }
        }

        .modal {
            display: none; position: fixed; z-index: 1001;
            left: 0; top: 0; width: 100%; height: 100%;
            overflow: auto; background-color: rgba(0,0,0,0.65); backdrop-filter: blur(4px);
        }
        .modal-content {
            background-color: var(--container-bg); margin: 15% auto; padding: 25px;
            border: none; width: 90%; max-width: 450px;
            border-radius: var(--border-radius); box-shadow: var(--shadow-md);
            position: relative; text-align: center;
        }
        .close-button {
            color: var(--text-secondary); position: absolute; top: 10px; right: 15px;
            font-size: 30px; font-weight: 300; line-height: 1;
        }
        .close-button:hover, .close-button:focus { color: var(--text-primary); text-decoration: none; cursor: pointer; }
        .modal-content h2 { margin-top: 0; color: var(--text-primary); margin-bottom: 15px; font-weight:600; font-size: 1.4em; }
        .modal-content p { color: var(--text-secondary); margin-bottom: 15px; font-size: 0.9em; }
        .modal-content p strong { color: var(--text-primary); font-weight: 500; }
        .modal-content #qrCodeImg { max-width: 160px; height: auto; margin: 10px auto; display: block; border-radius: 4px; }
        .modal-content .donation-text { font-weight: 600; margin-top: 10px; color: var(--text-primary); font-size: 1em; margin-bottom: 15px; }
        
        .social-links-container {
            margin-top: 15px; padding-top: 15px; border-top: 1px solid var(--border-color);
            display: flex; justify-content: center; gap: 20px;
        }
        .social-button {
            display: inline-flex; align-items: center; justify-content: center;
            width: 40px; height: 40px; border-radius: 50%; color: white;
            font-size: 18px; text-decoration: none;
            transition: transform 0.2s, opacity 0.2s; box-shadow: var(--shadow-sm);
        }
        .social-button:hover { transform: translateY(-2px); opacity: 0.9; }
        .social-button.youtube { background-color: var(--youtube-color); }
        .social-button.instagram { background: radial-gradient(circle at 30% 107%, #fdf497 0%, #fdf497 5%, #fd5949 45%,#d6249f 60%,#285AEB 90%); }

        .main-content-wrapper {
            flex-grow: 1; width: 100%; display: flex; flex-direction: column; align-items: center;
        }
        .container {
            background-color: var(--container-bg); padding: 20px; border-radius: var(--border-radius);
            box-shadow: var(--shadow-md); width: 100%; max-width: 650px; box-sizing: border-box;
            margin-bottom: 20px; 
        }
        .header-container {
            display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;
        }
        h1 {
            color: var(--text-primary); margin: 0; flex-grow: 1;
            font-size: 1.6rem; font-weight: 700; text-align: center;
        }
        #settingsBtn {
            background-color: transparent; color: var(--text-secondary); border: none;
            border-radius: 50%; width: 38px; height: 38px;
            font-size: 20px; line-height: 38px; text-align: center;
            cursor: pointer; transition: color 0.2s, background-color 0.2s;
            margin-left: 10px; flex-shrink: 0;
        }
        #settingsBtn:hover { color: var(--primary-color); background-color: #E0E7FF; }

        .input-group { margin-bottom: 18px; }
        label { display: block; margin-bottom: 6px; font-weight: 500; color: var(--text-primary); font-size:0.85rem;}
        input[type="url"], input[type="number"], select {
            width: 100%; padding: 10px 12px; border: 1px solid var(--border-color);
            border-radius: var(--border-radius); box-sizing: border-box; font-size: 0.95rem;
            background-color: var(--input-bg); color: var(--text-primary);
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        input[type="url"]:focus, input[type="number"]:focus, select:focus {
            outline: none; border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.3); background-color: var(--container-bg);
        }
        .url-input-wrapper { display: flex; align-items: stretch; gap: 8px; }
        .url-input-wrapper input[type="url"] { flex-grow: 1; }
        
        #pasteBtn {
            padding: 10px 12px; background-color: var(--input-bg);
            color: var(--text-secondary); border: 1px solid var(--border-color);
            border-radius: var(--border-radius); cursor: pointer;
            font-size: 0.85rem; font-weight: 500;
            transition: background-color 0.2s, color 0.2s;
            white-space: nowrap;
        }
        #pasteBtn:hover { background-color: var(--border-color); color: var(--text-primary); }

        .controls-grid {
            display: grid; grid-template-columns: 1fr; gap: 15px; margin-top: 15px;
        }
        .auto-refresh-controls { 
            display: flex; gap: 10px; align-items: center; width: 100%;
        }
        .button-container { display: flex; gap: 10px; width: 100%; }
        
        button {
            padding: 10px 15px; border: none; border-radius: var(--border-radius);
            font-size: 0.9rem; font-weight: 600; cursor: pointer;
            transition: background-color 0.2s, transform 0.1s;
            text-align: center; display: inline-flex;
            align-items: center; justify-content: center;
            flex-grow: 1;
        }
        button:active { transform: translateY(1px); }

        #autoRefreshToggleBtn {
            background-color: var(--danger-color); color: white; 
            font-size: 0.85rem; flex-grow: 0; min-width: 180px;
        }
        #autoRefreshToggleBtn.inactive { background-color: var(--success-color); }
        #autoRefreshToggleBtn.inactive:hover { background-color: var(--success-hover-color); }
        #autoRefreshToggleBtn:not(.inactive):hover { background-color: var(--danger-hover-color); }

        #countdownDisplay {
            font-size: 0.85rem; font-weight: 500; color: var(--text-secondary);
            text-align: center;
            background-color: var(--input-bg); padding: 10px 12px; border-radius: var(--border-radius);
            flex-grow: 1;
        }
        #openBtn { background-color: var(--primary-color); color: white; }
        #openBtn:hover { background-color: var(--primary-hover-color); }
        #reloadBtn { background-color: var(--secondary-color); color: white; }
        #reloadBtn:hover { background-color: var(--secondary-hover-color); }

        .info-text {
            text-align: center; font-size: 0.8rem; color: var(--text-secondary); margin-top: 12px;
        }
        .info-text.youtube-api-info { margin-top: 8px; font-style: normal; }
        .info-text.disclaimer { color: #D97706; font-weight:500; margin-top: 18px; font-size:0.75rem; }

        #views-container {
            display: grid; gap: 10px; margin-top: 0; width: 100%; max-width: 1280px; padding-bottom: 20px;
        }
        .player-wrapper {
            position: relative; width: 100%; aspect-ratio: 16 / 9;
            border: 2px solid transparent; border-radius: var(--border-radius); overflow: hidden;
            background-color: #000; box-shadow: var(--shadow-sm);
            transition: border-color 0.3s, box-shadow 0.3s;
        }
        .player-wrapper.active-audio {
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.4), var(--shadow-md);
        }
        .view-iframe, .yt-player-div {
            width: 100%; height: 100%; border: none; border-radius: calc(var(--border-radius) - 2px);
        }
        .player-overlay {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            cursor: pointer; z-index: 10; background-color: rgba(0,0,0,0);
            display: flex; align-items: center; justify-content: center;
            opacity: 0; transition: background-color 0.3s, opacity 0.3s;
        }
        .player-wrapper:hover .player-overlay { background-color: rgba(0,0,0,0.4); opacity: 1; }
        .player-overlay::after {
            content: "🔊 Activate Sound"; background-color: rgba(0,0,0,0.7);
            color: white; padding: 6px 10px; border-radius: 6px;
            font-size: 0.8rem; font-weight: 500; text-shadow: 1px 1px 2px black;
        }
        .player-wrapper.active-audio .player-overlay::after { content: "🔊 Sound Active"; }

        #views-container.cols-1 { grid-template-columns: 1fr; }
        #views-container.cols-2 { grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); }
        
        @media (min-width: 768px) {
            body { padding: 20px; }
            .container { padding: 30px; margin-bottom: 30px; }
            h1 { font-size: 1.75rem; }
            #settingsBtn { width: 40px; height: 40px; font-size: 22px; line-height: 40px; }
            label { font-size:0.9rem; }
            input[type="url"], input[type="number"], select { font-size: 1rem; padding: 12px 15px; }
            #pasteBtn { font-size: 0.9rem; padding: 10px 15px;}
            button { font-size: 0.95rem; padding: 12px 20px; }
            #autoRefreshToggleBtn { font-size: 0.9rem; }
            #countdownDisplay { font-size: 0.9rem; text-align: left; }
            .info-text { font-size: 0.85rem; margin-top: 15px;}
            .info-text.disclaimer { font-size:0.8rem; margin-top: 20px; }
            .player-overlay::after { font-size: 0.85rem; padding: 8px 12px;}
            #views-container { gap: 15px; }
            #views-container.cols-3 { grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); }
            #views-container.cols-4 { grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); }
            .controls-grid {
                 grid-template-columns: auto 1fr; align-items: center;
            }
            .auto-refresh-controls { justify-content: flex-start; margin-bottom:0; }
            .button-container { margin-top: 0; }
        }
        
        @media (max-width: 480px) {
             h1 { font-size: 1.4rem; }
             .container { padding: 15px; }
             #pasteBtn { padding: 10px 8px; font-size: 0.8rem;}
             button { font-size: 0.85rem; padding: 10px 12px; }
             #autoRefreshToggleBtn { font-size: 0.8rem; min-width: 160px;}
             #countdownDisplay { font-size: 0.8rem; }
             .auto-refresh-controls { flex-direction: column; gap: 8px;}
        }

        .site-footer {
            width: 100%; background-color: var(--container-bg); color: var(--text-secondary);
            padding: 20px; text-align: center; font-size: 0.8rem;
            border-top: 1px solid var(--border-color); margin-top: auto;
        }
        .footer-links {
            list-style: none; padding: 0; margin: 0 0 8px 0; display: flex;
            justify-content: center; flex-wrap: wrap; gap: 8px 15px;
        }
        .footer-links li a { color: var(--text-secondary); text-decoration: none; transition: color 0.2s; }
        .footer-links li a:hover { color: var(--primary-color); text-decoration: underline; }
        .footer-copyright { font-size: 0.75rem; }

        /* Adsterra Ad Container */
        .adsterra-banner-container {
            width: 100%;
            margin-top: 20px;
            text-align: center;
            padding-bottom: 15px;
            /* Adsterra ads often inject their own styles, so keep this minimal */
            /* You might need to adjust if the ad doesn't center or has too much margin */
            display: flex; /* Helps in centering the ad if it's an iframe */
            justify-content: center;
            align-items: center;
        }

    </style>
</head>
<body>
    <div id="rgb-sound-border"></div>

    <div id="aboutModal" class="modal">
        <div class="modal-content">
            <span class="close-button" id="closeAboutModal">×</span>
            <h2>About this App</h2>
            <p>This Apk is Developed By Aman Bhai.</p>
            <p><strong>UPI ID:</strong> 8967295143@fam</p>
            <img id="qrCodeImg" src="https://i.imgur.com/C3FuECH.jpeg" alt="UPI QR Code">
            <p class="donation-text">Scan and Donate Please 🙏</p>
            <div class="social-links-container">
                <a href="https://youtube.com/@djamanpanjiparabastino.1dj?si=3LmI2jitRG_UCdWR" target="_blank" rel="noopener noreferrer" class="social-button youtube" title="YouTube"><i class="fab fa-youtube"></i></a>
                <a href="https://www.instagram.com/aman_ansari___143?igsh=MW0ybmk2NW8zbzNzOA==" target="_blank" rel="noopener noreferrer" class="social-button instagram" title="Instagram"><i class="fab fa-instagram"></i></a>
            </div>
        </div>
    </div>

    <div class="main-content-wrapper">
        <div class="container">
            <div class="header-container">
                <h1>Multi View Pro</h1>
                <button id="settingsBtn" title="Settings">⚙️</button>
            </div>
            <div class="input-group">
                <label for="urlInput">Enter YouTube URL (or other web address):</label>
                <div class="url-input-wrapper">
                    <input type="url" id="urlInput" placeholder="e.g., https://www.youtube.com/watch?v=...">
                    <button id="pasteBtn" title="Paste from clipboard">Paste</button>
                </div>
            </div>
            <div class="input-group">
                <label for="screenCount">Number of Views (1-12):</label>
                <input type="number" id="screenCount" value="2" min="1" max="12">
            </div>
            
            <div class="controls-grid">
                <div class="auto-refresh-controls">
                    <button id="autoRefreshToggleBtn">Stop Automatic Refresh</button>
                    <span id="countdownDisplay">Refresh: ON</span>
                </div>
                <div class="button-container">
                    <button id="openBtn">Open Views</button>
                    <button id="reloadBtn">Reload All Views Now</button>
                </div>
            </div>

            <p class="info-text youtube-api-info">For YouTube videos, click on a view to activate its audio. Only one view will have sound.</p>
            <p class="info-text disclaimer">Disclaimer: This tool is for personal, educational, or testing purposes. Using it to artificially inflate views may violate platform Terms of Service. View counts on platforms like YouTube/Instagram may not increase as expected. Use responsibly.</p>
        </div>
        <div id="views-container"></div>
    </div> 

    <footer class="site-footer">
        <ul class="footer-links">
            <li><a href="https://djamanbastino1.blogspot.com/2025/05/about-us.html" target="_blank" rel="noopener noreferrer">About Us</a></li>
            <li><a href="https://djamanbastino1.blogspot.com/2025/05/contact-us.html" target="_blank" rel="noopener noreferrer">Contact Us</a></li>
            <li><a href="#">Privacy Policy</a></li>
            <li><a href="#">DMCA</a></li>
        </ul>
        <p class="footer-copyright">© <span id="currentYear"></span> Multi View Pro. All Rights Reserved.</p>
    </footer>

    <!-- Adsterra Banner Ad Container -->
    <div class="adsterra-banner-container">
        <script type="text/javascript">
            atOptions = {
                'key' : '31bc6e815659fbebd501bdf56af2537a',
                'format' : 'iframe',
                'height' : 60,
                'width' : 468,
                'params' : {}
            };
        </script>
        <script type="text/javascript" src="//trucklowest.com/31bc6e815659fbebd501bdf56af2537a/invoke.js"></script>
    </div>


    <script>
        const urlInput = document.getElementById('urlInput');
        const screenCountInput = document.getElementById('screenCount');
        const openBtn = document.getElementById('openBtn');
        const reloadBtn = document.getElementById('reloadBtn');
        const pasteBtn = document.getElementById('pasteBtn');
        const viewsContainer = document.getElementById('views-container');
        const autoRefreshToggleBtn = document.getElementById('autoRefreshToggleBtn');
        const countdownDisplay = document.getElementById('countdownDisplay');
        
        const settingsBtn = document.getElementById('settingsBtn');
        const aboutModal = document.getElementById('aboutModal');
        const closeAboutModalBtn = document.getElementById('closeAboutModal');
        const currentYearSpan = document.getElementById('currentYear');
        const rgbSoundBorder = document.getElementById('rgb-sound-border');

        let ytPlayers = {};
        let activeAudioPlayerId = null;
        let isYouTubeApiReady = false;
        let queuedPlayerCreations = [];

        const AUTO_REFRESH_INTERVAL_SECONDS = 60;
        let countdownValue = AUTO_REFRESH_INTERVAL_SECONDS;
        let countdownTimerId = null;
        let isAutoRefreshActive = true;

        var tag = document.createElement('script');
        tag.src = "https://www.youtube.com/iframe_api";
        var firstScriptTag = document.getElementsByTagName('script')[0];
        firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

        window.onYouTubeIframeAPIReady = function() {
            isYouTubeApiReady = true;
            queuedPlayerCreations.forEach(queueItem => createYouTubePlayer(queueItem.playerId, queueItem.videoId));
            queuedPlayerCreations = [];
        };

        function showRgbBorder() { if (rgbSoundBorder) rgbSoundBorder.style.display = 'block'; }
        function hideRgbBorder() { if (rgbSoundBorder) rgbSoundBorder.style.display = 'none'; }

        function getYouTubeVideoId(url) {
            const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
            const match = url.match(regExp);
            return (match && match[2].length === 11) ? match[2] : null;
        }

        function createYouTubePlayer(playerId, videoId) {
            if (!isYouTubeApiReady) {
                queuedPlayerCreations.push({ playerId, videoId });
                return;
            }
            let shouldBeMuted = true;
            if (playerId === activeAudioPlayerId) {
                 shouldBeMuted = false;
            }
            ytPlayers[playerId] = new YT.Player(playerId, {
                height: '100%', width: '100%', videoId: videoId,
                playerVars: {
                    'autoplay': 1, 'mute': shouldBeMuted ? 1 : 0,
                    'controls': 1, 'playsinline': 1, 'loop': 1, 'playlist': videoId,
                    'modestbranding': 1, 'rel': 0
                },
                events: { 'onReady': onPlayerReady }
            });
        }

        function onPlayerReady(event) {
            const player = event.target;
            const playerId = player.getIframe().id;
            if (playerId === activeAudioPlayerId) {
                if(player.isMuted()) player.unMute();
                showRgbBorder();
            } else {
                if(!player.isMuted()) player.mute();
            }
            player.playVideo();
        }
        
        function handlePlayerClick(playerId) {
            if (!ytPlayers[playerId]) return;
            if (activeAudioPlayerId && activeAudioPlayerId !== playerId && ytPlayers[activeAudioPlayerId]) {
                if(typeof ytPlayers[activeAudioPlayerId].mute === 'function') ytPlayers[activeAudioPlayerId].mute();
                document.getElementById(activeAudioPlayerId)?.closest('.player-wrapper')?.classList.remove('active-audio');
            }
            activeAudioPlayerId = playerId;
            if(typeof ytPlayers[activeAudioPlayerId].unMute === 'function') ytPlayers[activeAudioPlayerId].unMute();
            if(typeof ytPlayers[activeAudioPlayerId].playVideo === 'function') ytPlayers[activeAudioPlayerId].playVideo();
            document.getElementById(activeAudioPlayerId)?.closest('.player-wrapper')?.classList.add('active-audio');
            showRgbBorder();
        }
        
        pasteBtn.addEventListener('click', async () => {
            try {
                const text = await navigator.clipboard.readText();
                urlInput.value = text;
            } catch (err) { alert('Could not paste. Ensure HTTPS.'); }
        });

        openBtn.addEventListener('click', () => {
            const url = urlInput.value.trim();
            let count = parseInt(screenCountInput.value);

            if (!url) { alert('Please enter a URL.'); return; }
            if (isNaN(count) || count < 1 || count > 12) { alert('Enter views (1-12).'); return; }

            viewsContainer.innerHTML = ''; 
            ytPlayers = {}; 
            let tempActivePlayerIdCandidate = null;

            viewsContainer.className = 'cols-' + Math.min(count, 4);
             if (count === 1) viewsContainer.className = 'cols-1';
             else if (count <= 4) viewsContainer.className = 'cols-2';
             else if (count <= 9) viewsContainer.className = 'cols-3';
             else viewsContainer.className = 'cols-4';

            const videoId = getYouTubeVideoId(url);
            for (let i = 0; i < count; i++) {
                const playerWrapper = document.createElement('div');
                playerWrapper.classList.add('player-wrapper');
                const uniquePlayerId = `ytplayer-${Date.now()}-${i}`;
                if (videoId) {
                    if (i === 0) tempActivePlayerIdCandidate = uniquePlayerId;
                    const playerDiv = document.createElement('div');
                    playerDiv.id = uniquePlayerId;
                    playerDiv.classList.add('yt-player-div');
                    const overlay = document.createElement('div');
                    overlay.classList.add('player-overlay');
                    overlay.addEventListener('click', () => handlePlayerClick(uniquePlayerId));
                    playerWrapper.appendChild(playerDiv);
                    playerWrapper.appendChild(overlay);
                    viewsContainer.appendChild(playerWrapper);
                    if (i === 0) activeAudioPlayerId = tempActivePlayerIdCandidate;
                    createYouTubePlayer(uniquePlayerId, videoId);
                } else {
                    const iframe = document.createElement('iframe');
                    iframe.classList.add('view-iframe');
                    iframe.setAttribute('allowfullscreen', '');
                    iframe.setAttribute('allow', 'autoplay; encrypted-media; picture-in-picture');
                    iframe.src = url;
                    playerWrapper.appendChild(iframe);
                    viewsContainer.appendChild(playerWrapper);
                }
            }
            
            if (!videoId || count === 0) {
                activeAudioPlayerId = null; 
                hideRgbBorder();
            } else if (activeAudioPlayerId && ytPlayers[activeAudioPlayerId]) {
                 setTimeout(() => {
                    if (activeAudioPlayerId && ytPlayers[activeAudioPlayerId]) {
                         handlePlayerClick(activeAudioPlayerId); 
                    }
                }, 1000);
            }

            if (isAutoRefreshActive) startAutoRefresh();
            else stopAutoRefresh();
        });

        function performReloadAllViews(isAutoTriggered = false) {
            const regularIframes = viewsContainer.querySelectorAll('.view-iframe:not(.yt-player-div iframe)');
            let reloadedCount = 0;
            let activePlayerStillUnmuted = false;

            if (Object.keys(ytPlayers).length === 0 && regularIframes.length === 0) {
                if (!isAutoTriggered) alert('No views to reload.');
                hideRgbBorder(); return false;
            }
            regularIframes.forEach(iframe => {
                if (iframe.src && iframe.src !== 'about:blank' && iframe.src.startsWith('http')) {
                    const currentSrc = iframe.src; iframe.src = ''; iframe.src = currentSrc;
                    reloadedCount++;
                }
            });
            Object.keys(ytPlayers).forEach(playerId => {
                const player = ytPlayers[playerId];
                if (player && typeof player.getVideoData === 'function') {
                    const videoData = player.getVideoData();
                    if (videoData && videoData.video_id) {
                        player.cueVideoById(videoData.video_id); player.playVideo();
                        if (playerId !== activeAudioPlayerId) {
                            if(!player.isMuted()) player.mute();
                        } else {
                            if(player.isMuted()) player.unMute();
                            activePlayerStillUnmuted = true;
                        }
                        reloadedCount++;
                    }
                }
            });
            
            if(activePlayerStillUnmuted) showRgbBorder();
            else hideRgbBorder();

            if (!isAutoTriggered) alert(reloadedCount + ' view(s) reloaded/restarted.');
            else console.log(reloadedCount + ' view(s) auto-reloaded at ' + new Date().toLocaleTimeString());
            return true;
        }

        reloadBtn.addEventListener('click', () => performReloadAllViews(false));

        function updateCountdownDisplay() {
            if (isAutoRefreshActive && (Object.keys(ytPlayers).length > 0 || viewsContainer.querySelectorAll('.view-iframe:not(.yt-player-div iframe)').length > 0)) {
                 countdownDisplay.textContent = `Next refresh: ${countdownValue}s`;
            } else if (isAutoRefreshActive){
                 countdownDisplay.textContent = `Open views to start refresh`;
            } else {
                 countdownDisplay.textContent = `Refresh paused`;
            }
        }

        function startCountdown() {
            stopCountdownInternal();
            countdownValue = AUTO_REFRESH_INTERVAL_SECONDS;
            updateCountdownDisplay();
            if (Object.keys(ytPlayers).length === 0 && viewsContainer.querySelectorAll('.view-iframe:not(.yt-player-div iframe)').length === 0) {
                countdownDisplay.textContent = `Open views to start refresh`;
                hideRgbBorder(); return;
            }
            if (activeAudioPlayerId && ytPlayers[activeAudioPlayerId] && typeof ytPlayers[activeAudioPlayerId].isMuted === 'function' && !ytPlayers[activeAudioPlayerId].isMuted()) {
                showRgbBorder();
            }
            countdownTimerId = setInterval(() => {
                countdownValue--;
                updateCountdownDisplay();
                if (countdownValue <= 0) {
                    performReloadAllViews(true); 
                    countdownValue = AUTO_REFRESH_INTERVAL_SECONDS;
                    updateCountdownDisplay();
                }
            }, 1000);
        }
        
        function stopCountdownInternal() { if (countdownTimerId) { clearInterval(countdownTimerId); countdownTimerId = null; } }
        function startAutoRefresh() { isAutoRefreshActive = true; startCountdown(); updateAutoRefreshButton(); }
        function stopAutoRefresh() { isAutoRefreshActive = false; stopCountdownInternal(); updateAutoRefreshButton(); updateCountdownDisplay(); hideRgbBorder(); }

        function updateAutoRefreshButton() {
            if (isAutoRefreshActive) {
                autoRefreshToggleBtn.textContent = 'Stop Automatic Refresh';
                autoRefreshToggleBtn.classList.remove('inactive');
            } else {
                autoRefreshToggleBtn.textContent = 'Start Automatic Refresh';
                autoRefreshToggleBtn.classList.add('inactive');
            }
        }

        autoRefreshToggleBtn.addEventListener('click', () => { if (isAutoRefreshActive) stopAutoRefresh(); else startAutoRefresh(); });
        settingsBtn.addEventListener('click', () => { aboutModal.style.display = 'block'; });
        closeAboutModalBtn.addEventListener('click', () => { aboutModal.style.display = 'none'; });
        window.addEventListener('click', (event) => { if (event.target == aboutModal) aboutModal.style.display = 'none'; });

        if(currentYearSpan) currentYearSpan.textContent = new Date().getFullYear();
        updateAutoRefreshButton();
        updateCountdownDisplay(); 
        hideRgbBorder();
    </script>
</body>
</html>
