## Individual Assignment 1: XR Teardown — Job Simulator (2024 Vision Pro / Quest hands-only release)
5025231281 - Akhamar Elnath Chaniago - S1 Informatics

| | |
|---|---|
| Subject | Job Simulator: The 2050 Archives specifically its 2023+ major update: the hands-only visionOS launch and matching Quest hand-tracking update |
| Publisher / manufacturer | Owlchemy Labs (Google) |
| Release or major update | Original: Windows 5 Apr 2016, PS4 13 Oct 2016. **Qualifying 2023+ update: Apple Vision Pro launch + Quest "hands-only" update, both 28 May 2024** — the first time the title was playable start-to-finish with no controllers |
| Platform(s) | Apple Vision Pro (visionOS) |
| How I examined it | Documentation and Articles |
| Hands-on date(s) | - |

**My claim in one sentence.** To preface, Job Simulator by no means is a new title. It released on 2016, but had a new updated version when it was ported over for the Apple Vision Pro in 2024. Job Simulator is built around controller buttons for grabbing objects and doing the tasks, but once it was ported over, it had to be retrofitted with camera-based hand tracking to survive on the Apple Vision Pro that had no controllers. The visible seams of said retrofit are more informative than anything in the original release. 

## 1. Device class

Job Simulator targets six-degrees-of-freedom (6DoF) VR headsets across three ecosystems: PC VR via SteamVR, PlayStation VR on console, and standalone on Meta Quest. The subject of this teardown is its newest branch: the Apple Vision Pro port and the matching Quest update, both shipped 28 May 2024. Vision Pro is significant here because it was, in the words of a Game Developer report on the port, "the first major six degrees-of-freedom headset to ship without controllers" (Refer to Article 2). That meant Owlchemy could not simply port the existing PC/PSVR control scheme, hand tracking stopped being an optional accessibility layer and became the only way to play at all on that platform.

Despite Vision Pro's passthrough and spatial-computing capabilities, Job Simulator itself sits close to the virtuality end of the reality–virtuality continuum. The player's view is fully replaced by a stylised job-museum environment; the game is launched as a fully immersive VR title, not a passthrough or windowed spatial app, so the physical room plays no role once play begins.

This device-class choice has a direct consequence for input: because there is no physical controller to standardise around, and because Vision Pro and Quest use different camera hardware and different tracking update rates, Owlchemy had to build hand tracking that behaves consistently across two separate optical tracking stacks rather than one.

## 2. Input modality

**What the user does.** The hands-only build removes controllers entirely and maps the player's real hand movements directly onto in-game hands: pinching to grab objects, releasing to drop them, and using ordinary gestures to operate props inside the job simulations (stacking, throwing, ripping apart, serving). Object interaction that used to be triggered by a controller button is now triggered by a tracked pinch gesture instead.

**Why this and not that.** The obvious alternative, to keep using motion controllers, was in fact still shipped on Quest as a parallel option, so this is not a case of hands replacing controllers outright; it is hands being added as a first-class alternative. On Vision Pro, though, there was no alternative to offer, because the platform itself has none. Owlchemy's CEO framed the underlying philosophy bluntly: "The controllers are a barrier" (Refer to article 4, quoting Andrew Eiche). That framing explains why the studio treated hand tracking as a strategic bet rather than a novelty feature well before Vision Pro existed.

**Where it fails.** The retrofit exposes a hardware mismatch that the original 2016 design never had to deal with. According to a Game Developer interview with Owlchemy's engineers, camera-based hand tracking on Vision Pro updates at roughly 30Hz while the rest of the game renders at 90Hz, so "for every frame containing hand pose data, there would be at least two frames with no updated data at all" (Refer to article 2). Because a game built around precise grabbing needs a hand position every frame, not once every three, this gap is functionally a tracking failure mode baked into the platform, and it shows up most during fast throwing or catching motions — exactly the kind of slapstick, fast-paced interaction Job Simulator's comedy depends on.

**What I would change.** Owlchemy already addressed the frame gap with motion prediction rather than simply freezing or blending old poses. I would go one step further and expose tracking confidence to the player visually — a faint outline change on a hand when the system is extrapolating rather than measuring — so that a missed grab reads as "the system predicted wrong" rather than "I did it wrong," which matters in a game whose whole appeal is that nothing you do has real consequences.

## 3. Use of AI

I found evidence of AI in the perception layer supplied by the platform, and evidence of a separate, non-AI predictive technique built by Owlchemy on top of it, however there were no evidence of a generative AI system, LLM, or AI-driven content pipeline inside the game itself.

| Where | What it does | On-device or cloud | Cost it carries | Source + the line I am relying on |
|---|---|---|---|---|
| Perception | Estimates hand skeleton/pose from headset cameras so pinch and grab gestures can become game input | On-device (visionOS ARKit hand tracking on Vision Pro; Quest hand tracking on Quest) | Continuous camera processing, battery/thermal load, and accuracy loss under occlusion or fast motion | Owlchemy Labs launch announcement — "Owlchemy Labs has always been committed to pioneering hand tracking technology" |
| Motion prediction (extrapolation) | Fills the gap between 30Hz tracking updates and the 90Hz render loop by predicting where the hand is going, rather than freezing or blending stale data | On-device, built by Owlchemy inside a modified Unity XR VisionOS package | If a movement changes direction unpredictably between real tracking updates, the predicted hand position can diverge from the real one | Game Developer deep dive with Owlchemy engineers — "we opted to use extrapolation to predict where the hands were going to be while estimating where the next 'fresh' hand pose would be" (Refer to article 2) |

I distinguish these two layers deliberately due to the fact that the first is genuinely a machine-learning perception system, but it belongs to the platform (Apple's or Meta's), and not to Job Simulator. The second, however, is Owlchemy's own code, and while "extrapolation" is a predictive technique, it is closer to classical motion estimation than to a trained AI model. During my research, I found no documentation, interview, or store listing suggesting Job Simulator uses an LLM, speech-recognition-driven characters, or generative content of any kind.

## 4. Impact

**Intended benefit.** The 2024 update's stated goal was to remove the last major barrier to entry — the controller itself — for a title an UploadVR hands-on piece frames as "the mainstream VR minimum quality bar for hand tracking interaction going forward" (Refer to article 6). In practice this means a first-time VR user on Vision Pro can pick up the headset and immediately manipulate a virtual world with their bare hands, with no controller pairing, button mapping, or grip-strength requirement standing between them and the joke of the game.

**Privacy, security, or ethics.** The sensor data this app actually needs is hand pose and hand geometry, captured continuously by the headset's cameras while hand tracking is active. Because Job Simulator is a fully immersive VR title rather than a passthrough/MR experience, I found no evidence it requires raw passthrough camera frames, room meshes, or environment scans — the same distinction Maestro's teardown makes. The privacy question that is specific to this title, though, is that it now runs on two different platforms' hand-tracking stacks (Apple's and Meta's) under two different developer data policies, so the same gesture data is governed by different rules depending only on which headset the player owns — a fact easy to miss because the gameplay looks identical on both.

**Accessibility / human factors.** The Quest branch of this same 2024–2025 update cycle also shipped audio-description accessibility, letting low-vision players receive "game information through audio descriptions using Text To Speech" (Refer to article 3), plus a high-contrast Vision Assistance Mode. Taken together with the hands-only mode, this update actually widens access on Quest, because controllers remain available there as a fallback for players who cannot reliably perform pinch gestures. Vision Pro is the harder case: since the platform has no controller at all, a player with limited hand or finger mobility has no fallback input method on that specific hardware, which makes Job Simulator's accessibility story platform-dependent rather than a single, portable answer.

## 5. What I take from this

Job Simulator, I believe, is quite the interesting teardown for this assignment. It shows what happens when a game that is built around a certain architecture being forced to, for a lack of better word, metamorphosis into a hardware/system that was built for another. The interesting topic to talk about/tackle here is the fact that Owlchemy had to sew the project back up to hide the tracking-rate mismatch the original game from 2016 never truely had a problem with. Its quite the humbling reminder that the word "supports hand tracking" represents more so a "compatibility" rather than just another feature that the devs can add later on.

## References

1. Owlchemy Labs. (2024). *Owlchemy Labs Launches Two Best-Selling Titles, Job Simulator and Vacation Simulator, On Apple Vision Pro.* Primary developer press release confirming release date, pricing, and platform framing. https://owlchemylabs.com/blog/owlchemy-labs-launches-two-best-selling-titles-job-simulator-and-vacation-simulator-on-apple-vision-pro
2. Nunneley-Jackson, S. / Owlchemy Labs engineering team. (2024). *Deep Dive: How Owlchemy adapted its VR titles for the Apple Vision Pro.* Game Developer. Primary technical source: named senior engineers describing the hand-tracking extrapolation implementation. https://www.gamedeveloper.com/programming/how-owlchemy-brought-their-vr-titles-to-the-apple-vision-pro
3. Owlchemy Labs. (2025). *Job Simulator, Vacation Simulator and Cosmonious High now available on Meta Quest 3.* Primary developer blog confirming Quest 3 listing and accessibility feature set. https://owlchemylabs.com/blog/job-simulator-vacation-simulator-and-cosmonious-high-now-available-on-meta-quest-3
4. Peckham, M. (2024). *Job Simulator Now Supports Hand Tracking On Quest.* UploadVR. Reports the same-day Quest hands-only update alongside the Vision Pro launch. https://www.uploadvr.com/job-simulator-quest-hand-tracking-support/
5. Owlchemy Labs. (2024). *Job Simulator — Steam store page.* Store listing confirming platform support, VR-only requirement, and accessibility claims. https://store.steampowered.com/app/448280/Job_Simulator/
6. UploadVR. (2024). *Hands-On With Job Simulator On Apple Vision Pro.* Hands-on impressions piece framing the release's significance for controller-free VR. https://www.uploadvr.com/job-simulator-hand-tracking-apple-vision-pro-hands-on/

## Figure credits

- Fig. 1 — 
- Fig. 2 — 
- Fig. 3 — 

## AI-assistance disclosure

**What I used, and for what.** I use Claude as the main AI that I use. The ai is predominantly used for proof-reading the essay and for minor fixes overall. If further checking at a later time were to invalidate the points/claims that were in this repo, I do apologize for the oversight on my end.

Further clarification, I was unaware that there was a specific rule about committing the assignment with atleast 3 commits in atleast the last 2 days. I do sincerely apologize for my tardiness and will strive to be better for the assignments moving forward.

**What I disagreed with my AI assistant about.** 
