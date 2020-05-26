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


```
