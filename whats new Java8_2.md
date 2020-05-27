```java
// Map, Filter, Reduce(group by)

// Map: sample stream and chain consumer
    import java.util.ArrayList;
    import java.util.List;
    import java.util.function.Consumer;

    public class SampleStream {
        public static void main(String[] args) {
            final List<String> persons = new ArrayList<>();
            persons.add("ddd");
            persons.add("kutti");

            List<String> result = new ArrayList<>();
            Consumer<String> consumer1 = result::add;
            Consumer<String> consumer2 = System.out::println;

            persons.stream().forEach(consumer1.andThen(consumer2));

            System.out.println(">from result list");
            result.forEach(consumer2);

        }
    }
// Filter

    import java.util.ArrayList;
    import java.util.List;
    import java.util.function.Predicate;
    import java.util.stream.Stream;

    class Person {
        private String name;
        private int age;

        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }
    }

    public class SampleFilter {
        public static void main(String[] args) {
            final List<Person> list = new ArrayList<>();

            Person person1 = new Person("ddd", 42);
            Person person2 = new Person("kutti", 8);

            list.add(person1);
            list.add(person2);

            Predicate<Person> agePredicate = p -> p.getAge() < 10;
            final Stream<Person> filterStream = list.stream().filter(agePredicate);
            filterStream.forEach(p -> System.out.println(p.getAge()));

        }
    }

//  Predicate isEqual  -------------------------------------
    import java.util.function.Predicate;
    import java.util.stream.Stream;

    public class PredIsEqual {
        public static void main(String[] args) {
            Predicate<String> p = Predicate.isEqual("two");

            Stream stream = Stream.of("one", "two", "three");
            Stream filtered = stream.filter(p);
            filtered.forEach(System.out::println);
        }
    }

//  peek intermediate operation  -------------------------------------
    import java.util.ArrayList;
    import java.util.List;

    public class SamplePeek {
        public static void main(String[] args) {
            List<String> list = new ArrayList<>();
            list.add("one");
            list.add("two");
            list.add("three");
    // peek (intermediate operation, results stream()
    // where as forEach return void
            list.stream().peek(System.out::println).
                    filter(s -> s.equals("two")).
                    forEach(System.out::println);
        }
    }
// peek ( no output)------------------------------------
    import java.util.ArrayList;
    import java.util.List;
    import java.util.function.Predicate;
    import java.util.stream.Stream;

    public class IntermediaryFinal {
        public static void main(String[] args) {
            Stream<String> stream = Stream.of("one", "two", "three", "four");

            Predicate<String> p1 = Predicate.isEqual("two");
            Predicate<String> p2 = Predicate.isEqual("three");

            List<String> list = new ArrayList<>();

            stream.peek(System.out::println)
                    .filter(p1.or(p2))
                    .peek(list::add);

        }
    }

// flatMap ----------------------------------------------------------

    import java.util.Arrays;
    import java.util.List;

    public class SampleFlatMap {
        public static void main(String[] args) {
            List<Integer> list1 = Arrays.asList(1,2,3,4);
            List<Integer> list2 = Arrays.asList(5,6,7);
            List<Integer> list3 = Arrays.asList(8,9,10);

            List<List<Integer>> list = Arrays.asList(list1, list2, list3);

            list.stream().map(l -> l.stream()).forEach(System.out::println);
    //        java.util.stream.ReferencePipeline$Head@5f184fc6
    //        java.util.stream.ReferencePipeline$Head@3feba861
    //        java.util.stream.ReferencePipeline$Head@5b480cf9

            list.stream().flatMap(l -> l.stream()).forEach(System.out::print);
    //        12345678910
        }
    }

// reduce ----------------------------------------------------------

    import java.util.ArrayList;
    import java.util.List;

    public class SampleReduce {
        public static void main(String[] args) {
            List<Integer> list = new ArrayList<>();
            list.add(1);
            list.add(2);
            list.add(3);
            list.add(4);
            list.add(5);

            Integer sum = list.stream().reduce(0, (x1, x2) -> x1 + x2);
            System.out.println(sum);

        }
    }

// min ----------------------------------------------------------

    import java.util.ArrayList;
    import java.util.Comparator;
    import java.util.List;

    public class GetMinAge {
        public static void main(String[] args) {
            final List<Person> persons = new ArrayList<>();
            final Person person1 = new Person("ddd", 42);
            final Person person2 = new Person("kutti", 38);
            final Person person3 = new Person("kutti2", 8);

            persons.add(person1);
            persons.add(person2);
            persons.add(person3);

            Integer minAge = persons.stream().map(p -> p.getAge()).filter(a -> a < 40)
                    .min(Comparator.naturalOrder()).get();

            System.out.println(minAge);
        }
    }

// Optional ----------------------------------------------------------

    import java.util.ArrayList;
    import java.util.Arrays;
    import java.util.List;
    import java.util.Optional;

    public class SampleOptional {
        public static void main(String[] args) {
            List<Integer> list = Arrays.asList(10, 20, 30);

            Optional<Integer> minInt = list.stream().min(Integer::min);
            System.out.println(minInt.isPresent() ? minInt.get() : "no value found");

        }
    }

// Collecting in a String ----------------------------------------------------------

    import java.util.ArrayList;
    import java.util.List;
    import java.util.stream.Collectors;

    public class CollectAsString {
        public static void main(String[] args) {
            List<Person> list = new ArrayList<>();
            list.add(new Person("ddd", 42));
            list.add(new Person("daa", 38));
            list.add(new Person("dii", 7));

            String name = list.stream().map(Person::getName).collect(Collectors.joining(","));
            System.out.println("names=" + name);

        }
    }

// age : Person
// Map<Integer, List<Person>> result = ... .collect(Collectors.groupingBy(Person::getAge))

// age : groupByCount
// Map<Integer, Long> result = ... .collect(Collectors.groupingBy(Person::getAge, Collectors.counting())
```
