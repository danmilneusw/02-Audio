# 02-Audio
## Introduction
This session is all about audio. In the presentation we covered audio format types (WAV, MP3, OGG Vorbis etc), loudness (dB, dBFS), how digital audio works (mono/stereo/5.1, bit-depth, sampling rate), audio compression techniques (Nyquist-shannon theorem), how audio works inside a game engine (streaming, RAM), and techniques for audio compression inside the game engine (audio culling, priorities). If you want any reminders, the presentation is called **01 - Understanding Audio** and feel free to ask any questions too.

## Summary
- Understand the fundamentals of digital audio
- Learn audio optimisation techniques
- Learn game engine audio optimisation techniques
- Learn how to profile audio

## Tutorial
1. Open the Unity project **02 - Unity Audio Optimisation**. Do task 2 while it loads.
2. Open **02 - Visualising Audio.ipynb** in VS Code. Run the code to visualise the audio. Assess what the highest frequencies might be and test how different sampling rates affect the quality.
3. Profile the project: Open the Profiler (Windows > Analysis > Profiler). Add the Profiler Module for Audio (Inside the profiler go to Profiler Modules and tick Audio). Set the view to Detailed. Copy the information from the audio profiler, like shown here:

Total Audio Sources: 41<br>
Playing Audio Sources: 38<br>
Paused Audio Sources: 3<br>
Audio Clip Count: 8<br>
Audio Voices: 38<br>
Total Audio CPU: 1.3 %<br>
DSP CPU: 0.7 %<br>
Streaming CPU: 0.0 %<br>
Other CPU: 0.6 %<br>
Total Audio Memory: 46.5 MB<br>
Streaming File Memory: 0 B<br>
Streaming Decode Memory: 0 B<br>
Sample Sound Memory: 45.1 MB<br>
Other Memory: 1.4 MB<br>

<div align="center">
  <a href="Images\01 - Audio Profiler.png" target="_blank">
    <img src="Images\01 - Audio Profiler.png" alt="Audio Profiler" style="height:300px;"/>
  </a>
</div>

Also take note of the time per frame on the CPU usage, like shown here: 
<br>Time per Frame: 6 ms

<div align="center">
  <a href="Images/03 - CPU Usage.png" target="_blank">
    <img src="Images/03 - CPU Usage.png" alt="CPU Usage" style="height:300px;"/>
  </a>
</div>

<br>

You can learn more about the Audio Profiler Module at the [Unity Dosc](https://docs.unity3d.com/Manual/ProfilerAudio.html)

Now we have a recorded pre-optimisation performance. Your Total Audio Memory right now should be around 45 MB. This is quite high. In comparison, the Nintendo 3DS has 128 MB of RAM in total and the Nintendo DS only had 4 MB of RAM in total!

Your task is to reduce the Total Audio Memory to as low as possible. Less than 20 MB is good but you could maybe get less than 10 or 5 MB. You may increase an other metric by reducing Total Audio Memory, so be cautious on the impact you have on other metrics and assess the time per frame to see if there was any overall impact as a result. Of course make sure the audio quality is still good! Go through questions 4 and 5 to help you.

4. Using what you learned in the presentation and **[the official docs for audio clips in the inspector](https://docs.unity3d.com/Manual/class-AudioClip.html)** as reference, optimize the audio files in the inspector. Give it a go first, then take a look at the hints below to see if there's anything more you can do.

<details>
<summary>Hint for Q4 - 1</summary>
Can you Force to Mono? A stereo track has twice the data as a mono track. If a sound in your scene is playing from a single location (unlike the soundtrack), you can make it mono.
</details>
<details>
<summary>Hint for Q4 - 2</summary>
Set the compression format to from PCM (uncompressed) to Vorbis (compressed).
</details>
<details>
<summary>Hint for Q4 - 3</summary>
Override the sample rate or use Optimize Sample Rate. The less high frequency content your sound has, the lower the sample rate can be without losing too much quality (remember Nyquist-Shannon theorem).
</details>
<details>
<summary>Hint for Q4 - 4</summary>
Set the soundtrack 'Fantastic Dim Bar' to streaming mode. This will save loading this large file into RAM.
</details>

Do a quick profile before continuing. How do things compare to before?

5. In question 4 you optimised the audio, now you must consider optimizations related to the audio engine specifically. Create a mini audio middleware of your own that optimises audio performance. Have a go at thinking of your own ways first, I may have mentioned some ideas in the presentation. There's some hints below to give you ideas.

<details>
<summary>Hint for Q5 - 1</summary>
SFXFootstepsLooping is a 5 second plus loop. Use Audacity to edit this loop to be just one footstep sound or find a single footstep sound online and loop it. Write a script that applies randomised volume and pitch to create the illusion of using multiple footstep sounds (the Audio Source component for SFXFootstepsLooping is attached to the player game object).
</details>
<details>
<summary>Hint for Q5 - 2</summary>
Add audio culling: Set the priority of each each clip. Open Edit > Project Settings > Audio and set both thresholds to 1. When you run the game you will only head one audio source. What effect does this have on the audio metrics? Lower the number of voices to a value you think could work for the game and write an audio priority manager script that assesses the distance an audio source is from the player and increase their priority to ensure the most important audio is played. Remember to include a way to keep the music playing.
<br>
<br>

>**Real Voices**<br>
>"The real voice limit in Unity refers to the total number of actual audible sounds. For example, if you reduced the real voice limit to one, you would only be able to hear one sound at any one time. Once the real voice limit is reached, additional sounds are prioritised and virtualised."

>**Virtual Voices**<br>
>"Virtual voices are inaudible. They are sounds that continue to run in the background but will not be heard, even if they are within range of the Audio Listener. The benefit of a virtual sound is that it keeps the audio going in the background when there’s not a real channel free to play it. Once there is a spare channel, the virtualised voice becomes a real voice and picks up from the correct position, as if it had been playing this whole time. If the virtual voice limit is exceeded, the lowest priority sounds will be paused or won’t be started at all."
><a href="https://gamedevbeginner.com/unity-audio-optimisation-tips/">Source</a>
</details>
<details>
<summary>Hint for Q5 - 3</summary>
No more hints! Research any new techniques and write some scripts to manage the audio further and see if it has an effect in the audio profiler.
</details>

6. By how much did you reduce the Total Audio Memory? What is this as a percentage reduction? Did this have much effect on FPS and Time per Frame?

## Extra Resources
### Sourcing Free, Royalty-free Audio
Most royalty-free. All free but may require an account.
- [gamesound.xyz](https://gamesounds.xyz/)⭐
- [freesound](https://freesound.org/)
- [freeSFX](https://www.freesfx.co.uk/)
- [Ambisonic Sound Library](https://library.soundfield.com/)
- https://soundbible.com/
- [soundeffects+](https://www.soundeffectsplus.com/)
- [soundgator](https://www.soundgator.com/)
- [ZAPSPLAT](https://www.zapsplat.com/)
- [99 Sounds](https://99sounds.org/sounds/)
- [Audio Micro](https://www.audiomicro.com/free-sound-effects)
- [Parters in Rhyme](https://www.partnersinrhyme.com/pir/PIRsfx.shtml)

### Synthesise Your Own Audio
- [Leshy SFDesigner](https://www.leshylabs.com/apps/sfMaker/)
- [Pure Data](https://puredata.info/)

### Unity
- [Unity Docs - Audio Profiler Module](https://docs.unity3d.com/Manual/ProfilerAudio.html)
- [Unity Docs - Audio Files](https://docs.unity3d.com/Manual/AudioFiles.html)
- [Unity Docs - Audio Files 2 (I think this is the older equivalent of the link above but has more depth in some areas)](https://docs.unity3d.com/352/Documentation/Manual/AudioFiles.html#:~:text=Depending%20on%20the%20target%2C%20Unity,let%20Unity%20do%20the%20encoding.)
- [Game Dev Beginner - Unity Audio Optimisation Tips](https://gamedevbeginner.com/unity-audio-optimisation-tips/) ⭐
- [Game Developer - Getting More Bam for your RAM](https://www.gamedeveloper.com/audio/unity-audio-import-optimisation---getting-more-bam-for-your-ram)

### Unreal Engine 5
- [Unreal Engine 5 - Audio Engine Overview](https://docs.unrealengine.com/5.3/en-US/audio-engine-overview-in-unreal-engine/)

### Audio Middlewares
- [Wwise](https://www.audiokinetic.com/en/products/wwise/)
- [FMOD](https://www.fmod.com/)