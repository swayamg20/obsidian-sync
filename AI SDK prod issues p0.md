[[UI interactiron]]
- [x] iOS mic issue
- [ ] onboarding go through (no voice) 
- [x] lower-end android device lag issue (due to huge lottie and framer-motion) 
- [x] smoothness issue
- [x] keyboard in text view, submit button hidden
- [x] after submitting on text mode, change modeStatus to fetching result p1
- [x] fab button location height-wise changing p0
- [ ] do not show fab on confirmation page / review page (location.pathname)
- [x] tap to speak repeating
- [x] audio getting twice p0
- [x] ig getdeepgramkey response fails then it should not show i'm listening...
- [x] on clicking outside the mic also should stop
- [x] on switching from tab (if the browser is not opened) then the audio/mic should get stopped
- [ ] auto closing the full view
- [ ] prioratize API Calls
- [ ] if user says in Hindi, audio reply should be in Hindi and vice versa
- [ ] fillers should be in Hindi also
- [ ] implementation of clarity 
- [x] login auth token issue
- [ ] mic permission prod issues
- [ ] flicker on logged in and our API failure

how should mode status work:

| State/Trigger         | Display Text              | Condition                                                                                                                                                    |
| --------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Initial State         | "idle"                    | When FAB is opened/visible                                                                                                                                   |
| Mic Start Preparation | "getting things ready..." | When tap-to-start-mic is triggered until mic actually starts                                                                                                 |
| Active Listening      | "listening..."            | When microphone is actively listening for user input                                                                                                         |
| API Processing        | "fetching results"        | 1. When speech-to-text is complete and response API is hit<br>2. When suggestion is clicked and API is hit<br>3. When text input is submitted and API is hit |
| Audio Playback        | No text shown             | While response audio is playing                                                                                                                              |
| Post-Audio            | "idle"                    | When audio playback completes                                                                                                                                |
[[review]]

```[](<- This rule will be used only when required to do the package release
- This is a mono repo with two packages: voa-hotels and voa-utils check @package.json
- I am using pnpm changeset for version control
- When called this rule, you need to analyze the git diff -%3E then check which package needs updates -> bump up their version accordingly (major/minor)

- For this you need to create a .md file inside .changeset folder in the root directory of this project.

- The name of the file can be anything like lowercase, hyphen seperated.

Below is the example of the file
```
---
"@ixigo/voa-hotels": major/minor
"@ixigo/voa-utils": major/minor
---

%3CSUMMARY>

```
- You need to fill this file accordingly, DO NOT add the packaged which is not required to be bumped. Decide major/minor from the git diff. 

- Save this file.

- After the above step, run this command: `pnpm changeset version`

- finally do `git add .` then `git commit -m "version: <VERSION>"` then `git push`>)
```


197: delhi
2651: dubai

design feedback:

initBar to fab on scroll 
initBar -> text inc, logo size inc
initBar -> animation brightness / visibility inc
fab -> outer glow remove (on idle) P0
nudge -> shimmer speed dec, shimmer stopping on "long press...."
fab -> animation review (shimmer, glow)
dismiss -> hold and drag then only show
orb -> do not grow size
layout -> gap b/w chips and text should be low (20px around)
chips -> do not change vertical position, on text change
text -> do not change vertical size on line change, inc
text -> perplexity reference (faded)
nudge -> appearing tap to speak when listening
text mode -> blur height increase
text mode -> ai logo size check
text mode ->  input text size
text mode -> mic size inc
ripple -> also on the long press and any tap on orb

[[2025-03-26]]


[[2025-03-26]]

- [ ] on text input mode, buttons working should be there
- [ ] on input mode, on enter submit
- [ ] tap to speak text / orb blink
- [ ] filter functions not coming up on first search
- [ ] without text, suggestion should be a bit above
- [ ] if asked about MMT, or major OTAs. it should emphasis on ixigo only

Design changes:

- [ ] Searched for bangalore, got answer for gurugram in new session
- [ ] on long tap, keyboard should open automatically
- [ ] mic icon size
- [ ] gap between text and filters
- [ ] text upper boundary fading
- [ ] incorporate interactions
- [ ] init Bar should stay until its scrolled
- [ ] initBar only on homepage
- [ ] initBar animation
- [ ] on auto trigger, sometimes its not speaking up
- [ ] the text should stay there if reopened fab
- [ ] text should be synced with audio
- [ ] listening is for quite long time sometimes
- [ ] dismiss ai sdk modal
- [ ] dismiss AI animation
- [ ] dragging is slow
- [ ] differentiate with dragging and longpress drag 


```

```


- [ ] do not do anything on URL empty (deeplink function)



[[2025-05-27]]

- [ ] chips getting cut p0
- [ ] first complete current playing audio then play the second one (if any audio is playing currently) p0
- [ ] widget: icons not coming
- [ ] widget should not be coming on fetching results p0
- [ ] intermediate state, when its in filler state (and widget was there), then the gradient is on full screen view p0
- [ ] currently viewing was not coming
- [ ] hotel Id was not coming
- [ ] icons need to be updated
- [ ] dragging not smooth on some iPhones (17.5.1)
- [ ] AI SDK disappear is not smooth
- [ ] few css are not working on few iOS devices
- [ ] sometimes gradient comes a bit late (very small delay but still)
- [ ] VAD configurations (listening time, hold time)
- [ ] microphone active timeout
- [ ] getConfig multiple calls (only 1 call should be there)
- [ ] suggestion chips not coming on triggers
- [ ] deepgram fallback support

flights:

- [ ] scroll activity not working

[[2025-07-14]]
- [ ] comparison widget
- [ ] promise native bridge work for android
- [ ] orb init animation
- [ ] 

