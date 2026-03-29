## Design
Ragnarock wasn't built in a day. This document will outline everything about the speaker, its components, the design decisions, and even a timeline since there were many revisions over the years.

## Hardware Choices
I chose the W6-1139SIF Subwoofer driver since I needed something that was durable, had lots of travel (Xmax), good sensitivity, but without consuming too much power. The W6 has 12mm of Xmax, is very sensitive (produces 84dB with only one watt of power), and needs only 50 watts to reach its full potential. Comparable subwoofers require up to 400W just to match its performance.

I chose the two tweeters for their sensitivity. They can produce 96dB with one watt of power, allowing them to reach extreme volumes with low power and therefore low heat. They also have paper cones, which reduces the amount of correction that needs to be done to the audio signal (see [here](/architect.md/#linear-response-tuning) to learn more about this process.

I chose the amplifier because it has a fixed-gain, high efficiency architecture. It allows for predictable output from the drivers given a specific output from the DSP. Without this, the DSP would have no idea what sound the speaker is actually producing!

For the cabinet, I chose to use Aspen wood, bonded with mitre-cut joints. Aspen has desirable acoustic properties, primarily balancing physical strength with workability and acoustic neutrality. In layman's terms, it was the wood that is the strongest without messing with the speaker's sound quality.

## Cabinet Design
A speaker is a complex device. To produce good sound, it must have the perfect synergy between the cabinet and the drivers. The cabinet looks the way it is because I wanted to achieve a specific internal volume (9.5 litres in this case) with the smallest possible footprint. 9.5L is a key measurement, because it's exactly the correct volume such that the pressure generated inside the box by the subwoofer provides the resistance it needs to produce the best sound. With a volume that's too big, there wouldn't be enough resistance to the subwoofer, and it would slowly destroy itself. Too small of a volume is also bad, since it limits efficient the subwoofer is at turning electricity into sound. 

Ragnarock's Cabinet is also cross-braced, which limits rattling in the cabinet, and damped with fiberglass, reducing bloom and echoes. 
The cabinet also employs a passive radiator, which is used to offload work from the subwoofer while simultaneously lowering the resonance frequency of the cabinet. The resonance frequency of the cabinet is the frequency at which the efficiency of the entire system is highest. In other words, it's the frequency at which the cabient itself produces the most sound. 

The passive radiator tunes the cabinet to 32hz (versus ~61hz if it was simply sealed). This allows the subwoofer to extend deeper for a better sound. Since the passive radiator also contains a cone and suspension, it is able to offload work from the subwoofer. Rather than having the subwoofer itself produce all the bass, the passive radiator becomes pneumatically coupled to the subwoofer in the bass range. This coupling gets stronger the lower the frequency goes, to the point where at tuning (32hz), the subwoofer itself barely moves, while the passive radiator is producing all the bass. Above the tuning frequency, they can work together, producing more bass total than either one could produce on their own.

