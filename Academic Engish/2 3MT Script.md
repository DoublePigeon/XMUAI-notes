Do you know that  your voice could be cloned in less than 1 minute?

Imagine this: You just got back home and received a call, then "Your mom" asked for a grand to a bank account. But when you texted your mom about this, she replied that she didn't call you back then. 

With the rapid development of AI audio generation models, spoof audio and deepfake scams like this has become alarmingly often. However, former methods to detect them either lack the reliability to capture general traces of forgery or can only detect certain types of deepfake traces. 

Therefore, we have developed a novel method to merge feature from multiple forms of audio to detect general traces of forgery while maintaining a remarkable reliability. 

Essentially, audio is nothing but a bunch of mixed waves that have different amplitude and frequency and can be represented into two forms - waveform and spectrogram. They are the two sides of the same thing, but may contain different traces of forgery.

So if we align these traces from different forms, we can gather the features from both form, thus allowing us to achieve much more accuracy in detection, and this is exactly what we are doing. 

After building our deep learning model architeture based on this idea, we have worked out a model that performs well in deepfake detection. We used two challenging deepfake audio datasets to test our model, achieving 98.1% accuracy in one of them, and 93.42% in another.

So with our latest method of audio deepfake detection, deepfake scammers have nowhere to hide. Next time someone you know calls you for a quick grand, you can stay at ease, knowing that we can stop the deepfakes from fooling you and enjoy the rest of the day.

