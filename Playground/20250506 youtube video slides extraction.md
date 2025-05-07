
https://studio.firebase.google.com/



2025-05-06T22:14:09-07:00
Using firebase.
Look at this shit:
![[Pasted image 20250506221417.png]]

2025-05-06T22:36:46-07:00
ok this is kind of useful:
![[Pasted image 20250506223650.png]]

full of shit lol:
![[Pasted image 20250506224415.png]]

2025-05-06T22:53:23-07:00
wow it seems to be working:
![[Pasted image 20250506225328.png]]

fuck
![[Pasted image 20250506225946.png]]

Gemini API:
- https://console.cloud.google.com/marketplace/product/google/generativelanguage.googleapis.com

Somehow it is getting worse:
![[Pasted image 20250506231116.png]]

Look at this slacker:
![[Pasted image 20250506231227.png]]

2025-05-06T23:29:48-07:00
Ran tests. Seems like the frame extraction is not working properly.
![[Pasted image 20250506232959.png]]

2025-05-06T23:39:10-07:00
After some chatting, it seems like I could only get thumbnails, so the images make sense. Gemini also refuse to write me a scraper lol. The irony.

o4's recommendation:
### Recommendation

1. **If you just need frames for an AI pipeline**:  
    Use **ytdl-core + FFmpeg**. You already have `youtubedl-core`; switching to download the video (`-f mp4`) and then invoking FFmpeg (via [`fluent-ffmpeg`](https://github.com/fluent-ffmpeg/node-fluent-ffmpeg)) to extract `-ss <time> -frames:v 1` is by far the most robust and fastest once set up.
    
2. **If you really must simulate a real browser** (e.g. to respect YouTube’s player DRM or dynamic ads):  
    Go with **Puppeteer** rather than full Selenium. You’ll write plain JavaScript, control the `<video>` element’s `currentTime`, wait for the frame to render, then call `page.screenshot({ clip: … })`. It’s lighter-weight and better supported in the Node ecosystem.

2025-05-06T23:52:14-07:00
`npm install ytdl-core fluent-ffmpeg`
`npm install --save-dev @types/node`

Target video: [Cisco Quantum Summit 2025: Pathways to Practical and Functional Quantum Networking](https://www.youtube.com/watch?v=vspD719IM0E&ab_channel=OutshiftbyCisco)
cursor is asking me to 
- `npm install --save-dev typescript`
- `npm install --save-dev @types/fluent-ffmpeg`

2025-05-06T23:58:16-07:00
- seems to have compiled with `npx tsc`
- running it with `node dist/extract-frames.js` is not working
- error:
```shell
fatal error: Error: Could not extract functions
    at /Users/wentaojiang/Documents/GitHub/PlayGround/20250506_youtube2slides/node_modules/ytdl-core/lib/sig.js:20:11
    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)
```

Trying
- `npm install --save-dev ts-node`
- still not working

2025-05-07T00:01:42-07:00
Now trying:
- `npm install youtube-dl-exec`
- `brew install youtube-dl`








