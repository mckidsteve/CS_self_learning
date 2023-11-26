# Counter.java
```java
package counter;

import java.math.BigInteger;
import java.util.HashSet;
import java.util.Set;

/** A mutable incrementing counter that calls its listeners each time it reaches a new number. */
public class Counter {

    /** A listener for this counter. */
    public interface NumberListener {
        /** Called when the counter increments.
          * @param number the new number */
        public void numberReached(BigInteger number); 
    }

    /**
     * Example client code, trying out a Counter with a couple listeners.
     * @param args unused
     */
    public static void main(String[] args) {
        Counter counter = new Counter();

        // listen for prime numbers
        counter.addNumberListener(number -> {
            if (number.isProbablePrime(100)) {
                System.out.println(number + " prime");
            }
        });

        // listen for just one number, then stop listening
        counter.addNumberListener(new NumberListener() {
            public void numberReached(BigInteger number) {
                System.out.println(number);
                counter.removeNumberListener(this);
            }
        });

        // crank the counter in a background thread
        new Thread(() -> {
            while (true) {
                counter.increment();
            }
        }).start();
    }    
    
    // Rep
    private BigInteger number = BigInteger.ZERO;
    private Set<NumberListener> listeners = new HashSet<>();
    
    // Abstraction function
    //    AF(number, listeners) = a counter currently at 'number'
    //        that sends events to the 'listeners' whenever it changes
    // Rep invariant
    //    true
    // Thread safety argument
    //    uses the monitor pattern -- the rep is guarded by this object's lock,
    //    acquired on entering every public method
    
    /** Make a counter initially set to zero. */
    public Counter() {
    }
    
    /** @return the value of this counter. */
    public synchronized BigInteger number() {
        return number;
    }
    
    /** Increment this counter. */
    public synchronized void increment() {
        number = number.add(BigInteger.ONE);
        callListeners();
    }

    /** Modifies this counter by adding a listener.
      * @param listener called by this counter when it changes. */
    public synchronized void addNumberListener(NumberListener listener) {
        listeners.add(listener);
    }

    /** Modifies this counter by removing a listener.
      * @param listener will no longer be called by this counter. */
    public synchronized void removeNumberListener(NumberListener listener) {
        listeners.remove(listener);
    }

    // Call all listeners to notify them about a new number
    private void callListeners() {
        // iterate over a copy of listeners so that we are re-entrant
        // (i.e. a listener may call add/removeNumberListener,
        // which would mutate the set in the midst of iteration)
        for (NumberListener listener : Set.copyOf(listeners)) {
            listener.numberReached(number);
        }
    }
    
}
```