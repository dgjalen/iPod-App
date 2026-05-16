<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>iPod Classic Interface</title>  
    <style>  
        :root {  
            --ipod-blue-gradient: linear-gradient(to bottom, #507cb9 0%, #325383 100%);  
            --header-gradient: #000000;   
            --scrubber-progress: linear-gradient(to bottom, #888888 0%, #555555 100%);  
            --lcd-bg: #000000;   
            --border-color: #333333;   
        }  
  
        body {  
            background-color: #1a1a1a;  
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;  
            display: flex;  
            justify-content: center;  
            align-items: center;  
            min-height: 100vh;  
            margin: 0;  
            user-select: none;  
            -webkit-user-select: none;  
        }  
  
        /* Borderless Direct Screen Interface Module */  
        .ipod-screen-viewport {  
            width: 320px;  
            height: 240px;  
            background-color: var(--lcd-bg);  
            overflow: hidden;  
            position: relative;  
            box-shadow: 0 20px 50px rgba(0,0,0,0.6);  
            border: 1px solid #222;  
        }  
  
        .screen-face {  
            position: absolute;  
            width: 100%;  
            height: 100%;  
            display: flex;  
            flex-direction: column;  
            box-sizing: border-box;  
            background-color: var(--lcd-bg);  
        }  
  
        #front-view { z-index: 2; }  
        #album-tracks-view { z-index: 1; display: none; }  
        #now-playing-view { z-index: 1; display: none; }  
  
        /* Navigation Systems Header */  
        .screen-header {  
            height: 22px;  
            background: var(--header-gradient);  
            border-bottom: 1px solid #000000;   
            display: flex;  
            justify-content: space-between;  
            align-items: center;  
            padding: 0 8px;  
            font-size: 11px;  
            font-weight: bold;  
            color: #ffffff;   
            box-sizing: border-box;  
            z-index: 10;   
        }  
  
        .header-center-title {  
            position: absolute;  
            left: 50%;  
            transform: translateX(-50%);  
            white-space: nowrap;  
            max-width: 160px;  
            overflow: hidden;  
            text-overflow: ellipsis;  
        }  
  
        .view-toggle-btn {  
            background: transparent;  
            border: none;  
            cursor: pointer;  
            font-size: 10px;  
            color: #ffffff;   
            font-weight: bold;  
            padding: 0;  
            text-decoration: underline;  
        }  
  
        /* Custom Vector Shuffle Button Layout */  
        .shuffle-toggle-btn {  
            background: transparent;  
            border: none;  
            cursor: pointer;  
            padding: 0;  
            margin-right: 8px;  
            display: flex;  
            align-items: center;  
            justify-content: center;  
            width: 16px;  
            height: 12px;  
        }  
  
        .shuffle-icon-svg {  
            width: 100%;  
            height: 100%;  
            fill: none;  
            stroke: #888888;   
            stroke-width: 2;  
            stroke-linecap: round;  
            stroke-linejoin: round;  
            transition: stroke 0.2s filter 0.2s;  
        }  
  
        .shuffle-toggle-btn.active .shuffle-icon-svg {  
            stroke: #ffffff;  
            filter: drop-shadow(0px 1px 1px rgba(0,0,0,0.15));  
        }  
  
        .header-right-group {  
            display: flex;  
            align-items: center;  
        }  
  
        .battery-icon {  
            width: 16px;  
            height: 8px;  
            border: 1px solid #ffffff;   
            border-radius: 1px;  
            position: relative;  
            padding: 1px;  
            box-sizing: border-box;  
        }  
  
        .battery-icon::after {  
            content: '';  
            position: absolute;  
            right: -3px;  
            top: 1.5px;  
            width: 2px;  
            height: 3px;  
            background-color: #ffffff;   
        }  
  
        .battery-level {  
            width: 85%;  
            height: 100%;  
            background-color: #4caf50;   
        }  
  
        .screen-body {  
            flex: 1;  
            position: relative;  
            overflow: hidden;  
            display: flex;  
            flex-direction: column;  
            background-color: #000000;   
        }  
  
        /* Large Cover Flow Visual Engine */  
        .coverflow-wrapper {  
            position: absolute;  
            top: 0;  
            left: 0;  
            width: 100%;  
            height: 218px;   
            background: #000000;   
            display: flex;  
            justify-content: center;  
            align-items: flex-start;  
            perspective: 350px;  
            overflow: hidden;  
            z-index: 1;  
            cursor: grab;  
        }  
  
        .coverflow-wrapper:active {  
            cursor: grabbing;  
        }  
  
        .coverflow-stage {  
            width: 100%;  
            height: 100%;   
            position: relative;  
            transform-style: preserve-3d;  
            display: flex;  
            justify-content: center;  
            align-items: center;  
            pointer-events: none;   
        }  
  
        .coverflow-item {  
            width: 115px;   
            height: 115px;   
            position: absolute;  
            transition: transform 0.25s cubic-bezier(0.25, 0.46, 0.45, 0.94);  
            cursor: pointer;  
            transform-style: preserve-3d;  
            opacity: 1 !important;   
            pointer-events: auto;   
        }  
  
        /* Large visible pseudo reflection element extending downwards */  
        .coverflow-item::after {  
            content: '';  
            position: absolute;  
            top: 100%;  
            left: 0;  
            width: 100%;  
            height: 80px;   
            background: inherit;  
            background-image: inherit;  
            transform: scaleY(-1);  
            opacity: 0.35;  
            mask-image: linear-gradient(to bottom, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0) 80%);  
            -webkit-mask-image: linear-gradient(to bottom, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0) 80%);  
            pointer-events: none;  
            transform-style: preserve-3d;  
        }  
  
        .coverflow-art {  
            width: 100%;  
            height: 100%;  
            object-fit: cover;  
            box-shadow: 0 6px 15px rgba(0,0,0,0.9);  
            opacity: 1 !important;   
            -webkit-box-reflect: below 2px linear-gradient(transparent 20%, rgba(255,255,255,0.45) 100%);  
        }  
  
        /* Bottom Control Info Zone Layout */  
        .coverflow-info-zone {  
            position: absolute;  
            bottom: 0;  
            left: 0;  
            width: 100%;  
            height: 48px;  
            background: rgba(0, 0, 0, 0.6);   
            border-top: 1px solid #000000;   
            display: flex;  
            justify-content: space-between;  
            align-items: center;  
            padding: 0 12px;  
            box-sizing: border-box;  
            z-index: 5;   
        }  
  
        .cf-meta-text-column {  
            flex: 1;  
            display: flex;  
            flex-direction: column;  
            justify-content: center;  
            align-items: center;  
            text-align: center;  
            padding: 0 10px;  
            overflow: hidden;  
        }  
  
        .cf-title {  
            font-size: 11px;  
            font-weight: bold;  
            color: #ffffff;   
            width: 100%;  
            white-space: nowrap;  
            overflow: hidden;  
            text-overflow: ellipsis;  
        }  
  
        .cf-artist-album {  
            font-size: 9px;  
            color: #b0c4de;   
            margin-top: 1px;  
            width: 100%;  
            white-space: nowrap;  
            overflow: hidden;  
            text-overflow: ellipsis;  
        }  
  
        /* Identical Symmetrical Circular Corner Controls System */  
        .control-ring-btn {  
            background: transparent;  
            border: none;  
            cursor: pointer;  
            padding: 0;  
            display: flex;  
            align-items: center;  
            justify-content: center;  
            width: 22px;  
            height: 22px;  
            flex-shrink: 0;  
            opacity: 0.75;   
            transition: opacity 0.2s ease, transform 0.1s ease;  
        }  
  
        .control-ring-btn:hover {  
            opacity: 1.0;  
        }  
  
        .control-ring-btn:active {  
            transform: scale(0.92);  
        }  
  
        .ring-icon-svg {  
            width: 100%;  
            height: 100%;  
            fill: none;  
            stroke: #ffffff;   
            stroke-width: 2;  
        }  
  
        /* Classic List Interface Pane */  
        .classic-list-wrapper {  
            position: absolute;  
            top: 0;  
            left: 0;  
            width: 100%;  
            height: 218px;  
            overflow-y: auto;  
            background: #000000;   
            display: none;  
            z-index: 2;  
        }  
  
        .list-item {  
            padding: 6px 12px;  
            font-size: 11px;  
            font-weight: bold;  
            color: #ffffff;   
            border-bottom: 1px solid #222222;   
            display: flex;  
            justify-content: space-between;  
            align-items: center;  
            cursor: pointer;  
        }  
  
        .list-item.selected {  
            background: var(--ipod-blue-gradient);  
            color: #fff;  
        }  
  
        .list-item.selected span {  
            color: #fff !important;  
        }  
  
        /* Black Track List View styles matching image */  
        .album-list-container {  
            flex: 1;  
            overflow-y: auto;  
            background: #000000;  
        }  
  
        .album-header-title {  
            padding: 4px 12px;  
            font-size: 11px;  
            font-weight: bold;  
            color: #507cb9;  
            background: #111111;  
            white-space: nowrap;  
            overflow: hidden;  
            text-overflow: ellipsis;  
        }  
  
        .track-row {  
            padding: 8px 12px;  
            font-size: 11px;  
            font-weight: bold;  
            color: #ffffff;  
            border-bottom: 1px solid #111111;  
            display: flex;  
            align-items: center;  
            cursor: pointer;  
        }  
  
        .track-row.active-playing {  
            background: var(--ipod-blue-gradient);  
        }  
  
        .track-row-title {  
            flex: 1;  
            white-space: nowrap;  
            overflow: hidden;  
            text-overflow: ellipsis;  
            padding-right: 10px;  
        }  
  
        .track-row-duration {  
            color: #8899aa;  
            font-weight: normal;  
        }  
  
        .track-row.active-playing .track-row-duration {  
            color: #ffffff;  
        }  
  
        /* Authentic Now Playing Split Layout Mirroring Target Photo */  
        .np-main-split {  
            display: flex;  
            padding: 10px 14px 4px 14px;  
            gap: 14px;  
            align-items: flex-start;  
            flex: 1;  
            box-sizing: border-box;  
            perspective: 500px;  
            background-color: #000000;   
        }  
  
        /* 3D Slanted Artwork Container with Reflection - Bigger Frame Size */  
        .np-left-block {  
            width: 125px;  
            height: 125px;  
            flex-shrink: 0;  
            position: relative;  
            transform: rotateY(30deg) rotateX(2deg);  
            transform-style: preserve-3d;  
        }  
  
        /* Stronger, More Explicit Vector Reflection Base Style */  
        .np-album-art {  
            width: 100%;  
            height: 100%;  
            object-fit: cover;  
            border: 1px solid rgba(255, 255, 255, 0.15);   
            box-shadow: -4px 4px 8px rgba(0, 0, 0, 0.6);  
            display: block;  
            -webkit-box-reflect: below 2px linear-gradient(transparent 35%, rgba(255, 255, 255, 0.40) 100%);  
        }  
  
        .np-right-block {  
            flex: 1;  
            display: flex;  
            flex-direction: column;  
            text-align: left;  
            overflow: hidden;  
            padding-top: 8px;  
        }  
  
        .np-track-title {  
            font-weight: bold;  
            font-size: 14px;  
            color: #ffffff;   
            margin: 0 0 4px 0;  
            white-space: nowrap;  
            overflow: hidden;  
            text-overflow: ellipsis;  
        }  
  
        .np-artist-name, .np-album-name {  
            font-size: 11px;  
            color: #b0c4de;   
            font-weight: bold;  
            margin: 0 0 2px 0;  
            white-space: nowrap;  
            overflow: hidden;  
            text-overflow: ellipsis;  
        }  
  
        /* Repositioned Queue counter style setup */  
        .np-queue-pos {  
            font-size: 10px;  
            color: #8899aa;   
            font-weight: bold;  
            margin-top: 10px;   
        }  
  
        /* Timeline Scrubber Block Architecture matching Reference Image */  
        .np-scrubber-layer {  
            height: 45px;  
            display: flex;  
            flex-direction: column;  
            justify-content: center;  
            padding: 0 14px 8px 14px;  
            box-sizing: border-box;  
            z-index: 5;  
            background-color: #000000;   
        }  
  
        .scrubber-track-container {  
            width: 100%;  
            height: 12px;  
            background: #222222;   
            border: 1px solid #444444;   
            position: relative;  
            box-sizing: border-box;  
            display: flex;  
            align-items: center;  
            cursor: pointer;  
        }  
  
        .scrubber-filled-progress {  
            height: 100%;  
            width: 0%;  
            background: var(--scrubber-progress);  
            pointer-events: none;  
        }  
  
        .scrubber-time-row {  
            display: flex;  
            justify-content: space-between;  
            font-size: 10px;  
            font-weight: bold;  
            color: #ffffff;   
            margin-top: 3px;  
            padding: 0 1px;  
        }  
  
        .back-nav-hotspot {  
            cursor: pointer;  
        }  
    </style>  
</head>  
<body>  
  
    <div class="ipod-screen-viewport">  
        <input type="file" id="file-input" multiple accept="audio/mp3" style="display: none;">  
        <audio id="audio-engine"></audio>  
  
        <div class="screen-face" id="front-view">  
            <div class="screen-header">  
                <span style="width: 16px;"></span> <span class="header-center-title" id="browsing-header-title">iPod</span>  
                <button class="view-toggle-btn" id="view-mode-toggle">List</button>  
            </div>  
              
            <div class="screen-body">  
                <div class="coverflow-wrapper" id="coverflow-view-pane">  
                    <div class="coverflow-stage" id="coverflow-stage"></div>  
                </div>  
                  
                <div class="coverflow-info-zone" id="coverflow-meta-zone">  
                    <button class="control-ring-btn" id="global-play-toggle" title="Play Active Selection">  
                        <svg class="ring-icon-svg" viewBox="0 0 24 24">  
                            <circle cx="12" cy="12" r="10"/>  
                            <g id="play-glyph-group" fill="#ffffff" stroke="none">   
                                <path d="M9.5 8.5v7l5.5-3.5-5.5-3.5z"/>  
                            </g>  
                        </svg>  
                    </button>  
  
                    <div class="cf-meta-text-column">  
                        <div class="cf-title" id="cf-title">No Tracks Loaded</div>  
                        <div class="cf-artist-album" id="cf-artist-album">Click + icon to load music</div>  
                    </div>  
  
                    <button class="control-ring-btn" onclick="document.getElementById('file-input').click()" title="Load Music">  
                        <svg class="ring-icon-svg" viewBox="0 0 24 24">  
                            <circle cx="12" cy="12" r="10" />  
                            <line x1="12" y1="7" x2="12" y2="17" />  
                            <line x1="7" y1="12" x2="17" y2="12" />  
                        </svg>  
                    </button>  
                </div>  
                <div class="classic-list-wrapper" id="list-view-pane"></div>  
            </div>  
        </div>  
  
        <div class="screen-face" id="album-tracks-view">  
            <div class="screen-header">  
                <span class="back-nav-hotspot" id="tracklist-back-btn">◀ Menu</span>  
                <span class="header-center-title" id="tracklist-header-title">Album Tracks</span>  
                <div class="header-right-group">  
                    <div class="battery-icon"><div class="battery-level"></div></div>  
                </div>  
            </div>  
            <div class="screen-body album-list-container" id="tracklist-items-pane"></div>  
        </div>  
  
        <div class="screen-face" id="now-playing-view">  
            <div class="screen-header">  
                <span class="back-nav-hotspot" id="np-back-btn">◀ Menu</span>  
                <span class="header-center-title">Now Playing</span>  
                <div class="header-right-group">  
                    <button class="shuffle-toggle-btn" id="shuffle-toggle" title="Shuffle Mode">  
                        <svg class="shuffle-icon-svg" viewBox="0 0 16 12">  
                            <path d="M1 11h2c1.5 0 2.5-1 3.5-2.5S8 5 9.5 3.5 11 2 12.5 2h2.5"/>  
                            <path d="M15 2l-2.5-2v4L15 2z" fill="currentColor"/>  
                            <path d="M1 2h2c1.5 0 2.5 1 3.5 2.5S8 7 9.5 8.5 11 10 12.5 10h2.5"/>  
                            <path d="M15 10l-2.5-2v4L15 10z" fill="currentColor"/>  
                        </svg>  
                    </button>  
                    <div class="battery-icon"><div class="battery-level"></div></div>  
                </div>  
            </div>  
              
            <div class="screen-body">  
                <div class="np-main-split">  
                    <div class="np-left-block">  
                        <img src="" class="np-album-art" id="np-art-image" alt="">  
                    </div>  
                    <div class="np-right-block">  
                        <h4 class="np-track-title" id="np-title">Track Title</h4>  
                        <p class="np-artist-name" id="np-artist">Artist Name</p>  
                        <p class="np-album-name" id="np-album">Album Name</p>  
                        <span class="np-queue-pos" id="np-queue-string">0 of 0</span>  
                    </div>  
                </div>  
  
                <div class="np-scrubber-layer">  
                    <div class="scrubber-track-container" id="scrubber-timeline-click">  
                        <div class="scrubber-filled-progress" id="scrubber-fill"></div>  
                    </div>  
                    <div class="scrubber-time-row">  
                        <span id="time-elapsed">0:00</span>  
                        <span id="time-remaining">-0:00</span>  
                    </div>  
                </div>  
            </div>  
        </div>  
    </div>  
  
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jsmediatags/3.9.5/jsmediatags.min.js"></script>  
    <script>  
        function generateGeometricArt(title, primaryColor, secondaryColor) {  
            const canvas = document.createElement('canvas');  
            canvas.width = 200; canvas.height = 200;  
            const ctx = canvas.getContext('2d');  
            let grad = ctx.createLinearGradient(0, 0, 200, 200);  
            grad.addColorStop(0, primaryColor); grad.addColorStop(1, secondaryColor);  
            ctx.fillStyle = grad; ctx.fillRect(0, 0, 200, 200);  
            ctx.fillStyle = 'rgba(255, 255, 255, 0.15)';  
            ctx.beginPath(); ctx.arc(100, 100, 80, 0, Math.PI * 2); ctx.fill();  
            ctx.fillStyle = '#ffffff'; ctx.font = 'bold 16px Arial'; ctx.textAlign = 'center';  
            ctx.fillText(title.substring(0, 12), 100, 105);  
            return canvas.toDataURL();  
        }  
  
        const defaultTracksList = [  
            { title: "Stronger", artist: "Kanye West", album: "Graduation", cover: generateGeometricArt("Graduation", "#7a2da6", "#b01777"), url: "" },  
            { title: "Darling, I", artist: "Tyler, The Creator", album: "CHROMAKOPIA", cover: generateGeometricArt("CHROMAKOPIA", "#11261a", "#1b4d3e"), url: "" },  
            { title: "Heaven Knows I'm Miserable Now", artist: "The Smiths", album: "Louder Than Bombs", cover: generateGeometricArt("The Smiths", "#b35436", "#5a2a18"), url: "" }  
        ];  
  
        let originalPlaylistOrder = [...defaultTracksList];  
        let playlist = [...defaultTracksList];  
        let currentTrackIndex = 0;  
        let activeSelectionMode = 'coverflow';  
        let isScrubbingActive = false;  
        let isShuffleActive = false;  
        let navigatedFromTracklist = false;   
  
        // Coverflow Scrubber Navigation State Variables  
        let isCoverflowDragging = false;  
        let coverflowStartX = 0;  
        let coverflowTotalDeltaX = 0;  
        let originalDragIndex = 0;  
  
        const audioEngine = document.getElementById('audio-engine');  
        const viewModeToggleBtn = document.getElementById('view-mode-toggle');  
        const shuffleToggleBtn = document.getElementById('shuffle-toggle');  
        const playToggleBtn = document.getElementById('global-play-toggle');  
        const playGlyphGroup = document.getElementById('play-glyph-group');  
        const coverflowViewPane = document.getElementById('coverflow-view-pane');  
        const listViewPane = document.getElementById('list-view-pane');  
        const coverflowStage = document.getElementById('coverflow-stage');  
        const fileInput = document.getElementById('file-input');  
        const frontView = document.getElementById('front-view');  
        const albumTracksView = document.getElementById('album-tracks-view');  
        const nowPlayingView = document.getElementById('now-playing-view');  
        const scrubberTrackContainer = document.getElementById('scrubber-timeline-click');  
        const tracklistItemsPane = document.getElementById('tracklist-items-pane');  
  
        window.addEventListener('DOMContentLoaded', () => {  
            renderCoverflowDOMNodes();  
            renderClassicListViewNodes();  
            synchronizeActiveSelectionTrackView();  
            attachCoreSystemEventHooks();  
            attachCoverflowScrubberHooks();  
            attachTimelineScrubbingHooks();   
        });  
  
        function renderCoverflowDOMNodes() {  
            coverflowStage.innerHTML = '';  
            playlist.forEach((track, index) => {  
                const item = document.createElement('div');  
                item.className = 'coverflow-item';  
                item.setAttribute('data-track-index', index);  
                item.style.backgroundImage = `url(${track.cover})`;  
                item.style.backgroundSize = 'cover';  
                  
                const img = document.createElement('img');  
                img.className = 'coverflow-art';  
                img.src = track.cover;  
                item.appendChild(img);  
                  
                item.addEventListener('click', (e) => {  
                    if (Math.abs(coverflowTotalDeltaX) > 6) {  
                        e.stopPropagation();  
                        return;  
                    }  
                    handleDirectTrackNodeClickSelection(index);  
                });  
                  
                coverflowStage.appendChild(item);  
            });  
            transformCoverflowPerspectiveSlides();  
        }  
  
        function renderClassicListViewNodes() {  
            listViewPane.innerHTML = '';  
            playlist.forEach((track, index) => {  
                const row = document.createElement('div');  
                row.className = `list-item ${index === currentTrackIndex ? 'selected' : ''}`;  
                const leftText = document.createElement('span');  
                leftText.textContent = track.title;  
                const rightText = document.createElement('span');  
                rightText.style.color = '#8899aa';   
                rightText.style.fontWeight = 'normal';  
                rightText.textContent = track.artist;  
                row.appendChild(leftText);  
                row.appendChild(rightText);  
                row.addEventListener('click', () => handleDirectTrackNodeClickSelection(index));  
                listViewPane.appendChild(row);  
            });  
        }  
  
        function renderTracklistUI() {  
            tracklistItemsPane.innerHTML = '';  
            if(playlist.length === 0) return;  
  
            const firstTrack = playlist[0];  
            document.getElementById('tracklist-header-title').textContent = firstTrack.album || "Album";  
  
            const headerRow = document.createElement('div');  
            headerRow.className = 'album-header-title';  
            headerRow.textContent = `${firstTrack.album || 'Unknown Album'} (${playlist.length} Songs)`;  
            tracklistItemsPane.appendChild(headerRow);  
  
            playlist.forEach((track, index) => {  
                const row = document.createElement('div');  
                row.className = 'track-row';  
                if(index === currentTrackIndex && !audioEngine.paused) {  
                    row.classList.add('active-playing');  
                }  
  
                const titleSpan = document.createElement('span');  
                titleSpan.className = 'track-row-title';  
                titleSpan.textContent = `${index + 1}. ${track.title}`;  
  
                const durationSpan = document.createElement('span');  
                durationSpan.className = 'track-row-duration';  
                durationSpan.textContent = track.durationText || "3:20";  
  
                row.appendChild(titleSpan);  
                row.appendChild(durationSpan);  
  
                row.addEventListener('click', () => {  
                    currentTrackIndex = index;  
                    synchronizeActiveSelectionTrackView();  
                    navigatedFromTracklist = true;   
                    switchToLayoutModeView('now-playing');  
                    playTargetActiveSelectionIndexTrack();  
                    renderTracklistUI();  
                });  
  
                tracklistItemsPane.appendChild(row);  
            });  
        }  
  
        function transformCoverflowPerspectiveSlides() {  
            const items = document.querySelectorAll('.coverflow-item');  
            items.forEach((item) => {  
                const idx = parseInt(item.getAttribute('data-track-index'));  
                const offset = idx - currentTrackIndex;  
                if (offset === 0) {  
                    item.style.transform = `translateX(0) rotateY(0deg) scale(1.15) translateZ(60px)`;  
                    item.style.zIndex = "20";  
                } else if (offset < 0) {  
                    item.style.transform = `translateX(${offset * 36 - 55}px) rotateY(68deg) scale(0.82) translateZ(0px)`;  
                    item.style.zIndex = 10 + offset;  
                } else {  
                    item.style.transform = `translateX(${offset * 36 + 55}px) rotateY(-68deg) scale(0.82) translateZ(0px)`;  
                    item.style.zIndex = 10 - offset;  
                }  
            });  
        }  
  
        function attachCoverflowScrubberHooks() {  
            const startDrag = (clientX) => {  
                if (activeSelectionMode !== 'coverflow') return;  
                isCoverflowDragging = true;  
                coverflowStartX = clientX;  
                coverflowTotalDeltaX = 0;  
                originalDragIndex = currentTrackIndex;  
            };  
  
            const moveDrag = (clientX) => {  
                if (!isCoverflowDragging) return;  
                coverflowTotalDeltaX = clientX - coverflowStartX;  
  
                const indexShift = Math.round(coverflowTotalDeltaX / 40);  
                let targetIndex = originalDragIndex - indexShift;  
  
                targetIndex = Math.max(0, Math.min(playlist.length - 1, targetIndex));  
  
                if (targetIndex !== currentTrackIndex) {  
                    currentTrackIndex = targetIndex;  
                    synchronizeActiveSelectionTrackView();  
                }  
            };  
  
            const endDrag = () => {  
                isCoverflowDragging = false;  
            };  
  
            coverflowViewPane.addEventListener('mousedown', (e) => startDrag(e.clientX));  
            window.addEventListener('mousemove', (e) => moveDrag(e.clientX));  
            window.addEventListener('mouseup', endDrag);  
  
            coverflowViewPane.addEventListener('touchstart', (e) => startDrag(e.touches[0].clientX), { passive: true });  
            window.addEventListener('touchmove', (e) => moveDrag(e.touches[0].clientX), { passive: true });  
            window.addEventListener('touchend', endDrag);  
        }  
  
        // Click, hold, and drag scrubber timeline engine  
        function attachTimelineScrubbingHooks() {  
            const getScrubPercentage = (clientX) => {  
                const rect = scrubberTrackContainer.getBoundingClientRect();  
                const offsetX = clientX - rect.left;  
                return Math.max(0, Math.min(1, offsetX / rect.width));  
            };  
  
            const processScrubUpdate = (clientX) => {  
                if (!audioEngine.duration) return;  
                const pct = getScrubPercentage(clientX);  
                audioEngine.currentTime = pct * audioEngine.duration;  
                  
                // Visual response optimization while dragging  
                document.getElementById('scrubber-fill').style.width = (pct * 100) + '%';  
                document.getElementById('time-elapsed').textContent = formatPlaybackTimeDisplay(audioEngine.currentTime);  
                document.getElementById('time-remaining').textContent = "-" + formatPlaybackTimeDisplay(audioEngine.duration - audioEngine.currentTime);  
            };  
  
            scrubberTrackContainer.addEventListener('mousedown', (e) => {  
                isScrubbingActive = true;  
                processScrubUpdate(e.clientX);  
            });  
  
            window.addEventListener('mousemove', (e) => {  
                if (isScrubbingActive) {  
                    processScrubUpdate(e.clientX);  
                }  
            });  
  
            window.addEventListener('mouseup', () => {  
                isScrubbingActive = false;  
            });  
  
            // Touch device bindings  
            scrubberTrackContainer.addEventListener('touchstart', (e) => {  
                isScrubbingActive = true;  
                processScrubUpdate(e.touches[0].clientX);  
            }, { passive: true });  
  
            window.addEventListener('touchmove', (e) => {  
                if (isScrubbingActive) {  
                    processScrubUpdate(e.touches[0].clientX);  
                }  
            }, { passive: true });  
  
            window.addEventListener('touchend', () => {  
                isScrubbingActive = false;  
            });  
        }  
  
        function synchronizeActiveSelectionTrackView() {  
            if (playlist.length === 0) return;  
            const activeTrack = playlist[currentTrackIndex];  
              
            document.getElementById('cf-title').textContent = activeTrack.title;  
            document.getElementById('cf-artist-album').textContent = `${activeTrack.artist} — ${activeTrack.album}`;  
              
            document.getElementById('np-title').textContent = activeTrack.title;  
            document.getElementById('np-artist').textContent = activeTrack.artist;  
            document.getElementById('np-album').textContent = activeTrack.album;  
            document.getElementById('np-art-image').src = activeTrack.cover;  
            document.getElementById('np-queue-string').textContent = `${currentTrackIndex + 1} of ${playlist.length}`;  
  
            // Sync Header Album Title Context matching dynamic file details  
            document.getElementById('tracklist-header-title').textContent = activeTrack.album || "Album Tracks";  
  
            const rows = listViewPane.querySelectorAll('.list-item');  
            rows.forEach((row, idx) => {  
                row.classList.toggle('selected', idx === currentTrackIndex);  
            });  
            transformCoverflowPerspectiveSlides();  
        }  
  
        function handleDirectTrackNodeClickSelection(targetIndex) {  
            if (currentTrackIndex === targetIndex) {  
                navigatedFromTracklist = false;  
                switchToLayoutModeView('now-playing');  
                playTargetActiveSelectionIndexTrack();  
            } else {  
                currentTrackIndex = targetIndex;  
                synchronizeActiveSelectionTrackView();  
                if (!audioEngine.paused) {  
                    playTargetActiveSelectionIndexTrack();  
                }  
            }  
        }  
  
        function togglePlayPauseEngine() {  
            const track = playlist[currentTrackIndex];  
            if (!track || !track.url) {  
                document.getElementById('file-input').click();  
                return;  
            }  
  
            if (!audioEngine.src || !audioEngine.src.includes(encodeURIComponent(track.url).substring(0,20))) {  
                audioEngine.src = track.url;  
            }  
  
            if (audioEngine.paused) {  
                audioEngine.play()  
                    .then(() => {  
                        updatePlayPauseGlyphState(true);  
                        renderTracklistUI();  
                    })  
                    .catch(e => console.log("Audio device source exception handled:", e));  
            } else {  
                audioEngine.pause();  
                updatePlayPauseGlyphState(false);  
                renderTracklistUI();  
            }  
        }  
  
        function updatePlayPauseGlyphState(isPlaying) {  
            if (isPlaying) {  
                playGlyphGroup.innerHTML = `  
                    <rect x="8.5" y="8" width="2.5" height="8"/>  
                    <rect x="13" y="8" width="2.5" height="8"/>  
                `;  
            } else {  
                playGlyphGroup.innerHTML = `<path d="M9.5 8.5v7l5.5-3.5-5.5-3.5z"/>`;  
            }  
        }  
  
        function formatPlaybackTimeDisplay(seconds) {  
            if (isNaN(seconds)) return "0:00";  
            const mins = Math.floor(seconds / 60);  
            const secs = Math.floor(seconds % 60);  
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;  
        }  
  
        function playTargetActiveSelectionIndexTrack() {  
            const track = playlist[currentTrackIndex];  
            if (!track || !track.url) return;  
            audioEngine.src = track.url;  
            audioEngine.play()  
                .then(() => {  
                    updatePlayPauseGlyphState(true);  
                    renderTracklistUI();  
                })  
                .catch(e => console.log("Playback target error code emitted:", e));  
        }  
  
        function switchToLayoutModeView(targetViewMode) {  
            frontView.style.display = 'none';  
            albumTracksView.style.display = 'none';  
            nowPlayingView.style.display = 'none';  
  
            if (targetViewMode === 'menu') {  
                frontView.style.display = 'flex';  
            } else if (targetViewMode === 'tracklist') {  
                albumTracksView.style.display = 'flex';  
                renderTracklistUI();  
            } else if (targetViewMode === 'now-playing') {  
                nowPlayingView.style.display = 'flex';  
            }  
        }  
  
        function attachCoreSystemEventHooks() {  
            audioEngine.addEventListener('timeupdate', () => {  
                if (isScrubbingActive || !audioEngine.duration) return;  
                const current = audioEngine.currentTime;  
                const duration = audioEngine.duration;  
                const pct = (current / duration) * 100;  
  
                document.getElementById('scrubber-fill').style.width = pct + '%';  
                document.getElementById('time-elapsed').textContent = formatPlaybackTimeDisplay(current);  
                document.getElementById('time-remaining').textContent = "-" + formatPlaybackTimeDisplay(duration - current);  
            });  
  
            audioEngine.addEventListener('loadedmetadata', () => {  
                if (playlist[currentTrackIndex]) {  
                    playlist[currentTrackIndex].durationText = formatPlaybackTimeDisplay(audioEngine.duration);  
                    renderTracklistUI();  
                }  
            });  
  
            audioEngine.addEventListener('ended', () => {  
                if (playlist.length === 0) return;  
                currentTrackIndex = (currentTrackIndex + 1) % playlist.length;  
                synchronizeActiveSelectionTrackView();  
                playTargetActiveSelectionIndexTrack();  
            });  
  
            viewModeToggleBtn.addEventListener('click', () => {  
                if (activeSelectionMode === 'coverflow') {  
                    activeSelectionMode = 'list';  
                    coverflowViewPane.style.display = 'none';  
                    document.getElementById('coverflow-meta-zone').style.display = 'none';  
                    listViewPane.style.display = 'block';  
                    viewModeToggleBtn.textContent = 'Flow';  
                    document.getElementById('browsing-header-title').textContent = 'Music';  
                } else {  
                    activeSelectionMode = 'coverflow';  
                    listViewPane.style.display = 'none';  
                    coverflowViewPane.style.display = 'flex';  
                    document.getElementById('coverflow-meta-zone').style.display = 'flex';  
                    viewModeToggleBtn.textContent = 'List';  
                    document.getElementById('browsing-header-title').textContent = 'iPod';  
                    transformCoverflowPerspectiveSlides();  
                }  
            });  
  
            playToggleBtn.addEventListener('click', (e) => {  
                e.stopPropagation();  
                togglePlayPauseEngine();  
            });  
  
            shuffleToggleBtn.addEventListener('click', () => {  
                isShuffleActive = !isShuffleActive;  
                shuffleToggleBtn.classList.toggle('active', isShuffleActive);  
                  
                const activeTrack = playlist[currentTrackIndex];  
                if (isShuffleActive) {  
                    for (let i = playlist.length - 1; i > 0; i--) {  
                        const j = Math.floor(Math.random() * (i + 1));  
                        [playlist[i], playlist[j]] = [playlist[j], playlist[i]];  
                    }  
                } else {  
                    playlist = [...originalPlaylistOrder];  
                }  
                  
                if (activeTrack) {  
                    currentTrackIndex = playlist.findIndex(t => t.title === activeTrack.title);  
                }  
                renderCoverflowDOMNodes();  
                renderClassicListViewNodes();  
                synchronizeActiveSelectionTrackView();  
            });  
  
            document.getElementById('tracklist-back-btn').addEventListener('click', () => switchToLayoutModeView('menu'));  
              
            document.getElementById('np-back-btn').addEventListener('click', () => {  
                if (navigatedFromTracklist) {  
                    switchToLayoutModeView('tracklist');  
                } else {  
                    switchToLayoutModeView('menu');  
                }  
            });  
  
            fileInput.addEventListener('change', (e) => {  
                const files = Array.from(e.target.files);  
                if (files.length === 0) return;  
  
                playlist = [];  
                originalPlaylistOrder = [];  
                currentTrackIndex = 0;  
                let parsedCount = 0;  
  
                files.forEach((file) => {  
                    const objectUrl = URL.createObjectURL(file);  
                    let trackTitle = file.name.replace(/\.[^/.]+$/, "");  
                    let trackArtist = "Unknown Artist";  
                    let trackAlbum = "Unknown Album";  
                    let albumArtUrl = generateGeometricArt(trackTitle, "#444546", "#111213");  
  
                    jsmediatags.read(file, {  
                        onSuccess: function(tag) {  
                            if (tag.tags.title) trackTitle = tag.tags.title;  
                            if (tag.tags.artist) trackArtist = tag.tags.artist;  
                            if (tag.tags.album) trackAlbum = tag.tags.album;  
                              
                            if (tag.tags.picture) {  
                                const pic = tag.tags.picture;  
                                let base64String = "";  
                                for (let i = 0; i < pic.data.length; i++) {  
                                    base64String += String.fromCharCode(pic.data[i]);  
                                }  
                                albumArtUrl = `data:${pic.format};base64,${window.btoa(base64String)}`;  
                            }  
                            finalizeTrackImport();  
                        },  
                        onError: function(error) {  
                            console.log("Tag parsed error fallback initialized:", error);  
                            finalizeTrackImport();  
                        }  
                    });  
  
                    function finalizeTrackImport() {  
                        const entry = {  
                            title: trackTitle,  
                            artist: trackArtist,  
                            album: trackAlbum,  
                            cover: albumArtUrl,  
                            url: objectUrl,  
                            durationText: "0:00"  
                        };  
                        playlist.push(entry);  
                        originalPlaylistOrder.push(entry);  
                        parsedCount++;  
  
                        if (parsedCount === files.length) {  
                            renderCoverflowDOMNodes();  
                            renderClassicListViewNodes();  
                            synchronizeActiveSelectionTrackView();  
                            renderTracklistUI();  
                              
                            if(audioEngine.paused) {  
                                togglePlayPauseEngine();  
                            }  
                        }  
                    }  
                });  
            });  
        }  
    </script>  
</body>  
</html>  
