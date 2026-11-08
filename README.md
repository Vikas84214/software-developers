import java.util.concurrent.Semaphore;

class Foo {

    final Semaphore hasFirstRun = new Semaphore(0);
    final Semaphore hasSecondRun = new Semaphore(0);

    public void first(Runnable printFirst) throws InterruptedException {
        printFirst.run();
        hasFirstRun.release(); // Allows second() to run, and wakes it if it's sleeping.
    }

    public void second(Runnable printSecond) throws InterruptedException {
        /*
            hasFirstRun is initialized to 0. This means that if second() is executed
            before first(), it can't get the lock and goes to sleep. In this case, it
            will be waked up by first().
            
            If second() is executed after first(), then it can acquire the semaphore immediately
            and execute its code.
        */
        hasFirstRun.acquire(); 
        printSecond.run();
        hasSecondRun.release();
    }

    public void third(Runnable printThird) throws InterruptedException {
        hasSecondRun.acquire(); // same logic as second()
        printThird.run();
    }
}
