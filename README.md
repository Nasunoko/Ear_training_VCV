# Ear_training_VCV
An ear training patch for VCV Rack. Generates at random a chord (triad). Based on your parameters, the patch can generate diatonic chords, non diatonic chords, and altered chords.

Contains modules from :

VCV basic suite, Bogaudio, Mindmeld, Squinktronix, Count Modula, Aaron Static, CV Funk, ML Modules, Grande

User Guide :
The patch contains 7 main knobs, grouped in the MindMeld Patchmaster : 
- Gen : a trigger button that generates a random chord based on specified parameters.
- Reset : A forced reset of the whole patch. Use it as soon as something goes haywire, or for safety when you change any of the parameters.
- Tonality : Changes the tonality of the patch and the reference tonality for the chord generation. The current tonality is shown on the neighbouring Reftone module (but please change the tonality directly from the Tonality Patchmaster knob, to avoid problems in the patch)
- Alts p : The probability that the chord generation will switch to a diatonic chord with altered chords of the same scale degree (contains maj, sus2, sus4, dim, aug). When p = 0, the knob is off, and no altered chords are generated.
- Chrom p : The probability that the chord generation will switch to a neighboring chromatic degree of the generated scale degree. When p = 0, the knob is off, and no chords of a neighbouring chromatic degree are generated.
- Rdm invers : A latch button that activates random inversion of the generated chord (fundamental state, 1st inversion, 2 inversion) when turned on. 
- Rdm voicing : A latch button that activates random voicing of the generated chord (closed voicing, drop bass, drop highest note, open voicing) when turned on.

The patch usage is mainly thought around 4 modes or end results (with an approximate estimate of the numbers it could reach, though my estimations are probably exaggerated because of enharmony and other mistakes in calculation) :
Mode 1 : Fully diatonic chords generated (Alts p = 0, Chrom p = 0 ; 7 chords, 3 inversion options, 4 voicing options = 84 chords)
Mode 2 : Diatonic chords + Altered chords of the same degree (Alts p > 0, Chrom p = 0 ; 28 chords, 3 inversion options, 4 voicing options = 336 chords)
Mode 3 : Diatonic chords + Major or minor chords of a neighbouring chromatic degree (Alts p = 0, Chrom p > 0 ; 17 chords, 3 inversion options, 4 voicing options = 204 chords)
Mode 4 : Diatonic chords + Chords of a neighbouring chromatic degree, each potentially altered (Alts p > 0, Chrom p > 0 ; 28 + 25 = 53 chords, 3 inversion options, 4 voicing options = 636 chords)


System details :
