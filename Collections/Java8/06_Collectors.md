# Collectors in Java 8

## 1. Overview & Why Use Collectors?
The `Collectors` utility class (`java.util.stream.Collectors`) provides implementations of the `Collector` interface. It is primarily used with the Stream terminal operation `.collect()` to transform stream elements into collections, maps, aggregated values, or joined strings.

### Key Capabilities
* **Accumulation into Collections**: Convert streams into `List`, `Set`, or custom collections.
* **String Joining**: Concatenate element representations with custom delimiters, prefixes, and suffixes.
* **Summarization & Reduction**: Compute counts, sums, averages, min/max values, or custom reductions.
* **Grouping & Partitioning**: Group elements based on classification functions or split them by boolean predicates.

---

## 2. Collecting to Collections

```java
import java.util.*;
import java.util.stream.*;

public class CollectingToCollections {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Alice");

        // 1. Collect to List
        List<String> listResult = names.stream()
            .collect(Collectors.toList());

        // 2. Collect to Set (removes duplicates)
        Set<String> setResult = names.stream()
            .collect(Collectors.toSet());

        // 3. Collect to Specific Collection (e.g., LinkedList, TreeSet)
        LinkedList<String> linkedListResult = names.stream()
            .collect(Collectors.toCollection(LinkedList::new));

        TreeSet<String> treeSetResult = names.stream()
            .collect(Collectors.toCollection(TreeSet::new));
    }
}
```

---

## 3. Collecting to Map (`toMap`)
`Collectors.toMap()` maps stream elements into key-value pairs.

### Handling Key Collisions
When multiple elements produce the same key, a **merge function** must be supplied to resolve the collision; otherwise, an `IllegalStateException` is thrown.

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.*;

class Employee {
    private int id;
    private String name;
    private String department;

    public Employee(int id, String name, String department) {
        this.id = id;
        this.name = name;
        this.department = department;
    }

    public int getId() { return id; }
    public String getName() { return name; }
    public String getDepartment() { return department; }
}

public class ToMapExample {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee(101, "Alice", "IT"),
            new Employee(102, "Bob", "HR"),
            new Employee(103, "Charlie", "IT") // Duplicate department key if using department as key
        );

        // 1. Simple mapping (Key: ID, Value: Name) - Requires unique keys
        Map<Integer, String> idToNameMap = employees.stream()
            .collect(Collectors.toMap(Employee::getId, Employee::getName));

        // 2. Map with Merge Function to handle duplicate keys
        // (Key: Department, Value: Name, Merge: Keep existing value on collision)
        Map<String, String> deptToEmpMap = employees.stream()
            .collect(Collectors.toMap(
                Employee::getDepartment,
                Employee::getName,
                (existingVal, newVal) -> existingVal + ", " + newVal
            ));

        // 3. Specifying Map Implementation (e.g., TreeMap)
        Map<Integer, Employee> sortedEmpMap = employees.stream()
            .collect(Collectors.toMap(
                Employee::getId,
                Function.identity(),
                (oldVal, newVal) -> newVal,
                TreeMap::new
            ));
    }
}
```

---

## 4. Joining Strings (`joining`)
`Collectors.joining()` concatenates stream elements (converted to strings) with optional delimiters, prefixes, and suffixes.

```java
import java.util.*;
import java.util.stream.*;

public class JoiningExample {
    public static void main(String[] args) {
        List<String> frameworks = Arrays.asList("Spring", "Hibernate", "Docker");

        // 1. Simple concatenation
        String joined1 = frameworks.stream()
            .collect(Collectors.joining()); // SpringHibernateDocker

        // 2. With delimiter
        String joined2 = frameworks.stream()
            .collect(Collectors.joining(", ")); // Spring, Hibernate, Docker

        // 3. With delimiter, prefix, and suffix
        String joined3 = frameworks.stream()
            .collect(Collectors.joining(", ", "[", "]")); // [Spring, Hibernate, Docker]
    }
}
```

---

## 5. Grouping Data (`groupingBy`)
`Collectors.groupingBy()` organizes stream elements into groups based on a classification function (similar to `GROUP BY` in SQL).

```java
import java.util.*;
import java.util.stream.*;

class Item {
    private String name;
    private String category;
    private double price;

    public Item(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    public String getName() { return name; }
    public String getCategory() { return category; }
    public double getPrice() { return price; }
}

public class GroupingExample {
    public static void main(String[] args) {
        List<Item> items = Arrays.asList(
            new Item("Laptop", "Electronics", 1200.0),
            new Item("Phone", "Electronics", 800.0),
            new Item("Shirt", "Apparel", 40.0),
            new Item("Jeans", "Apparel", 60.0)
        );

        // 1. Simple Grouping: Category -> List of Items
        Map<String, List<Item>> byCategory = items.stream()
            .collect(Collectors.groupingBy(Item::getCategory));

        // 2. Grouping with Downstream Collector: Category -> Count of Items
        Map<String, Long> countByCategory = items.stream()
            .collect(Collectors.groupingBy(Item::getCategory, Collectors.counting()));

        // 3. Grouping with Downstream Mapping: Category -> List of Item Names
        Map<String, List<String>> namesByCategory = items.stream()
            .collect(Collectors.groupingBy(
                Item::getCategory,
                Collectors.mapping(Item::getName, Collectors.toList())
            ));

        // 4. Grouping with Custom Map Type (e.g., TreeMap for sorted keys)
        Map<String, Set<String>> sortedNamesByCategory = items.stream()
            .collect(Collectors.groupingBy(
                Item::getCategory,
                TreeMap::new,
                Collectors.mapping(Item::getName, Collectors.toSet())
            ));
    }
}
```

---

## 6. Partitioning Data (`partitioningBy`)
`Collectors.partitioningBy()` is a specialized form of `groupingBy()` that accepts a `Predicate<T>` and splits stream elements into two groups: `true` and `false`.

```java
import java.util.*;
import java.util.stream.*;

public class PartitioningExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(10, 15, 20, 25, 30);

        // Partition numbers into even (true) and odd (false)
        Map<Boolean, List<Integer>> evenOddMap = numbers.stream()
            .collect(Collectors.partitioningBy(n -> n % 2 == 0));

        System.out.println(evenOddMap.get(true));  // [10, 20, 30]
        System.out.println(evenOddMap.get(false)); // [15, 25]

        // Partitioning with downstream collector
        Map<Boolean, Long> evenOddCount = numbers.stream()
            .collect(Collectors.partitioningBy(n -> n % 2 == 0, Collectors.counting()));
    }
}
```

---

## 7. Aggregation & Summarization
Collectors provides built-in methods for computing summary metrics.

```java
import java.util.*;
import java.util.stream.*;

public class AggregationExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(10, 20, 30, 40, 50);

        // Counting
        long count = numbers.stream().collect(Collectors.counting()); // 5

        // Summing
        int sum = numbers.stream().collect(Collectors.summingInt(Integer::intValue)); // 150

        // Averaging
        double avg = numbers.stream().collect(Collectors.averagingInt(Integer::intValue)); // 30.0

        // Min / Max
        Optional<Integer> min = numbers.stream().collect(Collectors.minBy(Comparator.naturalOrder()));

        // Summary Statistics (calculates count, sum, min, average, max in one pass)
        IntSummaryStatistics stats = numbers.stream()
            .collect(Collectors.summarizingInt(Integer::intValue));

        System.out.println("Max: " + stats.getMax());
        System.out.println("Average: " + stats.getAverage());
    }
}
```

---

## 8. Advanced: `collectingAndThen`
`Collectors.collectingAndThen()` applies a finishing transformation function to the output of a primary collector.

```java
import java.util.*;
import java.util.stream.*;

public class CollectingAndThenExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Java", "Spring", "Microservices");

        // Collect to List and make it Unmodifiable
        List<String> unmodifiableList = list.stream()
            .collect(Collectors.collectingAndThen(
                Collectors.toList(),
                Collections::unmodifiableList
            ));

        // Find max element and return value directly instead of Optional
        List<Integer> nums = Arrays.asList(5, 12, 3, 8);
        int maxVal = nums.stream()
            .collect(Collectors.collectingAndThen(
                Collectors.maxBy(Integer::compareTo),
                opt -> opt.orElse(-1)
            ));
    }
}
```

---

## 9. Tricky Interview Questions & Answers

### Q1. What happens if `Collectors.toMap()` encounters duplicate keys without a merge function?
It throws an `IllegalStateException` at runtime (`Duplicate key ...`). To avoid this, always pass a merge function when key uniqueness in the input stream is not guaranteed.

### Q2. What is the key difference between `groupingBy()` and `partitioningBy()`?
* **`groupingBy()`**: Accepts a classification `Function<T, K>`. Can divide data into any number of keys determined by key type `K`.
* **`partitioningBy()`**: Accepts a `Predicate<T>`. Always produces a `Map<Boolean, List<T>>` containing exactly two keys: `true` and `false`. Performance is slightly higher because keys are bounded to booleans.

### Q3. How do you find the highest paid employee in each department using `Collectors`?
Combine `groupingBy` with `maxBy` and `collectingAndThen`:

```java
Map<String, Employee> topEarnerPerDept = employees.stream()
    .collect(Collectors.groupingBy(
        Employee::getDepartment,
        Collectors.collectingAndThen(
            Collectors.maxBy(Comparator.comparingDouble(Employee::getSalary)),
            Optional::get
        )
    ));
```

### Q4. Can `Collectors.toMap()` handle `null` values?
No. `Collectors.toMap()` uses `Map.merge()` under the hood, which throws a `NullPointerException` if the mapped key or value is `null`.

---

## 10. Quick Revision

* **`toList()` / `toSet()` / `toCollection()`**: Standard methods to collect elements into standard Java data structures.
* **`toMap(keyMapper, valueMapper, mergeFunction, mapFactory)`**: Collects to key-value pairs; use a merge function to handle key collisions safely.
* **`joining(delimiter, prefix, suffix)`**: Concatenates string representations cleanly.
* **`groupingBy(classifier, downstream)`**: Groups elements into dynamic categories (`Map<K, List<V>>`).
* **`partitioningBy(predicate)`**: Splitting elements into boolean key pairs (`Map<Boolean, List<V>>`).
* **`collectingAndThen(collector, finisher)`**: Useful for producing unmodifiable collections or unpacking `Optional` values post-reduction.
* **Java 8 Syntax**: Use `Collectors.toList()` instead of Java 16+ `Stream.toList()`.
