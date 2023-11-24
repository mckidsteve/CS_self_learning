## EditBuffer.java

```java
package editor;

/**
 * An EditBuffer represents a threadsafe mutable string of characters in a text editor.
 */
public interface EditBuffer {

    /**
     * Modifies this by inserting a string.
     * @param position position to insert at (requires 0 <= position <= current buffer length)
     * @param insertion string to insert
     */
    public void insert(int position, String insertion);
    
    /**
     * Modifies this by deleting a substring
     * @param position start of substring to delete (requires 0 <= position <= current buffer length)
     * @param len length of substring to delete (requires 0 <= len <= current buffer length - position)
     */
    public void delete(int position, int len);
    
    /**
     * @return length of text sequence in this edit buffer
     */
    public int length();
    
    /**
     * @return content of this edit buffer
     */
    @Override public String toString();
}
```

## SimpleBuffer.java
```java
package editor;

/**
 * SimpleBuffer is a threadsafe EditBuffer with a simple rep.
 */
public class SimpleBuffer implements EditBuffer {

    private String text;
    // Rep invariant: 
    //   true
    // Abstraction function: 
    //   represents the sequence text[0],...,text[text.length()-1]
    // Safety from rep exposure:
    //   TODO
    // Concurrency argument:
    //   TODO

    private void checkRep() {
        assert text != null;
    }

    /** Make a SimpleBuffer. */
    public SimpleBuffer() {
        text = "";
        checkRep();
    }

    @Override
    public void insert(int position, String insertion) {
        text = text.substring(0, position) + insertion + text.substring(position);
        checkRep();
    }

    @Override
    public void delete(int position, int len) {
        text = text.substring(0, position) + text.substring(position+len);
        checkRep();
    }

    @Override
    public int length() {
        return text.length();
    }

    @Override
    public String toString() {
        return text;
    }
}
```

## GapBuffer.java
```java
package editor;

/**
 * GapBuffer is a non-threadsafe EditBuffer that is optimized for editing with a 
 * cursor, which tends to make a sequence of inserts and deletes at the same place
 * in the buffer.
 */
public class GapBuffer implements EditBuffer {
    private char[] a;
    private int gapStart;
    private int gapLength;
    // Rep invariant:
    //   0 <= gapStart <= a.length
    //   0 <= gapLength <= a.length - gapStart
    // Abstraction function: 
    //   represents the sequence a[0],...,a[gapStart-1],a[gapStart+gapLength],...,a[a.length-1]
    
    private void checkRep() {
        assert a != null;
        assert 0 <= gapStart;
        assert gapStart <= a.length;
        assert 0 <= gapLength;
        assert gapLength <= a.length - gapStart;
    }
    
    /** Make a GapBuffer. */
    public GapBuffer() {
        a = new char[1];
        gapStart = 0;
        gapLength = 1;
        checkRep();
    }
    
    @Override
    public void insert(int position, String insertion) {
        int len = insertion.length();
        moveGap(position, len);
        insertion.getChars(0, len, a, gapStart);
        gapStart += len;
        gapLength -= len;
        checkRep();
    }

    @Override
    public void delete(int position, int len) {
        moveGap(position, 0);
        gapLength += len;
        checkRep();
    }

    @Override
    public int length() {
        return a.length - gapLength;
    }

    @Override
    public String toString() {
        char[] result = new char[a.length-gapLength];
        // copy the part of a before the gap)
        System.arraycopy(a, 0, result, 0, gapStart);
        // copy the part after the gap
        System.arraycopy(a, gapStart+gapLength, result, gapStart, a.length-gapStart-gapLength);
        return String.valueOf(result);
    }
    
    /**
     * Move the gap to a particular position and make sure it's at least a given size.
     * @param newGapStart desired gap start (requires 0 <= newGapStart <= a.length)
     * @param minGapLength desired minimum gap length (requires 0 <= minGapLength).
     * Effects: after this method returns, gapStart == newGapStart and gapLength >= minGapLength,
     * but the text sequence represented by the buffer is unchanged.
     */
    private void moveGap(int newGapStart, int minGapLength) {
        String oldText = null; // used for debugging, only if asserts are turned on
        assert (oldText = toString()) != null;
        
        // determine the target array we'll be copying to, which might be the same
        // as a, or might be a fresh, bigger array
        char[] b; // target array
        int newGapLength; // desired gap length in target array
        if (gapLength >= minGapLength) {
            // gap is already big enough; just use the existing array
            b = a;
            newGapLength = gapLength;
        } else {
            // need to make gap bigger, so need a new array
            int textLength = a.length - gapLength;
            b = new char[Math.max(textLength*2, textLength + minGapLength)];
            newGapLength = b.length - textLength;
        }

        // Now copy text from a to b.
        // Use System.arraycopy to do the copying, first because it's fast (exploiting native block
        // copying where possible), and second because its contract handles the case where we're moving
        // blocks within the same array (i.e. a==b), which is tricky.
        
        // a looks like <prefix_a> <gap_a> <suffix_a>
        // b will look like <prefix_b> <gap_b> <suffix_b>
        // Let's make sure we have names for the endpoints of these ranges,
        // so we'll define gapEnd and newGapEnd:
        //    a: 0...gapStart......gapEnd......a.length
        //    b: 0...newGapStart...newGapEnd...b.length
        int gapEnd = gapStart + gapLength;
        int newGapEnd = newGapStart + newGapLength;
        
        // Two cases: either gap is shifting to the left or to the right.
        //  (if gap stays at the same position, either case will work.)
        if (newGapStart < gapStart) {
            // gap is shifting to the left, a's prefix is longer than b's prefix
            //    a looks like ******___**
            //    b looks like ***________*****
            // copy part of a's prefix, up to b's gap
            System.arraycopy(a, 0, b, 0, newGapStart);
            // copy the rest of a's prefix after b's gap
            System.arraycopy(a, newGapStart, b, newGapEnd, gapStart - newGapStart);
            // copy a's suffix right after it
            System.arraycopy(a, gapEnd, b, newGapEnd + (gapStart - newGapStart), a.length-gapEnd);
        } else {
            // gap is shifting to the right, so a's prefix is shorter than b's prefix
            //    a looks like ***___*****
            //    b looks like ******________**
            // copy all of a's prefix
            System.arraycopy(a, 0, b, 0, gapStart);
            // copy part of a's suffix right after it, up to b's gap
            System.arraycopy(a, gapEnd, b, gapStart, newGapStart - gapStart);
            // copy the rest of a's suffix after b's gap
            System.arraycopy(a, gapEnd + (newGapStart - gapStart), b, newGapEnd, b.length-newGapEnd);
        }
        
        // replace the rep with b
        a = b;
        gapStart = newGapStart;
        gapLength = newGapLength;
        
        // assert the postcondition here to catch bugs early
        assert gapStart == newGapStart;
        assert gapLength >= minGapLength;
        assert toString().equals(oldText); // this is the most important bit
    }
}
``` 

## EditBufferTest.java

```java
package editor;

import static org.junit.jupiter.api.Assertions.*;

import java.util.*;

import org.junit.jupiter.api.Test;

/**
 * This class contains test cases for the EditBuffer interface.
 * It can't be run as a JUnit test suite by itself; it needs to be subclassed to
 * override the factory method make().
 */
public abstract class EditBufferTest {
    
    /** @return an empty EditBuffer */
    public abstract EditBuffer make();

    // make sure assertions are turned on, so that checkRep() runs
    @Test public void testAssertionsEnabled() {
        assertThrows(AssertionError.class, () -> { assert false; });
    }
    
    // Testing strategy:
    //
    // Partition char sequences: empty, one-char, many-chars
    // Partition positions: start, middle, end
    // Partition deletion lengths: 0, 1, many
    // Partition deletion-ends: start of buffer, middle of buffer, end of buffer
    //
    // Test insert with each partition for buf.insert(pos, ins)
    // Test delete with each partition for buf.delete(pos, len)
    // Test toString and length by using them for assertions
    
    // the subdomains covered by this test are shown by inline comments below
    @Test
    public void testInsert() {
        EditBuffer buf = make();
        assertEquals("", buf.toString());
        assertEquals(0, buf.length());
        
        buf.insert(0, "a"); // empty.insert(start, one-char)
        assertEquals("a", buf.toString());
        assertEquals(1, buf.length());

        buf.insert(1, "bcd"); // one-char.insert(end, many-chars)
        assertEquals("abcd", buf.toString());
        assertEquals(4, buf.length());

        buf.insert(2, ""); // many-chars.insert(middle, empty)
        assertEquals("abcd", buf.toString());
        assertEquals(4, buf.length());
    }

    // the subdomains covered by this test are shown by inline comments below
    @Test
    public void testDelete() {
        EditBuffer buf = make();

        buf.insert(0, "abcdefghi");
        assertEquals("abcdefghi", buf.toString());
        assertEquals(9, buf.length());
        
        buf.delete(0, 0); // many.delete(start, 0, start of buffer)
        assertEquals("abcdefghi", buf.toString());
        assertEquals(9, buf.length());

        buf.delete(0, 1); // many.delete(start, 1, middle of buffer)
        assertEquals("bcdefghi", buf.toString());
        assertEquals(8, buf.length());
        
        buf.delete(7, 0); // many.delete(end, 0, end of buffer)
        assertEquals("bcdefghi", buf.toString());
        assertEquals(8, buf.length());

        buf.delete(2, 3); // many.delete(middle, many, middle of buffer)
        assertEquals("bcghi", buf.toString());
        assertEquals(5, buf.length());
    }
    
    
    
//    private static final int N = 1000;
//
//    @Test
//    public void testManyInserts() { 
//        EditBuffer buf = make();
//        doManyInserts(buf, "a", N);
//        doManyInserts(buf, "b", N);
//        assertEquals(N, count(buf.toString(), "a"));
//        assertEquals(N, count(buf.toString(), "b")); 
//    }
//
//    /* insert s n times at random locations in buf. */
//    private static void doManyInserts(EditBuffer buf, String s, int n) {
//        for (int i = 0; i < n; ++i) {
//            int pos = (int) (Math.random() * buf.length());
//            buf.insert(pos, s);
//        }
//    }
// 
//    /* @return number of times needle occurs in haystack (including overlapping matches). */
//    private static int count(String haystack, String needle) {
//        int n = 0;
//        for (int i = haystack.indexOf(needle); i != -1; i = haystack.indexOf(needle, i+1)) {
//            ++n;
//        }
//        return n;
//    }
    
    
    
//    @Test
//    public void testManyInsertsMultithreaded() { 
//        EditBuffer buf = make();
//        Set<Thread> threads = new HashSet<Thread>();
//        threads.add(startInserterThread(buf, "a", N));
//        threads.add(startInserterThread(buf, "b", N));
//        waitForAll(threads);
//        assertEquals(N, count(buf.toString(), "a"));
//        assertEquals(N, count(buf.toString(), "b")); 
//    }
// 
//    /* @return a started thread making n randomly-positioned inserts of s into buf. */
//    private static Thread startInserterThread(final EditBuffer buf, final String s, final int n) {
//        Thread thread = new Thread(new Runnable() {
//            public void run() {
//                Thread.yield();
//                doManyInserts(buf, s, n);
//            }
//        });
//        thread.start();
//        return thread; 
//    }
// 
//    /* wait for all threads in set to complete. */
//    private static void waitForAll(Set<Thread> threads) {
//        for (Thread thread : threads) {
//            try {
//                thread.join();
//            } catch (InterruptedException e) {
//                throw new AssertionError("don't interrupt my testing");
//            }
//        } 
//    }
    
    
    
//    @Test
//    public void testFindReplace() { 
//        EditBuffer buf = make();
//        doManyInserts(buf, "a", N);
//        doManyInserts(buf, "b", N);
//        assertEquals(N, count(buf.toString(), "a"));
//        assertEquals(N, count(buf.toString(), "b")); 
//
//        while (Editor.findReplace(buf, "a", "c") || Editor.findReplace(buf, "b", "d")) { 
//        }
//        assertEquals(0, count(buf.toString(), "a"));
//        assertEquals(0, count(buf.toString(), "b")); 
//        assertEquals(N, count(buf.toString(), "c"));
//        assertEquals(N, count(buf.toString(), "d")); 
//    }
    
    
    
//    @Test
//    public void testFindReplaceMultithreaded() {
//        EditBuffer buf = make();
// 
//        doManyInserts(buf, "a", N);
//        doManyInserts(buf, "b", N);
//        assertEquals(N, count(buf.toString(), "a"));
//        assertEquals(N, count(buf.toString(), "b")); 
//
//        Set<Thread> threads = new HashSet<Thread>();
//        threads.add(startReplacerThread(buf, "a", "c"));
//        threads.add(startReplacerThread(buf, "b", "d"));
//        waitForAll(threads);
//        assertEquals(0, count(buf.toString(), "a"));
//        assertEquals(0, count(buf.toString(), "b"));
//        assertEquals(N, count(buf.toString(), "c"));
//        assertEquals(N, count(buf.toString(), "d"));
//    }
// 
//    /* @return a started thread that replaces all occurrences of s with t in buf. */
//    private static Thread startReplacerThread(final EditBuffer buf, final String s, final String t) {
//        Thread thread = new Thread(new Runnable() {
//            public void run() {
//                Thread.yield();
//                while (Editor.findReplace(buf, s, t)) {
//                }
//            }
//        });
//        thread.start();
//        return thread; 
//    }
}
``` 

## SimpleBufferTest.java

```java
package editor;

public class SimpleBufferTest extends EditBufferTest {
    
    @Override public EditBuffer make() {
        return new SimpleBuffer();
    }
}
```

## GapBufferTest.java

```java
package editor;

public class GapBufferTest extends EditBufferTest {    
    
    @Override public EditBuffer make() {
        return new GapBuffer();
    }
}
```