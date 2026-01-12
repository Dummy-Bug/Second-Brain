# Java Core Utilities Reference

## String, Character, Integer Conversion

-   `Integer.parseInt(s)`: String to int (primitive).
-   `Integer.valueOf(s)`: String to Integer (Object).
-   `String.valueOf(int)`: int to String.
-   `String str = new String(chArray)`: char[] to String.
-   `String[] arr = list.toArray(new String[list.size()])`: List\<String\> to String[].
-   `List<String> list = Arrays.asList(arr)`: String[] to List\<String\> (fixed-size, modify elements, no add/remove).

## String Manipulation

-   `s.charAt(i)`: Character access.
-   `s.length()`: Length.
-   `s.substring(0, 1)`: Substring [0, 1).
-   `s.substring(1)`: Substring [1, s.length()).
-   `s.equals("b")`: Equality.
-   `s.compareTo("b*c*d")`: Lexicographical comparison (returns -1 if s < "b*c*d").
-   `s.trim()`: Trim leading/trailing spaces.
-   `s.indexOf("a")`: Index of substring.
-   `s.indexOf('a', 2)`: Index from position 2.
-   `s.lastIndexOf('a')`: Last index.
-   `s.replaceAll(substr, target)`: Replace all occurrences.
-   `char[] arr = s.toCharArray()`: String to char[].
-   `String[] arr = s.split("\\*")`: Split by '\*'.
-   `String[] arr = s.split("\\.")`: Split by '.'.
-   `String res = String.join(String delimiter, List<String> data)`: Join strings with delimiter.
-   `Objects.equals(Object a, Object b)`: Null-safe equality.

## StringBuilder

-   `StringBuilder sb = new StringBuilder()`: Initialize StringBuilder.
-   `sb.append("a")`: Append.
-   `sb.insert(0, "a")`: Insert.
-   `sb.deleteCharAt(int index)`: Delete character.
-   `sb.reverse()`: Reverse.
-   `sb.toString()`: Convert to String.
-   `sb.length()`: Length.

## Arrays

-   `int[] arr = new int[10]`: Initialize int array.
-   `Arrays.sort(arr)`: Sort array.
-   `Arrays.fill(arr, -1)`: Fill with value.
-   `helper(new int[]{1, 2})`: Initialize array in method call.

## HashMap, TreeMap, HashSet, TreeSet

-   `HashMap<Character, Integer> map = new HashMap<>()`: Initialize HashMap.
-   `map.put('c', 1)`: Put key-value pair.
-   `map.get('c')`: Get value.
-   `map.getOrDefault(key, defaultValue)`: Get value or default.
-   `map.remove('c')`: Remove key-value pair.
-   `map.computeIfAbsent(key, mappingFunction)`: Compute if absent.
-   `map.containsKey('c')`: Check key existence.
-   `map.containsValue(1)`: Check value existence.
-   Iterate: `keySet()`, `values()`, `entrySet()`, `forEach()`.
-   `map.isEmpty()`, `map.size()`: Size and empty checks.
-   `HashSet<Integer> set = new HashSet<>()`: Initialize HashSet.
-   `set.add(10)`, `set.remove(10)`, `set.contains(10)`, `set.size()`, `set.isEmpty()`.
-   `set.retainAll(setB)`: Intersection.
-   `setB.removeAll(setC)`: Difference.
-   `setC.addAll(setD)`: Union.
-   `setC.containsAll(setD)`: Subset check.
-   `Object[] arr = setA.toArray()`: Set to array.
-   `TreeMap<Integer, String> map = new TreeMap<>()`: Initialize TreeMap (ascending order).
-   `TreeMap<String, Integer> treeMap = new TreeMap<>()`: TreeMap (lexicographical order).
-   `TreeMap<Integer, Integer> treeMap = new TreeMap<>(Collections.reverseOrder())`: TreeMap (descending order).
-   `treeMap.lowerKey(k)`, `treeMap.floorKey(k)`, `treeMap.higherKey(k)`, `treeMap.ceilingKey(k)`, `treeMap.firstKey()`.
-   `SortedMap<K, V> portionOfTreeMap = treeMap.headMap(K toKey)`.
-   `NavigableMap<K, V> map = treeMap.headMap(toKey, true)`.
-   `TreeSet<Integer> treeSet = new TreeSet<>()`: Initialize TreeSet (ascending order).
-   `treeSet.lower(Integer e)`, `treeSet.floor(Integer e)`, `treeSet.ceiling(Integer e)`, `treeSet.higher(Integer e)`, `treeSet.first()`, `treeSet.last()`.
-   `LinkedHashMap/LinkedHashSet`: Maintain insertion order.

## List, ArrayList, LinkedList

-   `List<Integer> list = new ArrayList<>()`: Initialize ArrayList.
-   `list.add(14)`, `list.add(0, 10)`, `list.get(int index)`, `list.remove(list.size() - 1)`, `list.set(int index, int val)`, `list.indexOf(Object o)`, `list.subList(int fromIndex, int toIndex)`.
-   `Collections.sort(list)`, `Collections.sort(list, Collections.reverseOrder())`, `Collections.sort(list, new Comparator<Integer>() { ... })`.
-   `list.forEach(num -> System.out.println(num))`: Iterate with lambda.

## Stack, Queue, PriorityQueue, Deque

-   `Stack<Integer> stack = new Stack<>()`.
-   `Queue<Integer> q = new LinkedList<>()`.
-   `PriorityQueue<Integer> pq = new PriorityQueue<>()`: Min-heap by default.
-   `class Node implements Comparable<Node> { ... }`, `PriorityQueue<Node> pq = new PriorityQueue<>()`.
-   `Deque<Integer> dq = new LinkedList<>()`: Monotone queue.

## Enum

-   `EnumSet.of(...)`, `EnumSet.complementOf(...)`, `EnumSet.allOf(...)`, `EnumSet.range(...)`.

## Random, Math, Collections, Objects, Lambda

-   `Random rand = new Random()`.
-   `Math.pow(x, y)`, `Math.round(a)`, `Math.abs(val)`, `Math.sqrt()`, `Math.sin(rad)`, `Math.PI`, `Math.E`.
-   `Collections.nCopies(n, obj)`, `getClass()`, `Collections.singletonList(obj)`, `Collections.unmodifiableSet(set)`, `Collections.swap(list, i, j)`.
-   `@FunctionalInterface interface Sprint { ... }`, `a -> a.canHop()`.

## Standard I/O, File I/O, Network I/O

-   `Scanner`, `BufferedReader`, `BufferedWriter`, `URL`.

## Atomic Classes

-   `AtomicInteger`, `AtomicBoolean`, `AtomicReference`, `AtomicStampedReference`.

## ExecutorService (Threads)

-   `Executors.newSingleThreadExecutor()`, etc.
-   `Future`, `Callable`, `Runnable`, `service.execute()`, `service.submit()`, `service.shutdown()`, etc.

## Synchronized Block/Method, Concurrent Collections

-   `synchronized(obj) {}`, synchronized method.
-   `ConcurrentHashMap`, `ConcurrentLinkedQueue`, `BlockingQueue`, `CopyOnWriteArrayList`, `ConcurrentSkipListMap`, etc.

## Generics, CountDownLatch, OOP, NIO, I/O

-   Generics (extends, super), CountDownLatch, static, final, Java NIO, Java I/O.