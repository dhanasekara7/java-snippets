##### Java Lambdas
```java
// ********* 1) thread old style
public class RunnableDemo {
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(
                 "inside runnable using an anonymous inner class");
            }
        }).start();
 }
}
// new
new Thread(() -> System.out.println(
 "inside Thread constructor using lambda")).start();
// new
Runnable r = () -> System.out.println(
 "lambda expression implementing the run method");
new Thread(r).start();
```