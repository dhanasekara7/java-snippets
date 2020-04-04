##### Java 8
### Lambda expressions
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

// ********* 2) accept
File directory = new File("./src/main/java");
String[] names = directory.list(new FilenameFilter() {
 @Override
 public boolean accept(File dir, String name) {
    return name.endsWith(".java");
 }
});
System.out.println(Arrays.asList(names));

//new
File directory = new File("./src/main/java");
String[] names = directory.list((dir, name) -> name.endsWith(".java"));
    System.out.println(Arrays.asList(names));
}
//with explicit data types
File directory = new File("./src/main/java");
String[] names = directory.list((File dir, String name) ->
 name.endsWith(".java"));
System.out.println(Arrays.asList(names));
```
### Method references
```java
// *************** Using a lambda expression
Stream.of(3, 1, 4, 1, 5, 9)
 .forEach(x -> System.out.println(x));

// Using a method reference
Stream.of(3, 1, 4, 1, 5, 9)
 .forEach(System.out::println);

// Assigning the method reference to a functional interface
//java.util.function.Consumer
Consumer<Integer> printer = System.out::println;
Stream.of(3, 1, 4, 1, 5, 9)
 .forEach(printer);

//***************** Using a method reference to a static method
Stream.generate(Math::random) // static method ( Supplier as argument for "generate" method)
 .limit(10)
 .forEach(System.out::println); // instance method for Consumer

/// object::instanceMethod
  System.out::println

/// Class::staticMethod
  Math::max

/// Class::instanceMethod
  String::length

// equivalent to System.out::println
x -> System.out.println(x)

// equivalent to Math::max
(x,y) -> Math.max(x,y)

// equivalent to String::length
x -> x.length()

// *************** Invoking a multiple-argument instance method from a class reference
List<String> strings =
 Arrays.asList("this", "is", "a", "list", "of", "strings");
List<String> sorted = strings.stream()
 .sorted((s1, s2) -> s1.compareTo(s2))
 .collect(Collectors.toList());

```