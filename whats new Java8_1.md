```java
// FileFilter -------------------------------------
    public class JavaFileFilter implements FileFilter {

        @Override
        public boolean accept(File file) {
            return file.getName().endsWith(".java");
        }

        public static void main(String[] args) {
            final JavaFileFilter javaFileFilter = new JavaFileFilter();
            File dir = new File("/user/dhans/java/");
            File[] files = dir.listFiles(javaFileFilter);
        }
    }

//  Using anonymous class-------------------------------------
    import java.io.File;
    import java.io.FileFilter;

    public class JavaFileFilter {

        public static void main(String[] args) {

            FileFilter javaFileFilter = new FileFilter() {
                @Override
                public boolean accept(File file) {
                    return file.getName().endsWith(".java");
                }
            };

            File dir = new File("/user/dhans/java/");
            File[] files = dir.listFiles(javaFileFilter);
            if (files != null) {
                for (File file : files) {
                    System.out.println(file);
                }
            }
        }
    }

//  Using lamda expression-------------------------------------
    import java.io.File;
    import java.io.FileFilter;

    public class JavaFileFilter {

        public static void main(String[] args) {

            FileFilter javaFileFilter = (File file) -> file.getName().endsWith(".java");

            File dir = new File("/user/dhans/java/");
            File[] files = dir.listFiles(javaFileFilter);
            if (files != null) {
                for (File file : files) {
                    System.out.println(file);
                }
            }
        }
    }

//  Using Runnable expression-------------------------------------

    public class RunnableLambda {

        public static void main(String[] args) {
            Runnable runnable = () -> {
                for (int i = 0; i < 5 ; i++) {
                    System.out.println(i);
                }
            };

            final Thread thread = new Thread(runnable);
            thread.start();
        }
    }

//  Using Comparator interface-------------------------------------

    import java.util.*;

    public class StringLengthComparator {

        Comparator<String> comparator = new Comparator<String>() {
            @Override
            public int compare(String str1, String str2) {
                return Integer.compare(str1.length(), str2.length());
            }
        };

        public static void main(String[] args) {
            StringLengthComparator slc = new StringLengthComparator();
            String str1 = "this is fun";
            String str2 = "yes";
            List<String> list = Arrays.asList(str1, str2);
            Collections.sort(list, slc.comparator);
            for(String str: list) {
                System.out.println(str);
            }

        }
    }

//  Using Comparator expression-------------------------------------
    import java.util.*;

    public class StringLengthComparator {

        Comparator<String> comparator = (String str1, String str2) -> {
            return Integer.compare(str1.length(), str2.length());
        };

        public static void main(String[] args) {
            StringLengthComparator slc = new StringLengthComparator();
            String str2 = "this is fun";
            String str1 = "yes";
            List<String> list = Arrays.asList(str1, str2);
            Collections.sort(list, slc.comparator);
            for(String str: list) {
                System.out.println(str);
            }
        }
    }

//  Functional interface annotation -------------------------------------
    @FunctionalInterface
    public interface MyFunctionalInterface {
        public void method();
    }

//  Package java.util.function  -----------------------------------------
    // Supplier
    @FunctionalInterface
    public interface Supplier<T> {
        T get();
    }

    // Consumer
    @FunctionalInterface
    public interface Consumer<T> {
        void accept(T t);
    }

    // BiConsumer
    @FunctionalInterface
    public interface BiConsumer<T, U> {
        void accept(T t, U u);
    }

    // Predicate
    @FunctionalInterface
    public interface Predicate<T> {
        boolean test(T t);
    }
    // BiPredicate
    @FunctionalInterface
    public interface BiPredicate<T, U> {
        boolean test(T t, U u);
    }

    // Function
    @FunctionalInterface
    public interface Function<T, R> {
        R apply(T t);
    }
    // BiFunction
    @FunctionalInterface
    public interface BiFunction<T, U, R> {
        R apply(T t, U u);
    }

    // UnaryOperator
    @FunctionalInterface
    public interface UnaryOperator<T> extends Function<T, T> {

    }

    // BinaryOperator
    @FunctionalInterface
    public interface BinaryOperator<T> extends BiFunction<T, T, T> {

    }
// Method references lambda expression

    Consumer<String> c = s -> System.out.println(s);
    c.accept("sa");

    Consumer<String> c2 = System.out::println;
    c.accept("sa2");


    Comparator<Integer> c = (i1, i2) Integer.compare(i1, i2)
    //can be written as
    Comparator<Integer> c = Integer::compare;

// list.forEach
    import java.util.ArrayList;
    import java.util.List;

    public class Customer {
        private String name;
        public Customer(String name) {
            this.name = name;
        }

        @Override
        public String toString() {
            return "Customer=" + this.name;
        }

        public static void main(String[] args) {
            final List<Customer> list = new ArrayList<>();
            list.add(new Customer("cust1"));
            list.add(new Customer("cust2"));

            list.forEach(System.out::println);
            // or
            list.forEach(cust -> System.out.println(cust));
        }
    }
// consumer andThen
    import java.util.ArrayList;
    import java.util.List;
    import java.util.function.Consumer;

    class MyCustomer {
        private String name;
        public MyCustomer(String name) {
            this.name = name;
        }
    }

    public class CopyCustomer {
        public static void main(String[] args) {
            MyCustomer myCustomer1 = new MyCustomer("ddd");
            MyCustomer myCustomer2 = new MyCustomer("kutti");

            final List<MyCustomer> list = new ArrayList<>();
            list.add(myCustomer1);
            list.add(myCustomer2);

            final List<MyCustomer> result = new ArrayList<>();
            Consumer<MyCustomer> c1 = System.out::println;
            Consumer<MyCustomer> c2 = result::add;

            list.forEach(c1.andThen(c2));

            System.out.println(result.size());

        }
    }


```