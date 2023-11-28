```java Pitch.java
package music;

/**
 * Pitch is an immutable type representing the frequency of a musical note.
 * Standard music notation represents pitches by letters: A, B, C, ..., G.
 * Pitches can be sharp or flat, or whole octaves up or down from these
 * primitive generators.
 * 
 * <p> For example:
 * <br> new Pitch('C') makes middle C
 * <br> new Pitch('C').transpose(1) makes C-sharp
 * <br> new Pitch('E').transpose(-1) makes E-flat
 * <br> new Pitch('C').transpose(OCTAVE) makes high C
 * <br> new Pitch('C').transpose(-OCTAVE) makes low C
 */
public class Pitch {

    private final int value;

    /*
     * Rep invariant: true.
     *
     * Abstraction function AF(value):
     *   AF(0),...,AF(11) map to middle C, C-sharp, D, ..., A, A-sharp, B.
     *   AF(i+12n) maps to n octaves above middle AF(i)
     *   AF(i-12n) maps to n octaves below middle AF(i)
     *
     * Safety from rep exposure:
     *   all fields are private and immutable.
     */

    private static final int[] SCALE = {
            9,  // A
            11, // B
            0,  // C
            2,  // D
            4,  // E
            5,  // F
            7,  // G
    };

    private static final String[] VALUE_TO_STRING = {
            "C", "^C", "D", "^D", "E", "F", "^F", "G", "^G", "A", "^A", "B"
    };
    
    /**
     * Middle C.
     */
    public static final Pitch MIDDLE_C = new Pitch('C');

    /**
     * Number of pitches in an octave.
     */
    public static final int OCTAVE = 12;

    private Pitch(int value) {
        this.value = value;
    }

    /**
     * Make a Pitch named c in the middle octave of the piano keyboard.
     * For example, new Pitch('C') constructs middle C.
     * @param c letter in {'A',...,'G'}
     */
    public Pitch(char c) {
        try {
            value = SCALE[c-'A'];
        } catch (ArrayIndexOutOfBoundsException e) {
            throw new IllegalArgumentException(c + " must be in the range A-G", e);
        }
    }

    /**
     * @param semitonesUp 
     * @return pitch made by transposing this pitch by semitonesUp semitones;
     *         for example, middle C transposed by 12 semitones is high C, and
     *         E transposed by -1 semitones is E flat
     */
    public Pitch transpose(int semitonesUp) {
        return new Pitch(value + semitonesUp);
    }

    /**
     * @param that 
     * @return number of semitones between this and that; i.e., n such that
     *         that.transpose(n).equals(this)
     */
    public int difference(Pitch that) {
        return this.value - that.value;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (obj.getClass() != this.getClass()) return false;
        
        Pitch that = (Pitch) obj;
        return this.value == that.value;
    }

    @Override
    public int hashCode() {
        return value;
    }

    /**
     * @return this pitch in abc music notation
     * @see "http://abcnotation.com/examples/"
     */
    @Override
    public String toString() {
        String suffix = "";
        int v = value;

        while (v < 0) {
            suffix += ",";
            v += OCTAVE;
        }

        while (v >= OCTAVE) {
            suffix += "'";
            v -= OCTAVE;
        }

        return VALUE_TO_STRING[v] + suffix;
    }
}
Instrument.java
package music;

/**
 * Instrument represents a musical instrument.
 * 
 * These instruments are the 128 standard General MIDI Level 1 instruments.
 * See: http://www.midi.org/about-midi/gm/gm1sound.shtml
 */
public enum Instrument {
    // Order is important in this enumeration because an instrument's
    // position must correspond to its MIDI program number.

    PIANO,
    BRIGHT_PIANO,
    ELECTRIC_GRAND,
    HONKY_TONK_PIANO,
    ELECTRIC_PIANO_1,
    ELECTRIC_PIANO_2,
    HARPSICHORD,
    CLAVINET,

    CELESTA,
    GLOCKENSPIEL,
    MUSIC_BOX,
    VIBRAPHONE,
    MARIMBA,
    XYLOPHONE,
    TUBULAR_BELL,
    DULCIMER,

    HAMMOND_ORGAN,
    PERC_ORGAN,
    ROCK_ORGAN,
    CHURCH_ORGAN,
    REED_ORGAN,
    ACCORDION,
    HARMONICA,
    TANGO_ACCORDION,

    NYLON_STR_GUITAR,
    STEEL_STRING_GUITAR,
    JAZZ_ELECTRIC_GTR,
    CLEAN_GUITAR,
    MUTED_GUITAR,
    OVERDRIVE_GUITAR,
    DISTORTION_GUITAR,
    GUITAR_HARMONICS,

    ACOUSTIC_BASS,
    FINGERED_BASS,
    PICKED_BASS,
    FRETLESS_BASS,
    SLAP_BASS_1,
    SLAP_BASS_2,
    SYN_BASS_1,
    SYN_BASS_2,

    VIOLIN,
    VIOLA,
    CELLO,
    CONTRABASS,
    TREMOLO_STRINGS,
    PIZZICATO_STRINGS,
    ORCHESTRAL_HARP,
    TIMPANI,

    ENSEMBLE_STRINGS,
    SLOW_STRINGS,
    SYNTH_STRINGS_1,
    SYNTH_STRINGS_2,
    CHOIR_AAHS,
    VOICE_OOHS,
    SYN_CHOIR,
    ORCHESTRA_HIT,

    TRUMPET,
    TROMBONE,
    TUBA,
    MUTED_TRUMPET,
    FRENCH_HORN,
    BRASS_ENSEMBLE,
    SYN_BRASS_1,
    SYN_BRASS_2,

    SOPRANO_SAX,
    ALTO_SAX,
    TENOR_SAX,
    BARITONE_SAX,
    OBOE,
    ENGLISH_HORN,
    BASSOON,
    CLARINET,

    PICCOLO,
    FLUTE,
    RECORDER,
    PAN_FLUTE,
    BOTTLE_BLOW,
    SHAKUHACHI,
    WHISTLE,
    OCARINA,

    SYN_SQUARE_WAVE,
    SYN_SAW_WAVE,
    SYN_CALLIOPE,
    SYN_CHIFF,
    SYN_CHARANG,
    SYN_VOICE,
    SYN_FIFTHS_SAW,
    SYN_BRASS_AND_LEAD,

    FANTASIA,
    WARM_PAD,
    POLYSYNTH,
    SPACE_VOX,
    BOWED_GLASS,
    METAL_PAD,
    HALO_PAD,
    SWEEP_PAD,

    ICE_RAIN,
    SOUNDTRACK,
    CRYSTAL,
    ATMOSPHERE,
    BRIGHTNESS,
    GOBLINS,
    ECHO_DROPS,
    SCI_FI,

    SITAR,
    BANJO,
    SHAMISEN,
    KOTO,
    KALIMBA,
    BAG_PIPE,
    FIDDLE,
    SHANAI,

    TINKLE_BELL,
    AGOGO,
    STEEL_DRUMS,
    WOODBLOCK,
    TAIKO_DRUM,
    MELODIC_TOM,
    SYN_DRUM,
    REVERSE_CYMBAL,

    GUITAR_FRET_NOISE,
    BREATH_NOISE,
    SEASHORE,
    BIRD,
    TELEPHONE,
    HELICOPTER,
    APPLAUSE,
    GUNSHOT,
}
Music.java
package music;

/**
 * Music represents a piece of music played by multiple instruments.
 */
public interface Music {

    /**
     * @return total duration of this piece in beats
     */
    double duration();

    /**
     * Play this piece.
     * @param player player to play on
     * @param atBeat when to play
     */
    void play(SequencePlayer player, double atBeat);

}
MusicLanguage.java
package music;

import static music.Pitch.OCTAVE;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * MusicLanguage defines static methods for constructing and manipulating Music expressions.
 */
public class MusicLanguage {

    /**
     * Prevent instantiation but allow subclassing.
     */
    protected MusicLanguage() {}

    ////////////////////////////////////////////////////
    // Factory methods
    ////////////////////////////////////////////////////

    /**
     * Make Music from a string using a variant of abc notation
     *    (see http://abcnotation.com/examples/).
     * 
     * <p> The notation consists of whitespace-delimited symbols representing
     * either notes or rests. The vertical bar | may be used as a delimiter
     * for measures; notes() treats it as a space.
     * Grammar:
     * <pre>
     *     notes ::= symbol*
     *     symbol ::= . duration          // for a rest
     *              | pitch duration      // for a note
     *     pitch ::= accidental letter octave*
     *     accidental ::= empty string    // for natural,
     *                  | _               // for flat,
     *                  | ^               // for sharp
     *     letter ::= [A-G]
     *     octave ::= '                   // to raise one octave
     *              | ,                   // to lower one octave
     *     duration ::= empty string      // for 1-beat duration
     *                | /n                // for 1/n-beat duration
     *                | n                 // for n-beat duration
     *                | n/m               // for n/m-beat duration
     * </pre>
     * <p> Examples (assuming 4/4 common time, i.e. 4 beats per measure):
     *     C     quarter note, middle C
     *     A'2   half note, high A
     *     _D/2  eighth note, middle D flat
     * 
     * @param notes string of notes and rests in simplified abc notation given above
     * @param instr instrument to play the notes with
     * @return the music in notes played by instr
     */
    public static Music notes(String notes, Instrument instr) {
        Music music = rest(0);
        for (String sym : notes.split("[\\s|]+")) {
            if (!sym.isEmpty()) {
                music = concat(music, parseSymbol(sym, instr));
            }
        }
        return music;
    }

    /* Parse a symbol into a Note or a Rest. */
    private static Music parseSymbol(String symbol, Instrument instr) {
        Matcher m = Pattern.compile("(?<pitch>[^/0-9]*)(?<numerator>[0-9]+)?(?<denominator>/[0-9]+)?").matcher(symbol);
        
        if (!m.matches()) throw new IllegalArgumentException("couldn't understand " + symbol);

        String pitchSymbol = m.group("pitch");

        double duration = 1.0;
        if (m.group("numerator") != null) duration *= Integer.valueOf(m.group("numerator"));
        if (m.group("denominator") != null) duration /= Integer.valueOf(m.group("denominator").substring(1));

        if (pitchSymbol.equals(".")) return rest(duration);
        else return note(duration, parsePitch(pitchSymbol), instr);
    }

    /* Parse a symbol into a Pitch. */
    private static Pitch parsePitch(String symbol) {
        if (symbol.endsWith("'"))
            return parsePitch(symbol.substring(0, symbol.length()-1)).transpose(OCTAVE);
        else if (symbol.endsWith(","))
            return parsePitch(symbol.substring(0, symbol.length()-1)).transpose(-OCTAVE);
        else if (symbol.startsWith("^"))
            return parsePitch(symbol.substring(1)).transpose(1);
        else if (symbol.startsWith("_"))
            return parsePitch(symbol.substring(1)).transpose(-1);
        else if (symbol.length() != 1)
            throw new IllegalArgumentException("can't understand " + symbol);
        else
            return new Pitch(symbol.charAt(0));
    }

    /**
     * @param duration duration in beats, must be >= 0
     * @param pitch pitch to play
     * @param instrument instrument to use
     * @return pitch played by instrument for duration beats
     */
    public static Music note(double duration, Pitch pitch, Instrument instrument) {
        return new Note(duration, pitch, instrument);
    }

    /**
     * @param duration duration in beats, must be >= 0
     * @return rest that lasts for duration beats
     */
    public static Music rest(double duration) {
        return new Rest(duration);
    }

    ////////////////////////////////////////////////////
    // Producers
    ////////////////////////////////////////////////////

    /**
     * @param m1 first piece of music
     * @param m2 second piece of music
     * @return m1 followed by m2
     */
    public static Music concat(Music m1, Music m2) {
        return new Concat(m1, m2);
    }
}
Note.java
package music;

/**
 * Note represents a note played by an instrument.
 */
public class Note implements Music {

    private final double duration;
    private final Pitch pitch;
    private final Instrument instrument;

    private void checkRep() {
        assert duration >= 0;
        assert pitch != null;
        assert instrument != null;
    }

    /**
     * Make a Note played by instrument for duration beats.
     * @param duration duration in beats, must be >= 0
     * @param pitch pitch to play
     * @param instrument instrument to use
     */
    public Note(double duration, Pitch pitch, Instrument instrument) {
        this.duration = duration;
        this.pitch = pitch;
        this.instrument = instrument;
        checkRep();
    }

    /**
     * @return pitch of this note
     */
    public Pitch pitch() {
        return pitch;
    }

    /**
     * @return instrument that should play this note
     */
    public Instrument instrument() {
        return instrument;
    }

    /**
     * @return duration of this note
     */
    @Override
    public double duration() {
        return duration;
    }

    /**
     * Play this note.
     */
    @Override
    public void play(SequencePlayer player, double atBeat) {
        player.addNote(instrument, pitch, atBeat, duration);
    }

    @Override
    public int hashCode() {
        long durationBits = Double.doubleToLongBits(duration);
        return (int) (durationBits ^ (durationBits >>> Integer.SIZE))
                + instrument.hashCode()
                + pitch.hashCode();
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (getClass() != obj.getClass()) return false;
        
        final Note other = (Note) obj;
        return duration == other.duration
                && instrument.equals(other.instrument)
                && pitch.equals(other.pitch);
    }

    @Override
    public String toString() {
        return pitch.toString() + duration;
    }
}
Rest.java
package music;

/**
 * Rest represents a pause in a piece of music.
 */
public class Rest implements Music {

    private final double duration;

    private void checkRep() {
        assert duration >= 0;
    }

    /**
     * Make a Rest that lasts for duration beats.
     * @param duration duration in beats, must be >= 0
     */
    public Rest(double duration) {
        this.duration = duration;
        checkRep();
    }

    /**
     * @return duration of this rest
     */
    @Override
    public double duration() {
        return duration;
    }

    /**
     * Play this rest.
     */
    @Override
    public void play(SequencePlayer player, double atBeat) {
        return;
    }

    @Override
    public int hashCode() {
        long durationBits = Double.doubleToLongBits(duration);
        return (int) (durationBits ^ (durationBits >>> Integer.SIZE));
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (getClass() != obj.getClass()) return false;
        
        final Rest other = (Rest) obj;
        return duration == other.duration;
    }

    @Override
    public String toString() {
        return "." + duration;
    }
}
Concat.java
package music;

/**
 * Concat represents two pieces of music played one after the other.
 */
public class Concat implements Music {

    private final Music first;
    private final Music second;

    private void checkRep() {
        assert first != null;
        assert second != null;
    }

    /**
     * Make a Music sequence that plays m1 followed by m2.
     * @param m1 music to play first
     * @param m2 music to play second
     */
    public Concat(Music m1, Music m2) {
        this.first = m1;
        this.second = m2;
        checkRep();
    }

    /**
     * @return first piece in this concatenation
     */
    public Music first() {
        return first;
    }

    /**
     * @return second piece in this concatenation
     */
    public Music second() {
        return second;
    }

    /**
     * @return duration of this concatenation
     */
    @Override
    public double duration() {
        return first.duration() + second.duration();
    }

    /**
     * Play this concatenation.
     */
    @Override
    public void play(SequencePlayer player, double atBeat) {
        first.play(player, atBeat);
        second.play(player, atBeat + first.duration());
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        return first.hashCode() + prime * second.hashCode();
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (getClass() != obj.getClass()) return false;
        
        final Concat other = (Concat) obj;
        return first.equals(other.first) && second.equals(other.second);
    }

    @Override
    public String toString() {
        return first + " " + second;
    }
}
ScaleSequence.java
package music.examples;

import javax.sound.midi.InvalidMidiDataException;
import javax.sound.midi.MidiUnavailableException;

import music.Instrument;
import music.Pitch;
import music.SequencePlayer;
import music.midi.MidiSequencePlayer;

public class ScaleSequence {
    
    /**
     * Play an octave up and back down starting from middle C.
     * 
     * @param args not used
     * @throws MidiUnavailableException if MIDI device unavailable
     * @throws InvalidMidiDataException if MIDI play fails
    */
    public static void main(String[] args) throws MidiUnavailableException, InvalidMidiDataException {

        Instrument piano = Instrument.PIANO;

        // create a new player
        final int beatsPerMinute = 120; // a beat is a quarter note, so this is 120 quarter notes per minute
        final int ticksPerBeat = 2;
        SequencePlayer player = new MidiSequencePlayer(beatsPerMinute, ticksPerBeat);

        // addNote(instr, pitch, startBeat, numBeats) schedules a note with pitch value 'pitch'
        // played by 'instr' starting at 'startBeat' to be played for 'numBeats' beats.
        
        int startBeat = 0;
        int numBeats = 1;
        player.addNote(piano, new Pitch('C'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('D'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('E'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('F'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('G'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('A'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('B'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('C').transpose(Pitch.OCTAVE), startBeat++, numBeats);
        player.addNote(piano, new Pitch('B'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('A'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('G'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('F'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('E'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('D'), startBeat++, numBeats);
        player.addNote(piano, new Pitch('C'), startBeat++, numBeats);

        System.out.println(player);

        // play!
        player.play();
    }
}
ScaleMusic.java
package music.examples;

import static music.Instrument.PIANO;
import static music.MusicLanguage.notes;

import javax.sound.midi.InvalidMidiDataException;
import javax.sound.midi.MidiUnavailableException;

import music.Music;
import music.midi.MusicPlayer;

public class ScaleMusic {

    /**
     * Play an octave up and back down starting from middle C.
     * 
     * @param args not used
     * @throws MidiUnavailableException if MIDI device unavailable
     * @throws InvalidMidiDataException if MIDI play fails
     */
    public static void main(String[] args) throws MidiUnavailableException, InvalidMidiDataException {
        
        // parse simplified abc into a Music
        Music scale = notes("C D E F G A B C' B A G F E D C", PIANO);
        
        System.out.println(scale);
        
        // play!
        MusicPlayer.play(scale);
    }
}
RowYourBoatInitial.java
package music.examples;

import music.*;
import music.midi.MusicPlayer;
import static music.Instrument.*;
import static music.MusicLanguage.*;

import javax.sound.midi.InvalidMidiDataException;
import javax.sound.midi.MidiUnavailableException;

public class RowYourBoatInitial {

    /**
     * Play a song.
     * 
     * @param args not used
     * @throws MidiUnavailableException if MIDI device unavailable
     * @throws InvalidMidiDataException if MIDI play fails
     */
    public static void main(String[] args) throws MidiUnavailableException, InvalidMidiDataException {
        Music rowYourBoat =
            notes("C C C3/4 D/4 E |"       // Row, row, row your boat,
                + "E3/4 D/4 E3/4 F/4 G2 |" // Gently down the stream.
                + "C'/3 C'/3 C'/3 G/3 G/3 G/3 E/3 E/3 E/3 C/3 C/3 C/3 |"
                                           // Merrily, merrily, merrily, merrily,
                + "G3/4 F/4 E3/4 D/4 C2",  // Life is but a dream.
                PIANO);
        
        System.out.println(rowYourBoat);
        
        MusicPlayer.play(rowYourBoat);
    }
}
SequencePlayer.java
package music;

/**
 * Schedules and plays a sequence of notes at given times.
 */
public interface SequencePlayer {

    /**
     * Schedule a note to be played starting at startBeat for the duration numBeats.
     * @param instr instrument for the note
     * @param pitch pitch value of the note
     * @param startBeat the starting beat (while playing, must be now or in the future)
     * @param numBeats the number of beats the note is played
     */
    public void addNote(Instrument instr, Pitch pitch, double startBeat, double numBeats);

    /**
     * Play the scheduled music.
     */
    public void play();

}
MidiSequencePlayer.java
package music.midi;

import java.math.BigInteger;
import java.text.MessageFormat;
import java.time.Duration;
import java.util.HashMap;
import java.util.Map;
import java.util.SortedMap;
import java.util.TreeMap;
import java.util.function.Consumer;

import javax.sound.midi.*;

import music.Instrument;
import music.Pitch;
import music.SequencePlayer;

/**
 * Schedules and plays a sequence of notes using the MIDI synthesizer.
 */
public class MidiSequencePlayer implements SequencePlayer {

    /**
     * Default tempo.
     */
    public static final int DEFAULT_BEATS_PER_MINUTE = 120;
    /**
     * Default MIDI ticks per beat.
     */
    public static final int DEFAULT_TICKS_PER_BEAT = 64;

    // the volume
    private static final int DEFAULT_VELOCITY = 100;

    // the generic marker meta message type
    private static final int META_MARKER = 6;
    // the "end_of_track" meta message type
    private static final int META_END_OF_TRACK = 47;

    // MIDI note number representing middle C
    private static final int MIDI_NOTE_MIDDLE_C = 60;
    
    private final Synthesizer synthesizer;

    // active MIDI channels, assigned to instruments
    private final Map<Instrument, Integer> channelForInstrument = new HashMap<>();

    // next available channel number (not assigned to an instrument yet)
    private int nextChannel = 0;

    private final Sequencer sequencer;
    private final Track track;
    private final int beatsPerMinute;
    private final int ticksPerBeat;

    // event callback functions
    private final SortedMap<Integer, Consumer<Double>> callbacks = new TreeMap<>();
    private final MetaMessage keepalive = new MetaMessage(META_MARKER, new byte[] { 0 }, 1);

    /*
     * Rep invariant:
     *   sequencer and track are non-null,
     *   beatsPerMinute and ticksPerBeat are positive,
     *   channels and callbacks are non-null,
     *   channels does not contain value nextChannel
     */

    private void checkRep() {
        assert sequencer != null : "sequencer should be non-null";
        assert track != null : "track should be non-null";
        assert beatsPerMinute >= 0 : "should be positive number of beats per minute";
        assert ticksPerBeat >= 0 : "should be positive number of ticks per beat";
        assert callbacks != null : "callbacks should be non-null";
        assert callbacks.keySet().stream().allMatch(n -> n >= 1) : "callback numbers should be positive";
        assert ! channelForInstrument.values().contains(nextChannel) : "nextChannel should not be assigned";
    }

    /**
     * Make a new MIDI sequence player with the default parameters.
     * @throws MidiUnavailableException if MIDI device unavailable
     * @throws InvalidMidiDataException if MIDI play fails
     */
    public MidiSequencePlayer() throws MidiUnavailableException, InvalidMidiDataException {
        this(DEFAULT_BEATS_PER_MINUTE, DEFAULT_TICKS_PER_BEAT);
    }

    /**
     * Make a new MIDI sequence player.
     * @param beatsPerMinute the number of beats per minute
     * @param ticksPerBeat the number of ticks per beat; every note plays for an integer number of ticks
     * @throws MidiUnavailableException if MIDI device unavailable
     * @throws InvalidMidiDataException if MIDI play fails
     */
    public MidiSequencePlayer(int beatsPerMinute, int ticksPerBeat)
            throws MidiUnavailableException, InvalidMidiDataException {
        synthesizer = MidiSystem.getSynthesizer();
        synthesizer.open();
        synthesizer.loadAllInstruments(synthesizer.getDefaultSoundbank());

        this.sequencer = MidiSystem.getSequencer();

        // create a sequence object with with tempo-based timing, where
        // the resolution of the time step is based on ticks per quarter note
        Sequence sequence = new Sequence(Sequence.PPQ, ticksPerBeat);
        this.beatsPerMinute = beatsPerMinute;
        this.ticksPerBeat = ticksPerBeat;

        // create an empty track; notes will be added to this track
        this.track = sequence.createTrack();

        sequencer.setSequence(sequence);

        checkRep();
    }

    /**
     * Schedule a note to be played with MIDI synthesizer.
     */
    @Override
    public void addNote(Instrument instr, Pitch pitch, double startBeat, double numBeats) {
        int channel = getChannel(instr);
        int note = getMidiNote(pitch);
        try {
            // schedule two events in the track, one for starting a note and
            // the other for ending the note.
            addMidiNoteEvent(ShortMessage.NOTE_ON, channel, note, (int) (startBeat * ticksPerBeat));
            addMidiNoteEvent(ShortMessage.NOTE_OFF, channel, note, (int) ((startBeat + numBeats) * ticksPerBeat));
        } catch (InvalidMidiDataException imde) {
            String msg = MessageFormat.format("Cannot add note with the pitch {0} at beat {1} " +
                                              "for duration {2}", note, startBeat, numBeats);
            throw new RuntimeException(msg, imde);
        }
    }

    /**
     * Schedule a MIDI note event.
     * @param eventType valid MidiMessage type in ShortMessage
     * @param channel valid channel
     * @param note valid pitch value
     * @param tick tick >= 0
     * @throws InvalidMidiDataException
     */
    private void addMidiNoteEvent(int eventType, int channel, int note, int tick) throws InvalidMidiDataException {
        ShortMessage msg = new ShortMessage(eventType, channel, note, DEFAULT_VELOCITY);
        this.track.add(new MidiEvent(msg, tick));
    }

    /**
     * Schedule a callback when the synthesizer reaches a time.
     * @param callback function to call
     * @param atBeat beat at which to call the callback (while playing, must be now or in the future)
     */
    public void addEvent(Consumer<Double> callback, double atBeat) {
        int callbackNumber = saveCallback(callback);
        try {
            addMidiMetaEvent(callbackNumber, (int) (atBeat * ticksPerBeat));
        } catch (InvalidMidiDataException imde) {
            throw new RuntimeException("Cannot add event at beat " + atBeat, imde);
        }
    }

    private int saveCallback(Consumer<Double> callback) {
        int key = callbacks.isEmpty() ? 1 : callbacks.lastKey() + 1;
        callbacks.put(key, callback);
        checkRep();
        return key;
    }

    /**
     * Schedule a MIDI meta event.
     * @param callback active callback number
     * @param tick tick >= 0
     * @throws InvalidMidiDataException
     */
    private void addMidiMetaEvent(int callback, int tick) throws InvalidMidiDataException {
        byte[] bytes = BigInteger.valueOf(callback).toByteArray();
        MetaMessage msg = new MetaMessage(META_MARKER, bytes, bytes.length);
        this.track.add(new MidiEvent(msg, tick));
        this.track.add(new MidiEvent(keepalive, tick+1));
    }

    /**
     * Open the MIDI sequencer and play the scheduled music.
     */
    @Override
    public void play() {
        try {
            sequencer.open();
        } catch (MidiUnavailableException mue) {
            throw new RuntimeException("Unable to open MIDI sequencer", mue);
        }
        sequencer.setTempoInBPM(this.beatsPerMinute);

        sequencer.addMetaEventListener(meta -> {
            try {
                if (meta.getType() == META_MARKER) {
                    // trigger event callback
                    int callbackNumber = new BigInteger(meta.getData()).intValue();
                    if (callbackNumber > 0) {
                        callbacks.remove(callbackNumber).accept(sequencer.getTickPosition() / (double)ticksPerBeat);
                    }
                } else if (meta.getType() == META_END_OF_TRACK) {
                    // allow the sequencer to finish
                    try { Thread.sleep(Duration.ofSeconds(1).toMillis()); } catch (InterruptedException ie) { }
                    // stop & close the sequencer
                    sequencer.stop();
                    sequencer.close();
                }
            } catch (Throwable t) {
                t.printStackTrace();
                throw t;
            }
        });

        // start playing!
        sequencer.start();
    }

    /**
     * Get a MIDI channel for the given instrument, allocating one if necessary.
     * @param instr instrument
     * @return channel for the instrument
     */
    private int getChannel(Instrument instr) {
        // check whether this instrument already has a channel
        if (channelForInstrument.containsKey(instr)) {
            return channelForInstrument.get(instr);
        }
        
        int channel = allocateChannel();
        patchInstrumentIntoChannel(channel, instr);
        channelForInstrument.put(instr, channel);
        checkRep();
        return channel;
    }

    /**
     * @return next available channel number
     */
    private int allocateChannel() {
        MidiChannel[] channels = synthesizer.getChannels();
        if (nextChannel >= channels.length) {
            throw new RuntimeException("Tried to use too many instruments: limited to " + channels.length);
        }
        return nextChannel++;
    }

    private void patchInstrumentIntoChannel(int channel, Instrument instr) {
        try {
            addMidiNoteEvent(ShortMessage.PROGRAM_CHANGE, channel, instr.ordinal(), 0);
        } catch (InvalidMidiDataException imde) {
            throw new RuntimeException("Cannot set instrument", imde);
        }
    }

    /**
     * @return the MIDI note number for a pitch, defined as the number of
     *         semitones above C 5 octaves below middle C; for example,
     *         middle C is note 60
     */
    private static int getMidiNote(Pitch pitch) {
        return MIDI_NOTE_MIDDLE_C + pitch.difference(Pitch.MIDDLE_C);
    }

    /**
     * @return a string that displays the entire track information as a
     *         sequence of MIDI events, where each event is either turning on
     *         or off a note at a certain tick, a marker event, or the end of
     *         the track
     */
    @Override
    public String toString() {
        String trackInfo = "";

        for (int i = 0; i < track.size(); i++) {
            final MidiEvent e = track.get(i);
            final MidiMessage msg = e.getMessage();
            final String msgString;

            if (msg instanceof ShortMessage) {
                final ShortMessage smg = (ShortMessage) msg;
                final int command = smg.getCommand();

                if (command == ShortMessage.NOTE_OFF) {
                    msgString = "NOTE_OFF Pitch: " + smg.getData1();
                } else if (command == ShortMessage.NOTE_ON) {
                    msgString = "NOTE_ON Pitch: " + smg.getData1();
                } else if (command == ShortMessage.PROGRAM_CHANGE) {
                    msgString = "PROGRAM_CHANGE " + Instrument.values()[smg.getData2()];
                } else {
                    msgString = "Unknown command " + command;
                }

            } else if (msg instanceof MetaMessage) {
                final MetaMessage mmg = (MetaMessage) msg;
                final int type = mmg.getType();
                final String typeName;

                if (type == META_MARKER) {
                    typeName = "MARKER";
                } else if (type == META_END_OF_TRACK) {
                    typeName = "END_OF_TRACK";
                } else {
                    typeName = "Unknown type " + type;
                }

                msgString = "Meta event: " + typeName;

            } else {
                msgString = "Unknown event";
            }

            trackInfo += msgString + " Tick: " + e.getTick() + "\n";
        }

        return trackInfo;
    }
}
MusicPlayer.java
package music.midi;

import javax.sound.midi.InvalidMidiDataException;
import javax.sound.midi.MidiUnavailableException;

import music.*;

/**
 * MusicPlayer can play a Music expression on the MIDI synthesizer.
 */
public class MusicPlayer {

    /**
     * Play music.
     * @param music music to play
     * @throws MidiUnavailableException if MIDI device unavailable
     * @throws InvalidMidiDataException if MIDI play fails
     */
    public static void play(Music music) throws MidiUnavailableException, InvalidMidiDataException {
        final SequencePlayer player = new MidiSequencePlayer();
        
        // load the player with a sequence created from music (add a small delay at the beginning)
        final double warmup = 0.125;
        music.play(player, warmup);
        
        // start playing
        player.play();
    }
}