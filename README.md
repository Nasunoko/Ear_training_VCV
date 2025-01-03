# Ear_training_VCV
An ear training patch for VCV Rack. Generates at random a chord (triad). Based on your parameters, the patch can generate diatonic chords, non diatonic chords, and altered chords.

Contains modules from :

VCV basic suite, Bogaudio, Mindmeld, Squinktronix, Count Modula, Aaron Static, CV Funk, ML Modules, Grande

User Guide :
The patch contains 7 main knobs, grouped in the MindMeld Patchmaster : 
- Gen : a trigger button that generates a random chord based on specified parameters.
- Reset : A forced reset of the whole patch. Use it as soon as something goes haywire, or for safety when you change any of the parameters.
- Tonality : Changes the tonality of the patch and the reference tonality for the chord generation. The current tonality is shown on the neighbouring Reftone module (but please change the tonality directly from the Tonality Patchmaster knob, to avoid problems in the patch)
- Alts p : The probability that the chord generation will switch to a diatonic chord with altered chords of the same scale degree (contains maj, min, sus2, sus4, dim, aug). When p = 0, the knob is off, and no altered chords are generated.
- Chrom p : The probability that the chord generation will switch to a neighboring chromatic degree of the generated scale degree. When p = 0, the knob is off, and no chords of a neighbouring chromatic degree are generated.
- Rdm invers : A latch button that activates random inversion of the generated chord (fundamental state, 1st inversion, 2 inversion) when turned on. 
- Rdm voicing : A latch button that activates random voicing of the generated chord (closed voicing, drop bass, drop highest note, open voicing) when turned on.

The patch usage is mainly thought around 4 modes or end results (with an approximate estimate of the numbers it could reach, though my estimations are probably exaggerated because of enharmony and other probable mistakes in calculation) :
- Mode 1 : Fully diatonic chords generated (Alts p = 0, Chrom p = 0 ; 7 chords, 3 inversion options, 4 voicing options = 84 chords)
- Mode 2 : Diatonic chords + Altered chords of the same degree (Alts p > 0, Chrom p = 0 ; 42 chords, 3 inversion options, 4 voicing options = 504 chords)
- Mode 3 : Diatonic chords + Major chords of a neighbouring chromatic degree (Alts p = 0, Chrom p > 0 ; 12 chords, 3 inversion options, 4 voicing options = 144 chords)
- Mode 4 : Diatonic chords + Chords of a neighbouring chromatic degree, each potentially altered (Alts p > 0, Chrom p > 0 ; 72 chords, 3 inversion options, 4 voicing options = 864 chords)

You can find the pitch cv output for the generated chord on the bottom left of the patch, near the MindMeld Patchmaster module : it will be the top output of the Port module. You can then use the cv output to arpegiate the chord or pitch it into any VCO of your choice. I like to use it by layering it over a drone (using Ouros by CV Funk) generated by dropping by one octave the fundamental of the tonality and putting the generated chord above it to hear the generated chord with the reference of the tonality.

NB : *When changing tonality, because of how the system works, you should always reset the whole patch with the reset button, for safety but because it is also very likely not doing so might lock the patch in the former tonality.*


System details :
Here I'd like to detail a bit more how the main system works, in case you notice any anomaly and need to check the rest of the module to identify the source of the problem. I've used as many TD 202 Submarine to label the structure ; you can also see the color coding of the CV cables in a VCV notes module on the left of the patch.

Row 1 : (Go from right to left) 
The Mindmeld Gen button is mapped to the Manual Gate "Rst, gen, invsn vcng" section. Its triggers are then sent to the "Random seq select" (via the VCV random module), which generates a random voltage address for two ADDR-Seq, the "Scale Step seq" and "Alt chord type seq". These two sequencers generate with the random CV adress CV voltages for the scale degrees steps (7 steps) and the chord alteration (6 steps). *If problems arise, please check in the right click menu that both seq check the "Wrap select at steps" option.* 
The resulting CV then goes to a series of Aaron Static modules, with the function of generating a chord based on the random step of the sequencers. For practical reason, I have chosen to use 4 parallel modules ; it might be possible to simplify this section and boil it down to 3 modules. Also, note that the Scale CV on the left is controlled by the Reftone Tonality (which is necessary for generating diatonic steps).

Row 2 :
The "Tonality offset" section is built to rectify an issue with the Addr-SEQ "Scale step seq", which would output CV from -10 to +10 volts, leading to wide gaps in the generated chords.
Then, with the three "p" sections ("Chrom steps p", "Alt p", "Alts+chrom p"), the probability knobs of the Patchmaster are mapped to a Manual CV, which then determines the ouput of a latched Chances module for each option. That way, I can generate mute gates signals which I use later in the patch to mute seq switch options and effectively build the 4 modes I need to use this patch. Still on the same logic, o the left, with the "Dtnc logic mute", I built a small system to generate a gate that mutes the diatonic chord switch when alt p or chrom p = 1 (thus locking the generated chord in full chromatic or full altered chords).
The rest of this row is dedicated to quantize the chromatics steps of the scale, using a system of double note toggle (a first trigger toggles the full chromatic scale, then a second trigger untoggles the diatonic scale of the current tonality, resulting in toggling the 5 chromatic steps of the current scale). This is the part that needs hard reset to work, as otherwise you're locked in the previous tonality when changing tonality.

Row 3 :
This last row is mostly dedicated to a switch + mute system that allows to output chords depending on the depended probability, effectively resulting in one of the desired modes (this is what I called the "Mode voltage address"). First, The 8:1 switch on the left receives the 4 chord CV options. It receives a voltage address from the PGMR's first row. The PGMR steps are toggled by an incoming trigger for each step that is received from a trigger signal being randomly output out of a 4 way sequential switch. these trigger signals are logically muted depending on the other paramaters, thus locking the randomized trigger signals on the desired chords. 
Finally, You have the "Rndm vcng invsn switch", which receives a random value and a gate signal from the Pulse module (mapped to the Patch master).


