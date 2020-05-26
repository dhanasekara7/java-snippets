### Java Generics Tutorial
```java
// Simple Box Class
public class Box {
    private Object object;
    public Object get() {
        return object;
    }
    public void set(Object object) {
        this.object = object;
    }
}

// generic version of Box class
public class Box<T> {
    private T t;
    public T get() {
        return t;
    }
    public void set(T t) {
        this.t = t;
    }
    // invoke Box with specified type
    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<>();
        Box<String> stringBox = new Box<>();
        Box rawBox = stringBox;
        rawBox.set(8);  // warning: unchecked invocation to set(T)
        Object ob = rawBox.get();

        // 1) to see unchecked warnings
        /*
        % javac -Xlint:unchecked Box.java -d ../out/production/JavaLearnWS/
        Box.java:15: 警告: [unchecked] raw型Boxのメンバーとしてのset(T)への無検査呼出しです
                rawBox.set(8);  // warning: unchecked invocation to set(T)
                          ^
          Tが型変数の場合:
            クラス Boxで宣言されているTはObjectを拡張します
        警告1個
        */
        // 2) @SuppressWarnings("unchecked") --to suppress
    }
}

// type parameter naming convetions
/**
E - Element (used extensively by the Java Collections Framework)
K - Key
N - Number
T - Type
V - Value
S,U,V etc. - 2nd, 3rd, 4th types
*/

// Mutiple type parameter and parameterized types

public class OrderedPair<K,V> implements Pair<K,V> {
    private K key;
    private V value;

    public OrderedPair(K key, V value) {
        this.key = key;
        this.value = value
    }

    @Override
    public K getKey() {
        return key;
    }

    @Override
    public V getValue() {
        return value;
    }

    public static void main(String[] args) {
        //calling Mutt parameter types
        final OrderedPair<Integer, String> op1 = new OrderedPair<>(123, "fun");

        // "parameterized type"
        // substitute type parameter with "parameterized type"
        final OrderedPair<Box<String>, String> op2 = new OrderedPair<>(new Box<>(), "op2");
    }
}

// bounded types
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
        // The isEven method invokes the intValue method defined in the Integer class through n.
    }

    // ...
}

//Multiple Bounds
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }

//Generic Methods and Bounded Type Parameters
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        // because the greater than operator (>) applies only to primitive types such as
        // short, int, double, long, float, byte, and char.
        // You cannot use the > operator to compare objects.
        // To fix the problem, use a type parameter bounded by the Comparable<T> interface:
        if (e > elem)  // compiler error
            ++count;
    return count;
}
//Generic Methods and Bounded Type Parameters
public static <T extends Comparable> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        // because the greater than operator (>) applies only to primitive types such as
        // short, int, double, long, float, byte, and char.
        // You cannot use the > operator to compare objects.
        // To fix the problem, use a type parameter bounded by the Comparable<T> interface:
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}


//Generics, Inheritance, and Subtypes
Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK

public void someMethod(Number n) { /* ... */ }
someMethod(new Integer(10));   // OK
someMethod(new Double(10.1));   // OK

Box<Number> box = new Box<Number>();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK

public void boxTest(Box<Number> n) { /* ... */ }
Box<Integer> box = new Box<Integer>();
boxTest(box); // fails because Box<Integer> and Box<Double> are not subtypes of Box<Number>

// Using the Collections classes as an example,
// ArrayList<E> implements List<E>, and List<E> extends Collection<E>.
// So ArrayList<String> is a subtype of List<String>, which is a subtype of Collection<String>.
// So long as you do not vary the type argument, the subtyping relationship is preserved between the types.

// type inference
public static <U> void addBox(U u,
    java.util.List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
}

//************
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
//alternatively Java compiler automatically infers type
BoxDemo.addBox(Integer.valueOf(10), listOfIntegerBoxes);

// ******** taget types
void processStringList(List<String> stringList) {
    // process stringList
}

processStringList(Collections.emptyList()); //will not compiler in java 7, but 8 ok
// because Collections.emptyList() --> List<Object>

processStringList(Collections.<String>emptyList()); //will work

//****************** wild card
// Upper Bounded wild cards
import java.util.Arrays;
import java.util.List;

public class JavaSum {

    public static double sumOfList(List<? extends Number> list) {
        double s = 0.0;
        for (Number nu : list) {
            s += nu.doubleValue();
        }
        return s;
    }

    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1,2,3,4,5);
        System.out.println(sumOfList(list1));

        //List<Double> list2 = Arrays.asList(1.1,2,3,4,5); // not ok
        List<? extends Number> list2 = Arrays.asList(1.1,2,3,4,5);
        System.out.println(sumOfList(list2));

    }
}

// ****************************  some more call
import java.util.Arrays;
import java.util.List;

public class UnboundedCall {
    public static void printList(List<Object> list) {
        for (Object elem : list)
            System.out.println(elem + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        List<Object> list = Arrays.asList(1,2,3,4,5);
        //List<Integer> list = Arrays.asList(1,2,3,4,5); // unbounded call can not be applied
        printList(list);
    }
}


// this is ok
public class UnboundedCall {
    public static void printList(List<?> list) {
        for (Object elem: list)
            System.out.print(elem + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1,2,3,4,5);
        //List<Object> list = Arrays.asList(1,2,3,4,5); // also ok
        printList(list);
    }
}

// lower bounded wildcard
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}

```

