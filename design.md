## Design
Ragnarock wasn't built in a day. This document will outline everything about the speaker, its components, the design decisions, and even a timeline since there were many revisions over the years.

## Hardware Choices
I chose the W6-1139SIF Subwoofer driver since I needed something that was durable, had lots of travel (Xmax), good sensitivity, but without consuming too much power. The W6 has 12mm of Xmax, is very sensitive (produces 84dB with only one watt of power), and needs only 50 watts to reach its full potential. Comparable subwoofers require up to 400W just to match its performance.

I chose the two tweeters for their sensitivity. They can produce 96dB with one watt of power, allowing them to reach extreme volumes with low power and therefore low heat. They also have paper cones, which reduces the amount of correction that needs to be done to the audio signal (see [here](/architect.md/#linear-response-tuning) to learn more about this process).

I chose the amplifier because it has a fixed-gain, high efficiency architecture. It allows for predictable output from the drivers given a specific output from the DSP. Without this, the DSP would have no idea what sound the speaker is actually producing!

For the cabinet, I chose to use Aspen wood, bonded with mitre-cut joints. Aspen has desirable acoustic properties, primarily balancing physical strength with workability and acoustic neutrality. In layman's terms, it was the wood that is the strongest without messing with the speaker's sound quality.

## Cabinet Design
A speaker is a complex device. To produce good sound, it must have the perfect synergy between the cabinet and the drivers. The cabinet looks the way it is because I wanted to achieve a specific internal volume (9.5 litres in this case) with the smallest possible footprint. 9.5L is a key measurement, because it's exactly the correct volume such that the pressure generated inside the box by the subwoofer provides the resistance it needs to produce the best sound. With a volume that's too big, there wouldn't be enough resistance to the subwoofer, and it would slowly destroy itself. Too small of a volume is also bad, since it limits how efficient the subwoofer is at turning electricity into sound. 

Ragnarock's Cabinet is also cross-braced, which limits rattling in the cabinet, and damped with fiberglass, reducing bloom and echoes. 
The cabinet also employs a passive radiator, which is used to offload work from the subwoofer while simultaneously lowering the resonance frequency of the cabinet. The resonance frequency of the cabinet is the frequency at which the efficiency of the entire system is highest. In other words, it's the frequency at which the cabient itself produces the most sound. 

The passive radiator tunes the cabinet to 32hz (versus ~61hz if it was simply sealed). This allows the subwoofer to extend deeper for a better sound. Since the passive radiator also contains a cone and suspension, it is able to offload work from the subwoofer. Rather than having the subwoofer itself produce all the bass, the passive radiator becomes pneumatically coupled to the subwoofer in the bass range. This coupling gets stronger the lower the frequency goes, to the point where at tuning (32hz), the subwoofer itself barely moves, while the passive radiator is producing all the bass. Above the tuning frequency, they can work together, producing more bass total than either one could produce on their own.

## Timeline
Ragnarock's hardware and software have both had long histories, and they're mostly separate. Here are the timelines of both, and I'll link them together where it makes sense.

## Hardware
Ragnarock started as an entirely different speaker, then named Skybreaker. It was a massive 19L cabinet, with a 4.25 inch woofer. That woofer was rated for 20 watts, and had a measly 4mm of Xmax. Skybreaker had the same dual tweeters that Ragnarock had today. All three of these drivers came from a Dell home theatre system that my parents owned before giving it to me. Those drivers were manufactured in 1998, the year my parents got married. The cabinet used a ported design, some 12 inches long and 2 inches wide. Unfortunately thanks to the relatively small diameter, it would produce chuffing and whistling when playing deep bass or at high volume.  We'll refer to this design as Hardware 1. 

The hardware didn't stay like that for long, though, since a TangBand W5-1138SMF was on the way. About a month after Skybreaker was initially constructed, I swapped in the W5. This took the woofer from 20W to 40W, and from 4mm to 9mm of Xmax. Not to mention that the W5 was a bigger driver at 5 inches versus the previous 4.25. We'll refer to this as Hardware 2.

Almost 5 months went by, before I decided to remove the port. It had dislodged itself from its mounting position, so I could either re-attach it or take this opportunity to replace it with something better. I installed a passive radiator in place of the port, which unintentionally dropped tuning from 32hz to 25hz.  We'll refer to this as hardware 3. 

Then, I built Ragnarock. It was a new cabinet, with bracing and damping (see [the specsheet](/Ragnarock/specsheet.html/#enclosure) for more details). This needed to be done, since the 18L cabinet was woefully large for the PR, as evidenced by the 25hz tuning with no added mass on it. The smaller cabinet raised the tuning frequency to around 29hz, but this was still too low. Adding to the bad news, I discovered that the W5 woofer had sustained mild damage due to being run at full power without adequate damping from the cabinet. This cabinet of 19L was so big that there was almost no resistance to the woofer, causing said damage. We'll refer to Ragnarock in this state as Hardware 4.

Eventually, I discovered that the new cabinet (9.5L) was still not small enough to stop the damage. Eventually I had to cease use of Ragnarock until this issue could be resolved. I solved the problem by repurposing the W5 in an entirely separate bass module project with a 4.5L cabinet and Ragnarock's passive radiator.

Finally, I purchased a new driver, the W6-1139SIF, and an accompanying passive radiator that was larger and had a higher tuning frequency. The W6 has a staggering 12mm of Xmax, can take up to 50 watts, and produces more sound per watt than either of the previous drivers. We refer to this as Hardware 5!


## Software
Architect in its youth was barely more than a fancy EQ. At this point (version 1.0), it had 6 EQ bands, with fixed gain on each band. Paired with these bands was a single limiter on the output, which I could use to prevent clipping. Architect 1.0 was paired with Hardware 1. The small driver had massive distortion problems, and Architect wasn't advanced enough to stop it from hitting its excursion limit.

Architect 2 came with massive improvements. We finally got a 31-band EQ for linearity tuning, and a 3-band compressor for protecting the drivers, though this protection was not telemetry based. It simply watched the output level and made sure it didn't exceed a value that caused overexcursion.

Architect 3 came at the same time as Hardware 2, bringing psychoacousting tuning, proper dual-channel EQ with 32 bands of EQ per channel, and an 8-band compressor which increased transparency of the compression system. Prior to this, compression was audible with a bass cut also cutting male vocals, etc. Architect 3 was used with Hardware 3 as well. 

Architect 4 came with Hardware 4, and was the single biggest update to Architect in the history of this speaker. It implemented collection and usage of the real-time telemetry data, removed EQ entirely by replacing each EQ band with a compressor band, added the full 38 bands of protection compression, and added the adaptive crossover technology. Architect 4 moved from 8 compression bands to 102, thanks to a total rewrite. Instead of working within linux to do the DSP tasks, Architect 4 introduced the bare-metal and ValOS architecture. See [the DSP page](/Ragnarock/architect.html) to learn more!

Architect 5 built on Architect 4, adding bass remapping, WebUI, Remote Access, real-time tuning profile swaps, AirPlay support (was bluetooth or 3.5mm only prior to this update), and enabled data logging for detailed analysis post-listening. Architect 5 is the current release as of March 2026, and will be used with Hardware 5. 
