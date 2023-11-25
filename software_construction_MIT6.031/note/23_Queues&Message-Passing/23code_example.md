## DrinksFridge.java
```java
package fridge;

import java.util.concurrent.BlockingQueue;

/**
 * A mutable type representing a refrigerator containing drinks, using
 * message passing to communicate with clients.
 */
public class DrinksFridge {
    
    private int drinksInFridge;
    private final BlockingQueue<Integer> in;
    private final BlockingQueue<FridgeResult> out;
    
    // Abstraction function:
    //   AF(drinksInFridge, in, out) = a refrigerator containing `drinksInFridge` drinks
    //                                 that takes requests from `in` and sends replies to `out`
    // Rep invariant:
    //   drinksInFridge >= 0

    private void checkRep() {
        assert drinksInFridge >= 0;
        assert in != null;
        assert out != null;
    }
    
    /**
     * Make a DrinksFridge that will listen for requests and generate replies.
     * 
     * @param requests queue to receive requests from
     * @param replies queue to send replies to
     */
    public DrinksFridge(BlockingQueue<Integer> requests, BlockingQueue<FridgeResult> replies) {
        this.drinksInFridge = 0;
        this.in = requests;
        this.out = replies;
        checkRep();
    }
    
    /**
     * Start handling drink requests.
     */
    public void start() {
        new Thread(new Runnable() {
            public void run() {
                while (true) {
                    // TODO: we may want a way to stop the thread
                    try {
                        // block until a request arrives
                        int n = in.take();
                        FridgeResult result = handleDrinkRequest(n);
                        out.put(result);
                    } catch (InterruptedException ie) {
                        ie.printStackTrace();
                    }
                }
            }
        }).start();
    }
    
    /**
     * Take (or add) drinks from the fridge.
     * @param n if >= 0, removes up to n drinks from the fridge;
     *          if < 0, adds -n drinks to the fridge.
     * @return FridgeResult reporting how many drinks were actually added or removed
     *      and how many drinks are left in the fridge. 
     */
    private FridgeResult handleDrinkRequest(int n) {
        // adjust request to reflect actual drinks available
        int change = Math.min(n, drinksInFridge);
        drinksInFridge -= change;
        checkRep();
        return new FridgeResult(change, drinksInFridge);
    }
}
``` 

## FridgeResult.java

```
package fridge;

/**
 * A threadsafe immutable message describing the result of taking or adding drinks to a DrinksFridge.
 */
public class FridgeResult {
    private final int drinksTakenOrAdded;
    private final int drinksLeftInFridge;
    // Rep invariant: 
    //   TODO
    
    /**
     * Make a new result message.
     * @param drinksTakenOrAdded (precondition? TODO)
     * @param drinksLeftInFridge (precondition? TODO)
     */
    public FridgeResult(int drinksTakenOrAdded, int drinksLeftInFridge) {
        this.drinksTakenOrAdded = drinksTakenOrAdded;
        this.drinksLeftInFridge = drinksLeftInFridge;
    }
    
    // TODO: we will want more observers, but for now...
    
    @Override public String toString() {
        return (drinksTakenOrAdded >= 0 ? "you took " : "you put in ") 
                + Math.abs(drinksTakenOrAdded) + " drinks, fridge has " 
                + drinksLeftInFridge + " left";
    }
}
``` 

## LoadFridge.java

```java
package fridge;

import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

public class LoadFridge {
    
    /** Create and use a drinks fridge. */
    public static void main(String[] args) {
        
        BlockingQueue<Integer> requests = new LinkedBlockingQueue<>();
        BlockingQueue<FridgeResult> replies = new LinkedBlockingQueue<>();
        
        // start an empty fridge
        DrinksFridge fridge = new DrinksFridge(requests, replies);
        fridge.start();
        
        try {
            // deliver some drinks to the fridge
            requests.put(-42);
            
            // maybe do something concurrently...
            
            // read the reply
            System.out.println(replies.take());
        } catch (InterruptedException ie) {
            ie.printStackTrace();
        }
        System.out.println("done");
        System.exit(0); // ends the program, including DrinksFridge
    }
}
```

## ManyThirstyPeople.java
```java
package fridge;

import java.io.IOException;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class ManyThirstyPeople {
    private static final int QUEUE_SIZE = 100;
    private static final int N = 100;
    
    /**  Send N thirsty people to the DrinksFridge. */
    public static void main(String[] args) throws IOException {
        // make request and reply queues big enough to hold QUEUE_SIZE messages each
        BlockingQueue<Integer> requests = new ArrayBlockingQueue<>(QUEUE_SIZE);
        BlockingQueue<FridgeResult> replies = new ArrayBlockingQueue<>(QUEUE_SIZE);
        
        DrinksFridge fridge = new DrinksFridge(requests, replies);
        fridge.start();
        
        try {
            // put enough drinks in the fridge to start
            requests.put(-N);
            System.out.println(replies.take());
            
            // send the requests
            for (int x = 1; x <= N; ++x) {
                requests.put(1); // give me 1 drink!
                System.out.println("person #" + x + " is looking for a drink");
            }
            // collect the replies
            for (int x = 1; x <= N; ++x) {
                System.out.println("person #" + x + ": " + replies.take());
            }
        } catch (InterruptedException ie) {
            ie.printStackTrace();
        }
        System.out.println("done");
        System.exit(0); // ends the program, including DrinksFridge thread
    }
}
```