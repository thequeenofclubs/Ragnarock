## Architect DSP: A Deep Dive
Architect is my codename for the DSP that powers Ragnarock. It's why Ragnarock is able to produce the sound quality and volume that it does.

## Hardware
The DSP runs, at its core, on a Raspberry Pi. It's paired with a HiFiBerry DAC + ADC combo, allowing the pi to control the amplifier directly, as well as receive analog audio from the 3.5mm jack on the back of the speaker.
The Pi splits the audio signal into two channels. One channel contains the bass tones of the signal, and the other contains the treble. The midrange (tones in-between bass and treble) are divided between the two at a specific point, known as the crossover frequency. 

The HiFiBerry unit allows the Pi to send and receive 24-bit @ 192khz audio signals for processing.
Internally, the Pi processes in 24-bit @ 384khz.

## Software
Architect is a software program. Its goal is to accept an input signal, apply adjustments to that signal, and then output the adjusted signal to the amplifier.
It must do this with as little latency as possible, to avoid desynchronization between audio and video while watching content or using a microphone.

## Linear Response Tuning
Architect knows the adjustments that need to be made to correct for the speaker cabinet & drivers themselves. Any speaker driver regardless of cost inherently applies "colouration" to a signal. This introduces nonlinearities in that signal. Quite literally, it makes different parts of the audio signal louder than others. The wooden cabinet of a speaker has a similar effect, to an even greater degree. Architect is pre-programmed with these nonlinearities, and actively corrects for them by applying boosts or reductions to the effected portions of the audio signal. This produces a pleasing, neutral, high-fidelity sound.

## Compression-Based Processing
Architect uses compression-based processing to adjust the incoming audio signal. Compression is a technique which allows Architect to variably reduce the level of specific portions of the audio signal. It determines how much reduction is required, the rate at which this adjustment is applied, and which portion of the audio signal this reduction gets applied to in real-time. 

Typical DSP uses fixed adjustments like EQ and filtering to adjust the audio signal, but these are inherently flawed, as they do not factor in the context. A compression-based adjustment is essentially self-aware. It gets applied only when needed, and steps aside otherwise. It applies a dynamic amount of boost or reduction depending on how far over the threshold the input signal is. E.g. A signal with a huge bass boost will receive massive reduction in that range, while a signal with no bass whatsoever will not receive any such reduction. A fixed adjustment applies that same massive reduction whether the signal needs it or not.

## Driver Protection
Architect uses 19 bands of compression per-channel, for 38 total bands, to protect the speaker drivers. The linear response tuning mentioned above requires applying large boosts to certain portions of the signal, which can potentially cause the drivers to exceed their safe power or excursion (in-out travel) limits. This is... bad. It will slowly damage the drivers, introducing distortion, scratchiness, and eventually a total seizure. When this happens, the driver is considered blown and is rendered useless. 

Architect avoids blowing the drivers by watching the audio signal after it has been adjusted for linear response.
If any portion of the signal exceeds the physical capabilities of the speaker driver, Architect dynamically and transparently clamps the signal to that safe value, protecting the driver without noticeably affecting the audio signal.
It can do this for 19 frequency bands for each channel. 

When the driver is reaching its physical limits, it's caused by the lowest tones being too low (subwoofer) or highest tones being too high (tweeter). The protection system solves this by clamping the most demanding tone ranges to a level which can be safely maintained by the driver, while allowing all other ranges to continue rising in volume. In essence, Architect trades extension for volume, but only when the volume is actually needed.

## Psychoacoustic Processing
The human ear is not a perfect sound sensor. There is a massive difference in sensitivity to different tones, and if not accounted for, often results in a speaker that sounds "thin" or "shouty".
The problem is that the sensitivity is dependent on the volume. If the sound level is very high, the difference in sensitivity is relatively small. This is why concerts and clubs sound deep, punchy, and full. At lower volumes, the difference in sensitivity skyrockets.
Architect takes this discrepancy into account, dynamically applying boost to the regions the human ear is less sensitive to, based on how loud the volume is. Lower volumes result in much bigger boosts, and very high volumes get little to no boosting.

## Adaptive Crossover
This is something you will never notice, but it's an important feature of Architect.
Since Ragnarock has two channels, a LF channel (bass) and an HF channel (treble), we must determine where to divide the signal. As I mentioned before, this point is known as the crossover frequency. Frequencies above it are sent to the HF channel and frequencies below it are sent to the LF channel.
The problem lies in driver capability. The large W6 driver is great at playing bass, but terrible at playing treble. The inverse goes for the dual tweeters. They are great for treble but horrible for bass. They're simply built for different purposes.
Therefore, we must distribute the sound such that the drivers play only what they can handle. As with all things, this ability is dependent on the output level.
The 2 inch tweeters can play deep bass, but only very quietly before they exceed their physical capabilities. 
The large 7 inch subwoofer can play treble, but also only quietly. 
This is where the adaptive crossover comes in. When playing at lower volume, the tweeters take on more of the bass energy, since they're able to spread the sound wider and play clearer than the subwoofer at this low volume. In this case, we cross over around 250hz, which is the frequency of a typical male singer voice.

When volume rises, the crossover slowly shifts upward, offloading more and more of the midrange onto the subwoofer, since the tweeters cannot play those tones at high volume without distorting.
At its highest, the subwoofer plays up to 1500hz, roughly equivalent to the ringing of a glass.

## Bass Remapping
The subwoofer in Ragnarock can play very deep in the frequency range, down to around 28hz before output collapses and power requirement skyrockets. To get around this limitation, I developed a technique in which Architect can use a modified PWM signal to trick your brain into hearing lower tones, without overstressing the driver. 

Instead of playing the tone directly, I choose a deep tone (often around 30hz) and modulate the volume of this tone to match the initial tone. In effect, the 30hz tone's amplitude is directly linked to the frequency of the incoming signal. As an example, let's take a 5hz tone being inputted into Architect. Insted of sending 5hz to the driver, which would instantly destroy it, Architect plays 30hz but turns it on/off at 5hz. The input signal rides on the 30hz carrier frequency.

This allows Architect to extend perceptual bass response down to 1hz. 

## Electromagnetic Force Braking System (EMFB)
A speaker driver is at its most simple level an electromagnetic device. It follows properties closely related to an electric motor. Thanks to my custom amplification architecture, I can use Architect to directly control the driver cones, by shorting their terminals to ground. This causes an instantaneous braking force on the cone, violently cancelling their inertia. This allows for more precise control for the tweeters, allowing them to produce higher tones than they would otherwise be capable of. Additionally, it allows the subwoofer driver to directly control the passive radiator. This allows the speaker to cancel out the boominess and lack of tightness that usually accompanies a passive radiator based system, by pneumatically coupling it to the subwoofer cone. 

## Telemetry System
Architect determines which corrections, protections, and processing is needed based on the physical conditions being experienced by the drivers. It does this by tracking the following data:
- Total system power consumption (Output voltage * amplifier gain coefficient / driver resistance)
- RMS power per driver
- Amplifier output voltage
- Excursion Clearance (How much further the driver can move before hitting the end of its travel)
- Per-band strain coefficient (How much headroom is left in any given frequency range)
- Current crossover frequency
- Per-band Boost/Reduction (Compression Activity)

This allows Architect to recalculate compression thresholds in real-time, adapt to driver aging, and prevent damage without the user noticing. These data are then used to protect the drivers, and maintain safe operation.

## Operating System
Architect runs in bare metal (pure RISC-V ARM assembly) on the Raspberry Pi, using cores 0-2. 
The workload is divided between the cores, which is how Architect can do so much processing so quickly. 
Core 0 handles tuning, for both linearity and psychoacoustics. Core 1 handles the compression bands. Core 2 handles the interaction between ValOS and Architect, as well as collecting and managing the telemetry data.

Architect is the just correction software. It runs in tandem environment I call ValOS, which runs on core 3 of the Pi. 
ValOS is a custom revision of Debian that operates on a shared-memory architecture.
ValOS Hosts a WebUI which allows for modification of parameters, tweaking tuning, and monitoring the aforementioned telemetry data for debugging or analysis. 

ValOS supports remote access via the WebUI, via VNC for graphical desktop tasks, or via SSH for quick fixes.
This allows for toggling between tuning profiles, adjusting tuning without reboots, and allows for per-input tuning profiles.

Additionally, ValOS handles the bluetooth and AirPlay servers, which allow Ragnarock to be wirelessly connectable.

It interacts with the Architect software itself via a shared memory architecture. Memory is shared between the two, for low-latency, high-speed communication between the two. ValOS places the audio samples from Bluetooth or AirPlay directly into a fixed location. Architect then copies that audio, on a sample-by-sample basis directly into a register for processing. Once complete, that sample is stored in the memory address which the DAC uses. 

This allows for a very low latency signal chain despite the massive amount of processing.
