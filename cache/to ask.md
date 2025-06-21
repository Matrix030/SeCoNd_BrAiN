
What are you looking for in an intern? As an intern, where would i have the most meaningful impact.

since you are not using any heavy weight AI models, are you using the biometrics/bioprint to compare the live video feed to made it more robust?

what are your thoughts about deep live cam, i saw you made a video about it and to be honest 

Honestly, it’s really cool what you’re building at Validia. Helping fight social engineering in such a hands-on way really makes a difference

“Thanks again for the call yesterday — I really appreciated that.  
I’d actually love to hear: what stood out to you in my resume or background? I’m especially curious because I’ve been trying to align my projects — like SteamLens or the distributed NLP work I did — toward roles at the intersection of AI, security, and scalable systems, which seems to line up with what you’re doing at Validia.”

# World Hacker Games Featuring Validia
started with a snapchat filter and now it is getting out of hand to do malicious things.
I was just watching your CTF you had a month ago on youtube,
what did you mean by there is not one solution, one model or one form of detection that is going to give you high success rate.

4 modules (what is the 4th?)- 
biometrics - facial and audio identify people and rule out deepface

standalone deepfake detection - hyper-tuned to look for any sort of facial forgery or any sort of audio deepfake being used. model that was trained by them. Once  forgery is detected it is made next to impossible to remove that and pass the check. Must pass at the beginning otherwise the check is destined to fail. Zoom Filters don't affect the detection. Specifically added in the Elements during building of model (touch up effect, blurred backgrounds)

at what time do you flag a person as an adversary? - completely configurable (30 to 300 seconds). Default (Enterprise)- flag it to everyone in chat that someone is not authenticated within a 2 minute time frame. But there are additional forms of authentication that the user can carry out.

there is also liveness - Looking for - is there a real person on the screen, built almost as a second factor, because their tech does great job in live scenario by matching biometrics, this is a fallback as it might be a video instead (which might pass as it is a real version of the actual person) of a real person. This module is built in to make sure that if  someone is actually present on the screen, its not a recorded video or a photo being played.


can determine location, use of VPN proxy TOR etc, to improve security.

J - Wanting to hit on a lot of different datapoints and not just a single model (Truth Algorithm) - A nearly impossible task 
Their Approach - Steadily build more and more indicators overtime to really show the threat and pick up signals 


Background - continuous monitoring (video and audio)
AI generated photos - if used in biometric creation (could break the system) one area of vulnerability (way through the product)

All onboarding is done through platform with additional deepfake check as people create things there.
On Page - this person has been authenticated as an individual within the organization, they also attach the associated profile.


voice, liveliness and biometrics (DeepFake Tool Metric??)
![[Pasted image 20250619110718.png]]



audio confidence went down with more and more authentication

secure authentication:
![[Pasted image 20250619110912.png]]

CTF - What they did:
save image for face clone - inject into webcam. more recon for better quality
sent a phising email to get an onboarding meet link
 joined the meeting, 
 passed the liveness detection.
audio passed.



It was great to see how robust your system is right now.
don't just blacklist the domain, go all the way to the root. 


The fact that you put your product in front of hackers on a live show, really shows the confidence that you have in the product. 