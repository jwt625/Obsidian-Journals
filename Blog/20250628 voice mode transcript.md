
2025-06-28T12:30:03-07:00
openai's chatgpt voice mode sucks:

![[Pasted image 20250628123032.png]]
I spent last night building my own streaming with whisper, but it sucks, breaking the words into pieces.
- https://github.com/jwt625/PlayGround
Also tried to use assembly AI. Need to pay to use streaming although the website shows I have credits for streaming:
- https://www.assemblyai.com/dashboard/usage
![[Pasted image 20250628123136.png]]

## whisper.cpp
Now starting in a new repo and trying to use whisper.cpp:
- https://github.com/ggml-org/whisper.cpp

2025-06-28T12:36:46-07:00
Successfully built the stream binary.

2025-06-28T12:41:51-07:00
Test story and transcription:

```markdown
**The Magic Coffee Shop**

Every morning, Sarah walked past the small coffee shop on Maple Street. The sign read "Luna's Café" in faded blue letters. She had never gone inside because it always seemed closed, even during busy hours.

One rainy Tuesday, Sarah decided to try the door. To her surprise, it opened with a gentle chime. Inside, an elderly woman with silver hair smiled warmly.

"Welcome, dear. I've been waiting for you," the woman said, pouring steaming coffee into a blue ceramic cup. "This is our special blend. It helps people find what they're looking for."

Sarah took a sip. The coffee tasted like cinnamon and dreams. Suddenly, she remembered her childhood goal of becoming a writer. She had forgotten it years ago while working at the bank.

"Thank you," Sarah whispered, placing coins on the counter.

When she turned back from the door, the shop had vanished. Only an empty lot remained, with a small blue cup sitting on the sidewalk.

Sarah picked up the cup and smiled. She knew exactly what she was looking for now.
```

```

### Transcription 6 START | t0 = 0 ms | t1 = 28818 ms

[00:00:00.000 --> 00:00:04.000]   The magic coffee shop.
[00:00:04.000 --> 00:00:08.120]   Every morning Sarah walked past the small coffee shop on Maple Street.
[00:00:08.120 --> 00:00:11.840]   The sign reads "Nunas cafe in faded blue letters."
[00:00:11.840 --> 00:00:17.760]   She had never gone inside because it always seemed closed, even during busy hours.
[00:00:17.760 --> 00:00:21.200]   One raining Tuesday Sarah decided to try the door.
[00:00:21.200 --> 00:00:24.840]   To her surprise it opened with a gentle chime.
[00:00:24.840 --> 00:00:29.080]   Inside an elderly woman with silver hair smiled warmly.

### Transcription 6 END

### Transcription 7 START | t0 = 6055 ms | t1 = 36055 ms

[00:00:00.000 --> 00:00:05.720]   coffee shop on Maple Street. The sign reads, "Nunas cafe invaded blue letters." She had
[00:00:05.720 --> 00:00:11.920]   never gone inside because it always seemed closed, even during busy hours. One raining
[00:00:11.920 --> 00:00:18.480]   Tuesday, Sarah decided to try the boar. To her surprise, it opened with a gentle chime.
[00:00:18.480 --> 00:00:24.280]   Inside an elderly woman with silver hair smiled warmly. "Welcome, dear. I've been waiting
[00:00:24.280 --> 00:00:29.840]   for you," the woman said, "for streaming steaming coffee into a blue ceramic cup."

### Transcription 7 END

### Transcription 8 START | t0 = 10697 ms | t1 = 40697 ms

[00:00:00.000 --> 00:00:06.800]   She had never gone inside because it always seemed closed even during busy hours.
[00:00:06.800 --> 00:00:10.280]   One rainy Tuesday, Sarah decided to try the door.
[00:00:10.280 --> 00:00:13.880]   To her surprise, it opened with a gentle chime.
[00:00:13.880 --> 00:00:18.000]   Inside an elderly woman with silver hair smiled warmly.
[00:00:18.000 --> 00:00:19.000]   "Welcome, dear.
[00:00:19.000 --> 00:00:24.440]   I've been waiting for you," the woman said, "for streaming steaming coffee into a blue
[00:00:24.440 --> 00:00:25.880]   ceramic cup.
[00:00:25.880 --> 00:00:27.420]   This is our special blend.
[00:00:27.420 --> 00:00:31.580]   helps people find what they are looking for.

### Transcription 8 END

### Transcription 9 START | t0 = 14786 ms | t1 = 44786 ms

[00:00:00.000 --> 00:00:06.720]   even during busy hours. One raining Tuesday, Sarah decided to try the boar. To her surprise,
[00:00:06.720 --> 00:00:13.400]   it opened with a gentle chime. Inside an elderly woman with silver hair smiled warmly.
[00:00:13.400 --> 00:00:19.940]   "Welcome there. I've been waiting for you," the women said, "for streaming steaming coffee into
[00:00:19.940 --> 00:00:25.440]   a blue ceramic cup. This is our special blend. It helps people find what they are looking for."
[00:00:25.440 --> 00:00:29.600]   Sarah took a sip, the coffee tasted like cinema and dreams.

### Transcription 9 END

### Transcription 10 START | t0 = 18753 ms | t1 = 48753 ms

[00:00:00.000 --> 00:00:04.720]   Sarah decided to try the boar. To her surprise, it opened with a gentle chime.
[00:00:04.720 --> 00:00:12.240]   Inside an elderly woman with silver hair smiled warm. "Welcome, dear. I've been waiting for you,"
[00:00:12.240 --> 00:00:19.200]   the woman said, pouring streaming coffee into a blue ceramic cup. "This is our special blend.
[00:00:19.200 --> 00:00:25.120]   It helps people find what they are looking for." Sarah took a sip, the coffee tasted like cinema
[00:00:25.120 --> 00:00:29.920]   and dreams. Suddenly she remembered her childhood goal of becoming a writer. She had

### Transcription 10 END

### Transcription 11 START | t0 = 21280 ms | t1 = 51280 ms

[00:00:00.000 --> 00:00:02.840]   surprise it opened with a gentle chime.
[00:00:02.840 --> 00:00:07.360]   Inside an elderly woman with silver hair smiled warmly.
[00:00:07.360 --> 00:00:08.360]   "Welcome, dear.
[00:00:08.360 --> 00:00:13.880]   I've been waiting for you," the woman said, "for streaming steaming coffee into a blue
[00:00:13.880 --> 00:00:15.320]   ceramic cup.
[00:00:15.320 --> 00:00:16.840]   This is our special blend.
[00:00:16.840 --> 00:00:19.600]   It helps people find what they are looking for."
[00:00:19.600 --> 00:00:20.920]   Sarah took a sip.
[00:00:20.920 --> 00:00:23.600]   The coffee tasted like cinnamon and dreams.
[00:00:23.600 --> 00:00:27.280]   Suddenly she remembered her childhood goal of becoming a writer.
[00:00:27.280 --> 00:00:29.880]   had forgotten it years ago while working at the bank.

### Transcription 11 END

### Transcription 12 START | t0 = 23633 ms | t1 = 53633 ms

[00:00:00.000 --> 00:00:05.000]   "Inside an elderly woman with silver hair smiled warm.
[00:00:05.000 --> 00:00:06.000]   "Welcome, dear.
[00:00:06.000 --> 00:00:08.520]   "I've been waiting for you," the woman said,
[00:00:08.520 --> 00:00:12.800]   "for streaming steaming coffee into a blue ceramic cup.
[00:00:12.800 --> 00:00:14.360]   "This is our special blend.
[00:00:14.360 --> 00:00:17.160]   "It helps people find what they are looking for.
[00:00:17.160 --> 00:00:18.460]   "Sara took a sip.
[00:00:18.460 --> 00:00:21.120]   "The coffee tasted like cinema and dreams.
[00:00:21.120 --> 00:00:23.000]   "Suddenly she remembered her childhood
[00:00:23.000 --> 00:00:24.760]   "go of becoming a writer.
[00:00:24.760 --> 00:00:28.200]   "She had forgotten it years ago while working at the bank."
[00:00:28.200 --> 00:00:29.640]   Thank you, Sarah Whispert.

### Transcription 12 END

### Transcription 13 START | t0 = 25690 ms | t1 = 55690 ms

[00:00:00.000 --> 00:00:06.400]   with silver hair smiled warm. "Welcome there, I've been waiting for you," the women said,
[00:00:06.400 --> 00:00:12.800]   "for streaming steaming coffee into a blue ceramic cup. This is our special blend. It helps
[00:00:12.800 --> 00:00:18.320]   people find what they are looking for." Sarah took a sip. The coffee tasted like cinema and
[00:00:18.320 --> 00:00:23.680]   dreams. Suddenly she remembered her childhood goal of becoming a writer. She had forgotten it
[00:00:23.680 --> 00:00:29.840]   years ago while working at the bank. "Thank you, Sarah Whispert, placing coins on the counter."

### Transcription 13 END

### Transcription 14 START | t0 = 28982 ms | t1 = 58982 ms

[00:00:00.000 --> 00:00:05.320]   "Come dear, I've been waiting for you," the women said, "for streaming Steaming Coffee
[00:00:05.320 --> 00:00:07.600]   into a Blue Ceramic Cup.
[00:00:07.600 --> 00:00:09.120]   This is our special blend.
[00:00:09.120 --> 00:00:11.920]   It helps people find what they are looking for.
[00:00:11.920 --> 00:00:13.200]   Sarah took a sip.
[00:00:13.200 --> 00:00:15.880]   The coffee tasted like cinema and dreams.
[00:00:15.880 --> 00:00:19.560]   Suddenly she remembered her childhood goal of becoming a writer.
[00:00:19.560 --> 00:00:22.600]   She had forgotten it years ago while working at the bank.
[00:00:22.600 --> 00:00:25.000]   "Thank you, Sarah Whispert.
[00:00:25.000 --> 00:00:26.720]   Placing coins on the counter.
[00:00:26.720 --> 00:00:29.960]   When she turned back from the door, the shop had been...

### Transcription 14 END

### Transcription 15 START | t0 = 31889 ms | t1 = 61889 ms

[00:00:00.000 --> 00:00:04.160]   pouring streaming steaming coffee into a blue ceramic cup.
[00:00:04.160 --> 00:00:05.920]   This is our special blend.
[00:00:05.920 --> 00:00:08.560]   It helps people find what they are looking for.
[00:00:08.560 --> 00:00:09.960]   Sarah took a sip.
[00:00:09.960 --> 00:00:12.640]   The coffee tasted like cinema and dreams.
[00:00:12.640 --> 00:00:16.240]   Suddenly she remembered her childhood goal of becoming a writer.
[00:00:16.240 --> 00:00:19.520]   She had forgotten it years ago while working at the bank.
[00:00:19.520 --> 00:00:23.440]   Thank you, Sarah whispered, placing coins on the counter.
[00:00:23.440 --> 00:00:27.480]   When she turned back from the door, the shop had vanished.
[00:00:27.480 --> 00:00:29.760]   Only an empty lot remained.
[00:00:29.760 --> 00:00:39.760]   [BLANK_AUDIO]

### Transcription 15 END

### Transcription 16 START | t0 = 37393 ms | t1 = 67393 ms

[00:00:00.000 --> 00:00:05.840]   It helps people find what they are looking for. Sarah took a sip, the coffee tasted like
[00:00:05.840 --> 00:00:11.160]   cinema and dreams. Suddenly she remembered her childhood goal of becoming a writer. She
[00:00:11.160 --> 00:00:14.160]   had forgotten it years ago while working at the bank.
[00:00:14.160 --> 00:00:20.200]   "Thank you, Sarah Whispert. Placing coins on the counter. When she returned back from
[00:00:20.200 --> 00:00:26.520]   the door, the shop had vanished. Only an empty lot remained, with a small blue cup sitting
[00:00:26.520 --> 00:00:29.880]   on the sidewalk. Sarah picked up the cup and smiled.

### Transcription 16 END

### Transcription 17 START | t0 = 40622 ms | t1 = 70622 ms

[00:00:00.000 --> 00:00:04.080]   Sarah took a sip, the coffee tasted like cinema and dreams.
[00:00:04.080 --> 00:00:07.600]   Suddenly she remembered her childhood goal of becoming a writer.
[00:00:07.600 --> 00:00:10.480]   She had forgotten it years ago while working at the bank.
[00:00:10.480 --> 00:00:12.320]   "Thank you, Sarah Whispert.
[00:00:12.320 --> 00:00:18.800]   Placing coins on the counter. When she turned back from the door, the shop had vanished.
[00:00:18.800 --> 00:00:24.320]   Only an empty lot remained, with a small blue cup sitting on the sidewalk.
[00:00:24.320 --> 00:00:29.280]   Sarah picked up the cup and smiled. She knew exactly what she was looking for now.

### Transcription 17 END

### Transcription 18 START | t0 = 42988 ms | t1 = 72988 ms

[00:00:00.000 --> 00:00:05.680]   like cinema and dreams, suddenly she remembered her childhood goal of becoming a writer. She had
[00:00:05.680 --> 00:00:11.840]   forgotten it years ago while working at the bank. "Thank you, Sarah Whispert, placing coins on the
[00:00:11.840 --> 00:00:18.880]   counter. When she returned back from the door, the shop had vanished. Only an empty lot remained,
[00:00:18.880 --> 00:00:25.360]   with a small blue cup sitting on the sidewalk. Sarah picked up the cup and smiled. She knew exactly
[00:00:25.360 --> 00:00:29.040]   what she was looking for now.
```


2025-06-28T14:25:25-07:00
FE not displaying the trasncript properly. FE server console log is fine:
### FE console log
```

🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:22.257659', 'microphone_level': 0.0}
🎤 Whisper.cpp result: '' (confidence: 0.40, time: 0.01s)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:22.894018', 'microphone_level': 0.0}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:23.532947', 'microphone_level': 0.00030672947844634737}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:24.174333', 'microphone_level': 1.7245953748153914e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:24.814734', 'microphone_level': 3.435980481638967e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:25.453059', 'microphone_level': 0.011685958994090853}
🎤 Whisper.cpp result: '' (confidence: 0.40, time: 0.11s)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:26.095467', 'microphone_level': 0.00025737980852980714}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:26.736825', 'microphone_level': 0.0255415876831349}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:27.376586', 'microphone_level': 0.0005224075221919335}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:28.013268', 'microphone_level': 0.0010513709404719783}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:28.653416', 'microphone_level': 0.09068651089033937}
🎤 Whisper.cpp result: 'Hello hello hello the magic' (confidence: 0.80, time: 0.10s)
📝 Raw transcript (microphone): Hello hello hello the magic
✨ New content (microphone): Hello hello hello the magic
📤 New transcript queued: {'type': 'transcript_update', 'session_id': 'session_20250628_142321', 'timestamp': '2025-06-28T14:23:28.758104', 'source': 'microphone', 'text': 'Hello hello hello the magic', 'raw_text': 'Hello hello hello the magic', 'confidence': 0.8, 'is_final': True, 'is_deduplicated': True}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:29.293949', 'microphone_level': 0.023825394651416632}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:29.934360', 'microphone_level': 0.00017536217290226524}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:30.574891', 'microphone_level': 0.06217195210096095}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:31.213348', 'microphone_level': 0.08714920254896313}
💾 Saving audio buffers at chunk 150
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:31.856085', 'microphone_level': 0.0545689464688654}
🎤 Whisper.cpp result: 'coffee shop every morning Sarah walked' (confidence: 0.80, time: 0.09s)
📝 Raw transcript (microphone): coffee shop every morning Sarah walked
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:32.495022', 'microphone_level': 0.013875686264119579}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:33.135341', 'microphone_level': 0.03292571651738029}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:33.773675', 'microphone_level': 0.030596798840821178}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:34.413512', 'microphone_level': 0.004642602176208151}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:35.055258', 'microphone_level': 0.00010427690263189882}
🎤 Whisper.cpp result: 'the small coffee shop on Maple Street.' (confidence: 0.80, time: 0.11s)
📝 Raw transcript (microphone): the small coffee shop on Maple Street.
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:35.696043', 'microphone_level': 0.03585906981490524}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:36.334770', 'microphone_level': 0.03840683262338061}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:36.978047', 'microphone_level': 0.05992475861170347}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:37.615300', 'microphone_level': 0.06650410146116902}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:38.257720', 'microphone_level': 0.05018134074488052}
🎤 Whisper.cpp result: 'The sign read "Nunas cafe in Fade"' (confidence: 0.80, time: 0.16s)
📝 Raw transcript (microphone): The sign read "Nunas cafe in Fade"
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:38.894914', 'microphone_level': 0.10862039706965086}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:39.534599', 'microphone_level': 0.002374391587510276}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:40.177423', 'microphone_level': 0.05387426666167562}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:40.817405', 'microphone_level': 0.05874267302445105}
💾 Saving audio buffers at chunk 300
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:41.453726', 'microphone_level': 4.9958038574557765e-05}
🎤 Whisper.cpp result: 'Blue letters she had never come inside' (confidence: 0.80, time: 0.11s)
📝 Raw transcript (microphone): Blue letters she had never come inside
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:42.091817', 'microphone_level': 0.00717463306931445}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:42.734794', 'microphone_level': 0.026382690768699264}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:43.374101', 'microphone_level': 0.00011300480383579814}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:44.015699', 'microphone_level': 0.05905351700786996}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:44.654370', 'microphone_level': 0.026807008694914092}
🎤 Whisper.cpp result: 'because it always seemed closed even during busy' (confidence: 0.80, time: 0.11s)
📝 Raw transcript (microphone): because it always seemed closed even during busy
✨ New content (microphone): because it always seemed closed even during busy
📤 New transcript queued: {'type': 'transcript_update', 'session_id': 'session_20250628_142321', 'timestamp': '2025-06-28T14:23:44.767446', 'source': 'microphone', 'text': 'because it always seemed closed even during busy', 'raw_text': 'because it always seemed closed even during busy', 'confidence': 0.8, 'is_final': True, 'is_deduplicated': True}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:45.295386', 'microphone_level': 4.661454306598491e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:45.934819', 'microphone_level': 0.006468977290622758}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:46.574110', 'microphone_level': 0.03870964463579974}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:47.215272', 'microphone_level': 0.023226943891958753}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:47.853119', 'microphone_level': 0.1276082298406171}
🎤 Whisper.cpp result: 'hours. One raining Tuesday, Sarah.' (confidence: 0.80, time: 0.16s)
📝 Raw transcript (microphone): hours. One raining Tuesday, Sarah.
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:48.495068', 'microphone_level': 0.04742814798425245}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:49.136055', 'microphone_level': 0.17053281145092625}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:49.774586', 'microphone_level': 0.00018596727752662263}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:50.413606', 'microphone_level': 0.028790924158881268}
💾 Saving audio buffers at chunk 450
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:51.055855', 'microphone_level': 0.002751543105174294}
🎤 Whisper.cpp result: 'Sarah decided to try the door to a surprise.' (confidence: 0.80, time: 0.13s)
📝 Raw transcript (microphone): Sarah decided to try the door to a surprise.
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:51.693961', 'microphone_level': 0.01222524568145209}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:52.332632', 'microphone_level': 0.04385018803455077}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:52.978470', 'microphone_level': 0.0013413982977526456}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:53.614597', 'microphone_level': 4.830109966493568e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:54.256597', 'microphone_level': 4.313861017322927e-05}
🎤 Whisper.cpp result: 'It opened with a gentle chime.' (confidence: 0.80, time: 0.13s)
📝 Raw transcript (microphone): It opened with a gentle chime.
🔄 Duplicate content ignored (microphone)
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:54.897535', 'microphone_level': 6.602631717721118e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:55.533522', 'microphone_level': 4.0270685767597736e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:56.176277', 'microphone_level': 4.324390376916966e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:56.811942', 'microphone_level': 4.435490000421492e-05}
🔊 Audio level queued: {'type': 'audio_level', 'timestamp': '2025-06-28T14:23:57.455383', 'microphone_level': 4.28848534073927e-05}
🎤 Whisper.cpp result: 'the city.' (confidence: 0.60, time: 0.13s)
📝 Raw transcript (microphone): the city.
🔄 Duplicate content ignored (microphone)
💾 Saving final audio buffers...
127.0.0.1 - - [28/Jun/2025 14:23:57] "POST /api/stop HTTP/1.1" 200 -
```

### Test dedup
2025-06-28T15:14:01-07:00
If I read everything really clearly, it could dedup alright:
```

--- Transcription 33 ---
[RAW]  One rainy Tuesday, Sarah decided to try the door. To her surprise, it opened with a gentle chime,
[NEW]  One rainy Tuesday, Sarah decided to try the door. To her surprise, it opened with a gentle chime,
[FULL] The Magic Coffee Shop. Every morning Sarah walked past the small coffee shop on Maple Street. The sign read "Nunas Cafe Infated Blue Letters". She had never gone inside because it always seemed closed. She had never gone inside because it always seemed closed, even during busy hours. One raining Tuesday. One rainy Tuesday Sarah decided to try the door. The sign read "Nunas Cafe" in faded blue letters. To her surprise, it opened with a gentle chime. and elderly women with silver hair smiled warmly. One rainy Tuesday, Sarah decided to try the door. To her surprise, it opened with a gentle chime,
----------------------------------------
[DEBUG 166] Found transcript: inside an elderly woman with silver hair smiled warmly.
[DEDUP DEBUG] Accumulated: 'The Magic Coffee Shop. Every morning Sarah walked past the small coffee shop on Maple Street. The sign read "Nunas Cafe Infated Blue Letters". She had never gone inside because it always seemed closed. She had never gone inside because it always seemed closed, even during busy hours. One raining Tuesday. One rainy Tuesday Sarah decided to try the door. The sign read "Nunas Cafe" in faded blue letters. To her surprise, it opened with a gentle chime. and elderly women with silver hair smiled warmly. One rainy Tuesday, Sarah decided to try the door. To her surprise, it opened with a gentle chime, inside an elderly woman with silver hair smiled warmly.'
[DEDUP DEBUG] New raw: 'inside an elderly woman with silver hair smiled warmly.'
[DEDUP DEBUG] Last raw: 'inside an elderly woman with silver hair smiled warmly.'
[DEDUP DEBUG] Extracted: 'inside an elderly woman with silver hair smiled warmly.'

--- Transcription 34 ---
[RAW]  inside an elderly woman with silver hair smiled warmly.
[NEW]  inside an elderly woman with silver hair smiled warmly.
[FULL] The Magic Coffee Shop. Every morning Sarah walked past the small coffee shop on Maple Street. The sign read "Nunas Cafe Infated Blue Letters". She had never gone inside because it always seemed closed. She had never gone inside because it always seemed closed, even during busy hours. One raining Tuesday. One rainy Tuesday Sarah decided to try the door. The sign read "Nunas Cafe" in faded blue letters. To her surprise, it opened with a gentle chime. and elderly women with silver hair smiled warmly. One rainy Tuesday, Sarah decided to try the door. To her surprise, it opened with a gentle chime, inside an elderly woman with silver hair smiled warmly.
----------------------------------------
```



### Gave up, dedup too annoying. Switch to LLM
2025-06-28T16:02:38-07:00
Working quite nice
![[Pasted image 20250628213129.png]]


2025-06-28T21:30:59-07:00
`brew install blackhole-2ch`.

Have to setup this loopback because of stupid macos:
![[Pasted image 20250628214556.png]]

2025-06-28T22:09:14-07:00
Almost there, just need a multi-input device
![[Pasted image 20250628220923.png]]

Getting this:
- https://rogueamoeba.com/loopback/
- actually no, should just figure out how to capture system audio


2025-06-29T11:43:30-07:00
Debugging SDL device
```
(voice-mode-transcript) voice-mode-transcriptWentaos-Mac-mini:Voice_Mode_transcript wentaojiang$ ./whisper.cpp/build/bin/whisper-stream -m ./whisper.cpp/models/ggml-base.en.bin -t 1 --step 0 --length 1000 -vth 0.9 2>&1 | grep "Capture device"
init:    - Capture device #0: '???’s AirPods Pro'
init:    - Capture device #1: 'BlackHole 2ch'
```

```
(voice-mode-transcript) voice-mode-transcriptWentaos-Mac-mini:Voice_Mode_transcript wentaojiang$ python -c "import pyaudio; a=pyaudio.PyAudio(); [print(f'{i}: {a.get_device_info_by_index(i)[\"name\"]}') for i in range(a.get_device_count()) if a.get_device_info_by_index(i)['maxInputChannels']>0]; a.terminate()"
2: ???’s AirPods Pro
4: BlackHole 2ch
```

Run whisper.cpp:
`./whisper.cpp/build/bin/whisper-stream -m ./whisper.cpp/models/ggml-base.en.bin -t 1 --step 0 --length 10000 -vth 0.6 -c 0`

2025-06-29T11:50:16-07:00
This is clearly working!
Here is some youtube video:
```

### Transcription 0 START | t0 = 53 ms | t1 = 10053 ms

[00:00:00.000 --> 00:00:03.040]   Luna works on how to use it with an ESP32 and Raspberry Pi Pico.
[00:00:03.040 --> 00:00:06.120]   We'll also use a Windows utility to change the sensor's parameters.
[00:00:06.120 --> 00:00:08.320]   We're going to distance today, so welcome to the workshop.

### Transcription 0 END

### Transcription 1 START | t0 = 2121 ms | t1 = 12121 ms

[00:00:00.000 --> 00:00:03.600]   Raspberry Pi Pico will also use a Windows utility to change the sensor's parameters.
[00:00:03.600 --> 00:00:06.240]   We're going to distance today, so welcome to the workshop.

### Transcription 1 END
```


2025-06-29T17:20:31-07:00
Added a bunch of other functionalities.
Could use this for browsing the database:
- https://sqlitebrowser.org/dl/
![[Pasted image 20250629173030.png]]


