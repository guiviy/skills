# Lip Sync and AI Dubbing

## Pick the pipeline before you generate anything

- Decide the dialogue lines first, then the voice, then the video shot.

- Check that the shot shows the speaker's face large and mostly front facing, because lip sync models redraw the mouth region and fail on tiny faces, side profiles, and occluded mouths.

- Treat clips from `media.generate_video` as silent picture plus placeholder sound, and plan to replace the audio track after lip sync.

- Choose the voice route by language first, because Chinese dialogue favors Fish Audio and GPT-SoVITS while ElevenLabs favors English and multilingual library voices.

- Clone only voices the user owns or has permission to use, and never clone a real actor, singer, or public figure without consent.

## Generate the voice

- Write each line as its own audio file, one file per shot, so a retake never forces a full re-render.

- Keep each line under the shot length, and leave a half second of silence at the head and tail for the lip sync model to settle.

- Use ElevenLabs for a large library of ready voices and for fast API generation, and use its Instant Voice Cloning on a paid plan when the user supplies a reference recording.

- Use Fish Audio for Chinese-first dialogue, for its 10 to 45 second sample cloning, and for fine control of speed, pitch, and emotion tags in the text.

- Use GPT-SoVITS locally only on a machine with an NVIDIA GPU of at least 6 GB VRAM, and use its zero shot mode with a 5 second sample for a quick test or its few shot mode with 1 minute of clean audio for a keeper voice.

- Store every voice under a character name, such as Lu Yun or Zhang Aoxue, and reuse the same voice id or model for every episode of a series.

- Check every generated line by listening to the waveform length and by reading the transcript back, because a misread polyphonic character ruins a whole shot.

## Run the lip sync

- Feed the video clip and the matching audio file into one lip sync model, and never feed a full episode in one pass.

- Use MuseTalk for fast drafts and for dialogue heavy batches, because it runs in real time on modest GPUs and its code carries an MIT license.

- Use LatentSync for final close up shots, because its diffusion output is sharper and temporally steadier, and budget a GPU with at least 8 GB VRAM for version 1.5 or 18 GB for version 1.6.

- Use the two stage pattern when volume and quality both matter, and draft every cut in MuseTalk before re-rendering only the adopted cuts in LatentSync.

- Avoid Wav2Lip for published work, because its open weights are limited to research and personal use and its face resolution is low.

- Use SadTalker only when the input is a single still image rather than a video clip, and use InfiniteTalk when the shot also needs head and body motion.

- Use a hosted service such as sync.so or Hedra when no GPU machine is available, because hosted lip sync takes video plus audio through a browser or API and returns a finished clip.

- Run the lip sync on a GPU machine or a hosted service rather than on the Muse VM, because the VM has no GPU and local diffusion lip sync on CPU is impractically slow.

## Assemble and check the result

- Mux the lip synced video with its audio using `ffmpeg`, copying the video stream and mapping the new audio stream, and cut with the shortest stream when the audio is shorter than the picture.

- Check P, B, and M sounds close the lips fully, check the mouth stops moving during silence, and check teeth stay sharp at the start of words.

- Check the whole shot for flicker around the mouth and chin, and re-render a flickering shot in LatentSync rather than trying to patch single frames.

- Check that the voice, face, costume, and background still match the series bible for that character before delivery.

- Deliver the finished clip as a file attachment, and name the voice model and lip sync model used so a later episode can repeat the exact combination.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Websites

- [ElevenLabs](https://elevenlabs.io/) for library voices, Instant Voice Cloning, and API text to speech. Carry the voice id and model id into the API call, and verify the current price on its pricing page before quoting a cost.

- [Fish Audio](https://fish.audio/) for Chinese-first voices, short sample cloning, and WebSocket streaming synthesis. Carry the voice id and emotion tags into the API call, and verify the current price on its pricing page before quoting a cost.

- [GPT-SoVITS](https://github.com/RVC-Boss/GPT-SoVITS) for free local voice cloning with a WebUI on port 9874. Carry the reference audio and its transcript into the WebUI, and confirm the GPU requirement before starting.

- [sync.so](https://sync.so/) for hosted lip sync without a local GPU. Carry the video file and the audio file into the browser or API job, and verify the per second price before quoting a cost.

## Alternatives for better results

- Generate the video shot with the character facing camera and speaking, rather than trying to lip sync a back view or a wide shot where the face is small.

- Record or pick the voice before generating the video, so the shot length and the actor's pauses can match the line instead of stretching audio to fit picture.

- Use the spoken audio skill for plain narration or voice notes that need no moving lips, and reserve this pipeline for shots where the audience sees the speaker talk.

- Split a long speech into short lines across cuts, because short lines sync more reliably and let one bad line be replaced without redoing the scene.
