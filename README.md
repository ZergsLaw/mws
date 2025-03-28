1.  ```java
    // Дано:
    // Приложение, которое вызывает само себя рекурсивно в блоках try и finally.
    // Вопрос:
    // Как себя поведет это приложение?
    // Сложность: Средняя

    public class Worker {
       public static void main(String[] args) {
           workHard();
           System.out.println("Time to rest.");
       }

       private static void workHard() {
           try {
               workHard();
           } finally {
               workHard();
           }
       }
    }
    ```

2.  ```java
    // Дано:
    // Класс представляет биатлонную мишень.
    // В программе есть два потока – спортсмен и тренер.
    // Спортсмен стреляет, тренер – отслеживает попадания.
    // Вопрос:
    // Может ли что-то пойти не так?
    // Сложность: Сложная

    public class BiathlonTarget {
       private int[] slots = new int[5];

       public void hit(int index) {
           if (index < 0 || index >= 5) {
               return;
           }

           slots[index] = 1;
       }

       public int[] check() {
           return slots.clone();
       }
    }
    ```

3.  ```java
    // Дано:
    // Класс RandomWitness, который в многопоточной среде, в цикле,  либо записывает случайные числа в начало списка,
    // либо выводит 5 последних сгенерированных случайных чисел.
    // Вопрос:
    // Что не так с классом RandomWitness?
    // Сложность: Средняя

    final class RandomWitness implements Runnable {
        private boolean observe = true;
        private boolean report = false;

        private Random random = new Random();

        private List<Integer> keys = new ArrayList<>();

        @Override
        public void run() {
            while (!Thread.currentThread().interrupted()) {
                if (observe) {
                    keys.addFirst(random.nextInt());
                } else if (report) {
                    System.out.println(
                            "The five last randoms are " + Arrays.toString(keys.stream().limit(5).toArray())
                    );
                }
            }
            System.out.println("I'm done.");
        }

        public void observe() {
            observe = true;
            report = false;
        }

        public void report() {
            observe = false;
            report = true;
        }
    }
    ```

4.  ```java
    // Дано:
    // Класс Friends хранит список друзей в коллекции.
    // Метод isMyFriend проверяет, есть ли данный друг в списке.
    // Вопрос:
    // Что не так с этим классом, учитывая, что используется ConcurrentSkipListSet и Comparator?
    // Сложность: Легкая

    public class Friends {
        private Set<Friend> friends = new ConcurrentSkipListSet<>(
                Comparator.comparing(f -> f.birthday)
        );

        void add(Friend friend) {
            friends.add(friend);
        }

        boolean isMyFriend(Friend friend) {
            return friends.contains(friend);
        }

        public static class Friend {
            private String firstName;
            private String lastName;
            private LocalDate birthday;

            public Friend(String firstName, String lastName, LocalDate birthday) {
                this.firstName = firstName;
                this.lastName = lastName;
                this.birthday = birthday;
            }

            @Override
            public boolean equals(Object o) {
                if (o == null || getClass() != o.getClass()) return false;
                Friend friend = (Friend) o;
                return Objects.equals(firstName, friend.firstName) && Objects.equals(lastName, friend.lastName) && Objects.equals(birthday, friend.birthday);
            }
        }
    }
    ```

5.  ```java
    // Дано:
    // Класс FriendsBirthdays, который хранит дни рождения друзей в ConcurrentSkipListMap.
    // Вопрос:
    // Что с ним не так с точки зрения требований к ключам ConcurrentSkipListMap?
    // Сложность: Легкая

    public class FriendsBirthdays {
        private Map<Friend, LocalDate> birthdays = new ConcurrentSkipListMap<>();

        void add(Friend friend, LocalDate date) {
            birthdays.put(friend, date);
        }

        LocalDate find(Friend friend) {
            return birthdays.get(friend);
        }

        public static class Friend {
            private String firstName;
            private String lastName;

            public Friend(String firstName, String lastName) {
                this.firstName = firstName;
                this.lastName = lastName;
            }

            @Override
            public boolean equals(Object o) {
                if (o == null || getClass() != o.getClass()) return false;
                Friend friend = (Friend) o;
                return Objects.equals(firstName, friend.firstName) && Objects.equals(lastName, friend.lastName);
            }
        }
    }
    ```

6.  ```java
    // Дано:
    // Программа с вложенными блоками try-finally, циклами, операторами break, continue и return.
    // Вопрос:
    // Что будет напечатано в результате запуска программы?
    // Сложность: Сложная

    public class TryFinally {
        public static void main(String[] args) {
            try {
                System.out.println(tryFinally());
            } finally {
                System.out.println("7");
            }
        }

        private static boolean tryFinally() {
            while (true) {
                System.out.println("1");
                try {
                    try {
                        try {
                            DO_IT_AGAIN: try {
                                System.out.println("2");
                                throw new Error();
                            } finally {
                                System.out.println("3");
                                break DO_IT_AGAIN;
                            }
                        } finally {
                            System.out.println("4");
                            return true;
                        }
                    } finally {
                        System.out.println("5");
                        continue;
                    }
                } finally {
                    System.out.println("6");
                }
            }
        }
    }
    ```

7.  ```java
    // Дано:
    // Программа, которая работает с переменной типа float, уменьшая её на 0.1f в цикле while.
    // Вопрос:
    // Что напечатает следующая программа?
    // Сложность: Средняя

    public class LittleByLittle {
        public static void main(String[] args) {
            float v = 0.5f;
            System.out.print("Little by little");
            while ((v-=0.1f) != 0) {
                System.out.print(", by little");
            }
            System.out.print(" we cross the line!");
        }
    }
    ```

8.  ```java
    // Дано:
    // Функция, которая определяет, является ли число нечетным, используя оператор остатка от деления.
    // Также в функции складываются символы.
    // Вопрос:
    // Что не так с этой функцией?
    // Сложность: Легкая

    public static boolean isOdd(int value) {
        if (value % 2 == 1) {
            System.out.println('O' + 'd' + 'd');
            return true;
        } else {
            System.out.println('E' + 'v' + 'e' + 'n');
            return false;
        }
    }
    ```

9. ```java
    // Дано:
    // Программа с циклом while, в условии которого используется постфиксный инкремент переменной i.
    // Вопрос:
    // Сколько раз программа напечатает “Working…”?
    // Сложность: Легкая

    public class Looper {
        public static void main(String[] args) {
            int i = 0;
            while((i = i++) < 7) {
                System.out.println("Working…");
            }
        }
    }
    ```

10. ```java
    // Дано:
    // Программа, выполняющая деление 0.0 на 0.0 и сравнивающая результат с 0.0.
    // Вопрос:
    // Что напечатает программа?
    // Сложность: Средняя

    public class ZeroIsZero {
        public static void main(String[] args) {
            double i = 0.0 / 0.0;
            System.out.println(i - i == 0.0);
        }
    }
    ```

11. ```java
    // Дано:
    // Программа с циклом while, условие которого `i != i + 0`.
    // Вопрос:
    // Чему должно равняться i, чтобы программа работала вечно?
    // Сложность: Средняя

    public class Infinity {
        public static void main(String[] args) {
            // i = ?

            while (i != i + 0) {
                System.out.println("To infinity and beyond!");
            }
        }
    }
    ```

12. ```java
    // Дано:
    // Программа, в которой две переменные (i и j) имеют тип Integer и сравниваются.
    // Вопрос:
    // В каком случае для двух переменных, представляющих целые числа, программа напечатает текст?
    // Сложность: Средняя

    public class Impossible {
        public static void main(String[] args) {
            // i = ?
            // j = ?

            if (i <= j && j <= i && i != j) {
                System.out.println("This is it!");
            }
        }
    }
    ```

13. ```java
    // Дано:
    // Класс Null со статическим методом greet.
    // Вопрос:
    // Как поведет себя эта программа, если вызвать метод greet, приведя null к типу Null?
    // Сложность: Легкая

    public class Null {
        public static void greet() {
            System.out.println("Hi, it's null!");
        }

        public static void main(String[] args) {
            ((Null) null).greet();
        }
    }
    ```

14. ```java
    // Дано:
    // Класс SuperReliableCache, в котором есть статический блок инициализации и метод getResult.
    // Вопрос:
    // Какое значение напечатает программа и почему?
    // Сложность: Средняя

    class SuperReliableCache {
        static {
            calculateAndRemember();
        }

        private static int result;

        public static int getResult() {
            calculateAndRemember();
            return result;
        }

        private static boolean initialized = false;

        private static synchronized void calculateAndRemember() {
            if (!initialized) {
                for (int i = 0; i < 3; i++) {
                    result += i;
                }
                initialized = true;
            }
        }

        public static void main(String[] args) {
            System.out.println(SuperReliableCache.getResult());
        }
    }
    ```

15. ```java
    // Дано:
    // Класс Printer с методом print, принимающим переменное число строковых аргументов.
    // Вопрос:
    // Что не так с классом, учитывая использование побитового И (&) в условиях?
    // Сложность: Средняя

    public class Printer {
        public boolean print(String... args) {
            if (args.length != 0 & args[0].equals("Print once")) {
                System.out.println(args);
                return true;
            } else if (args.length != 0 & args[0].equals("Print twice")) {
                System.out.println(args);
                System.out.println(args);
                return true;
            } else {
                return false;
            }
        }
    }
    ```

16. ```java
    // Дано:
    // Программа, выполняющая последовательное приведение типов для числа -1.
    // Вопрос:
    // Что напечатает программа?
    // Сложность: Средняя

    public class Converter {
        public static void main(String[] args) {
            System.out.println((int) (char) (byte) (double) -1);
        }
    }
    ```

17. ```kotlin
    // Дано:
    // Функция, присваивающая значение nullable переменной `a` non-nullable переменной `b`.
    // Вопрос:
    // Какие значения будут выведены?
    // Сложность: Легкая
    fun NullableMadness(){
            val a: String? = "Kotlin"
            val b: String = a!!
            println(a == b)
            println(a === b)
    }
    ```

18. ```kotlin
    // Дано:
    // Функция, использующая умное приведение типов (smart cast) и явное приведение типов (as).
    // Вопрос:
    // Что произойдет с кодом при приведении к Int?
    // Сложность: Средняя
    fun SmartCastOrNotSmartCast() {
        val x: Any = "Hello"
        if (x is String) {
            println(x.length)
        }
        x as String
        println(x.length)
        x as Int
        println(x)
    }
    ```

19. ```kotlin
    // Дано:
    // Класс A с полем value. Функция, создающая два экземпляра класса A, используя apply и also.
    // Вопрос:
    // Какие значения value будут у объектов a и b?
    // Сложность: Средняя

    class A {
        var value = 10
    }

    fun ApplyPuzzle() {
        val a = A().apply {
            value += 5
        }
        println(a.value)
        val b = A().also {
            it.value += 5
        }
        println(b.value)
    }
    ```

20. ```kotlin
    // Дано:
    // Переменная x инициализируется с помощью `by lazy`.  Внутри блока `lazy` есть вывод на печать.
    // Вопрос:
    // Сколько раз выведется "Lazy init"?
    // Сложность: Легкая

    val x: Int by lazy {
        println("Lazy init")
        42
    }

    @Test
    fun LazyOrNotLazy() {
        println("Before first access")
        println(x)
        println("Before second access")
        println(x)
    }
    ```

21. ```kotlin

    // Дано:
    // Функции, использующие scope functions run и let для преобразования строки.
    // Вопрос:
    // Всегда ли `result1` будет равен `result2`?
    // Сложность: Легкая

    fun ScopeFunction() {
        val result1 = "Kotlin".run {
            this.reversed()
        }
        val result2 = "Kotlin".let {
            it.reversed()
        }
        println(result1 == result2)
    }
    ```

22. ```kotlin
    // Дано:
    // Две пары переменных, к которым применяются префиксный и постфиксный инкремент.
    // Вопрос:
    // Что выведет программа?
    // Сложность: Легкая

    fun IncrementMagic() {
        var a = 5
        val b = a++
        println("$a $b")

        var x = 5
        val y = ++x
        println("$x $y")
    }
    ```

23. ```kotlin
    // Дано:
    // Функция, использующая выражение `when` для проверки типа переменной.
    // Вопрос:
    // Что выведет программа?
    // Сложность: Легкая

    fun MagicWhen() {
        val value: Any = "42"
        when (value) {
            42 -> println("Int 42")
            "42" -> println("String 42")
            else -> println("Something else")
        }
    }
    ```

24. ```kotlin
    // Дано:
    // Создаются список (list) и множество (set), содержащие одинаковые элементы.
    // Вопрос:
    // Что выведет программа при сложении списка и множества в разном порядке?
    // Сложность: Легкая
    fun SetListAndOthers() {
        val list = listOf(1, 2, 3)
        val set = setOf(1, 2, 3)

        println(list + set)
        println(set + list)
    }
    ```

25. ```kotlin

    // Дано:
    // Создается изменяемый список (mutableListOf), ссылка на который объявлена как `val`.
    // Вопрос:
    // Почему val позволяет менять содержимое списка?
     // Сложность: Легкая
    fun changableVal(){
        val list = mutableListOf(1, 2, 3)
        list.add(4)
        println(list)
    }

    ```

26. ```kotlin
    // Дано:
    // Цикл forEach, внутри которого есть return.
    // Вопрос:
    // Будет ли напечатано " Done"? Почему?
    // Сложность: Средняя

    fun CraftyForEach() {
        listOf(1, 2, 3, 4, 5).forEach {
            if (it == 3) return
            print(it)
        }
        println(" Done")
    }
    ```

27. ```kotlin
    // Дано:
    // Сравниваются ссылки на объекты Integer с использованием оператора `===`.
    // Вопрос:
    // Что будет напечатано?
    // Сложность: Средняя

    fun StrangeEquals() {
        val a: Int = 127
        val b: Int = 127
        println(a === b)
        val t = 64
        val x: Int = t + t
        val y: Int = 2  * t
        println(x === y)
    }
    ```

28. ```kotlin
    //Дано:
    // функция test, которая явно ничего не возвращает (void).
    //Вопрос: Что будет выведено?
    //Сложность: Легкая
    fun MultiplyUnit() {
        println(test() == Unit)
    }

    fun test() {
        return
    }
    ```

29. ```java
    //Дано:
    //Есть код ядра системы управления лифтом.  Лифт движется в одном направлении.
    //Класс Elevator содержит список этажей, флаг направления движения и номер текущего этажа.
    //Вопрос:
    //Оцените сложность данного решения и предложите оптимизации.
    //Сложность: Средняя
    public class Elevator {
        private LinkedList<Integer> list =  new LinkedList<>();
        private boolean moveUp = true;
        private int lastFloor = 1;

        public void add(Integer newFloor){
            for(var floor : list){
                if (newFloor.equals(floor)){
                    return ;
                }
            }
            list.add(newFloor);
        }

        public int findNextFloor(int startFloor){
            int answer = -1;
            if (moveUp){
                for(var floor : list){
                    if (floor > startFloor && (answer == -1 || answer > floor)){
                        answer = floor;
                    }
                }
            }else {
                for(var floor : list){
                    if (floor < startFloor && (answer == -1 || answer < floor)){
                        answer = floor;
                    }
                }
            }
            return answer;
        }
        public int findNext(){
            var answer = findNextFloor(lastFloor);
            if (answer == -1){
                moveUp = !moveUp;
                answer = findNextFloor(lastFloor);
                if (answer == -1){
                    moveUp = !moveUp;
                }
            }
            if (answer != -1){
                lastFloor = answer;
            }
            return lastFloor;
        }
    }
    ```

30. ```java
    //Дано:
    //Класс SimpleCompareString, в котором сравниваются строки.
    //Вопрос:
    //Что выведет данная программа?
    //Сложность: Легкая
    public class SimpleCompareString {

        public String value(){
            return "test";
        }

        public void compare(){
            var str1 = value();
            var str2 = value();
            System.out.println(str1 == str2);
            var str3 = new String("test");
            var str4 = new String("test");
            System.out.println(str3 == str4);
            System.out.println(str1 == str4);
        }
    }

    ```

31. ```kotlin
    // Дано:
    // Корутина, в которой запускается suspend функция, увеличивающая переменную value внутри блока, защищенного ReentrantLock.
    // Вопрос: Как отработает данный код и завершится ли он?
    // Сложность: Сложная
    class CoroutineTest {
        fun main() = runBlocking {
            var value = 0
            val lock = ReentrantLock()
            Array(1000) {
                launch(Dispatchers.Default) {
                    lock.lock()
                    try {
                        delay(1)
                        ++value
                    } finally {
                        lock.unlock()
                    }
                }
            }.forEach { it.join() }

            assertEquals(1000, value)
        }
    }
    ```

32. ```java
    //Дано:
    //Приведен код, в котором создается неизменяемый Map, и предпринимаются попытки изменить его.
    //Вопрос: Что не так с данным кодом?
    //Сложность: Средняя
    public class MapCheck {
        public static void main(String[] args) {
            Map map = Map.of(1, "A", 2, "B");
            Collection data = map.keySet();
            System.out.println(data);

            data.remove(1);
            System.out.println(data);
            System.out.println(map);
            data.add(3);
        }
    }
    ```

33. ```java
    // Дано:
    // В облачном сервисе есть счётчик обрабатываемых запросов.
    // Приведён код обработчика, где каждый вызов `handleRequest()` инкрементирует счётчик.
    // Вопросы к участнику:
    // Почему счётчик может оказаться неточным в многопоточной среде?
    // Как исправить код?
    // Сложность: Легкая
    public class RequestHandler {
        private static int requestCount = 0;

        public void handleRequest() {
            // Обработка запроса (детали опущены)
            requestCount++;
        }

        public int getRequestCount() {
            return requestCount;
        }
    }
    ```

34. ```java
    // Дано:
    // Код банковского перевода между счетами, использующий вложенную синхронизацию.
    // Вопросы к участнику:
    // Что пошло не так в данном коде?  Как это влияет на работу?
    // Как устранить проблему?
    // Сложность: Легкая

    public class Account {
        private int balance;

        public Account(int balance) {
            this.balance = balance;
        }

        public void transfer(Account target, int amount) {
            synchronized(this) {
                synchronized(target) {
                    if (amount > 0 && this.balance >= amount) {
                        this.balance -= amount;
                        target.balance += amount;
                    }
                }
            }
        }

        public int getBalance() {
            return balance;
        }
    }
    ```

35. ```java
    // Дано:
    // Код, который запускает новый поток для обработки каждой задачи.
    // Вопросы к участнику:
    // Какие проблемы возникнут при таком подходе?
    // Почему это плохо масштабируется в облачной среде?
    // Предложите более эффективное решение.
    // Сложность: Легкая
    public class TaskProcessor {
        public void processTasks(List<Runnable> tasks) {
            for (Runnable task : tasks) {
                new Thread(task).start();
            }
        }
    }
    ```

36. ```java
    // Дано:
    // Код, реализующий паттерн Singleton с использованием двойной проверки блокировки.
    // Вопросы к участнику:
    // Может ли в этом коде произойти ошибка в многопоточной среде?
    // Опишите, в чём она заключается.
    // Как гарантировать корректность и потокобезопасность?
    // Сложность: Легкая
    public class SingletonService {
        private static SingletonService instance;

        private SingletonService() {
            // приватный конструктор
        }

        public static SingletonService getInstance() {
            if (instance == null) {                      // Первая проверка
                synchronized(SingletonService.class) {
                    if (instance == null) {              // Вторая проверка
                        instance = new SingletonService();
                    }
                }
            }
            return instance;
        }
    }
    ```

37. ```java
    // Дано:
    // Система, в которой многопоточно обрабатываются задания. Задания помещаются в блокирующую очередь.
    // Вопросы к участнику:
    // 1. В чём могут быть причины "зависания" заданий?
    // 2. Нужно ли что-то менять в механизме `poll` / `add` / `LinkedBlockingQueue`?
    // 3. Почему система может терять производительность, либо задания не обрабатываются?
    // 4. Какие решения вы предложите (реализации, настройки)?
    // Сложность: Средняя
    import java.util.concurrent.BlockingQueue;
    import java.util.concurrent.LinkedBlockingQueue;
    import java.util.concurrent.TimeUnit;

    public class TaskSystem {
        private final BlockingQueue<String> queue = new LinkedBlockingQueue<>();

        public void produce(String task) {
            queue.add(task);
            System.out.println("Produced: " + task);
        }

        public void consume() {
            try {
                String task = queue.poll(1, TimeUnit.SECONDS);
                if (task != null) {
                    System.out.println("Consumed: " + task);
                } else {
                    System.out.println("No task found");
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
    ```

38. ```java
    // Дано:
    // В облаке используется кэш, где разные потоки читают и записывают данные.
    // Код, в котором при первом обращении данные вычисляются и сохраняются, а при последующих возвращаются из памяти.
    // Вопросы к участнику:
    // 1. Почему `ConcurrentHashMap` не решает проблему одновременного вычисления?
    // 2. Как предотвратить повторные расчёты?
    // 3. Какие подходы существуют для эффективного и потокобезопасного кэша?
    // Сложность: Средняя
    import java.util.Map;
    import java.util.concurrent.ConcurrentHashMap;

    public class CacheService {
        private final Map<String, Object> cache = new ConcurrentHashMap<>();

        public Object getData(String key) {
            Object value = cache.get(key);
            if (value == null) {
                // Дорогостоящая операция вычисления
                value = expensiveCompute(key);
                cache.put(key, value);
            }
            return value;
        }

        private Object expensiveCompute(String key) {
            // Имитация длительной операции
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            return "ValueFor:" + key;
        }
    }
    ```

39. ```java
    // Дано:
    // Разработчик использует пул потоков, но забыл останавливать пул при завершении.
    // Вопросы к участнику:
    // 1. В чём проблема?
    // 2. Зачем нужно вызывать `shutdown()` и/или `shutdownNow()`?
    // 3. Какие лучшие практики управления жизненным циклом ExecutorService?
    // Сложность: Средняя
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    public class App {
        private final ExecutorService executor = Executors.newFixedThreadPool(4);

        public void runTasks() {
            for (int i = 0; i < 100; i++) {
                executor.submit(() -> {
                    // Имитация работы
                    try {
                        Thread.sleep(50);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                });
            }
        }

        public static void main(String[] args) {
            App app = new App();
            app.runTasks();
            // Завершение main
        }
    }
    ```

40. ```java
    // Дано:
    // В облачной системе настройки могут обновляться динамически.
    // Есть статический класс, который хранит конфигурацию.
    // Вопросы к участнику:
    // 1. Как возможно получить NPE, если строка не должна быть null?
    // 2. Является ли `volatile` надёжным решением?
    // 3. Как лучше организовать переключения режима?
    // Сложность: Средняя

    public class Config {
        public static volatile String currentMode = "DEFAULT";
    }

    public class ModeService {
        public static void setMode(String mode) {
            Config.currentMode = mode;
        }

        public static void doWork() {
            if (Config.currentMode.equals("DEFAULT")) {
                // Выполнить логику по умолчанию
            } else {
                // Выполнить другой режим работы
            }
        }
    }
    ```

41. ```java
    // Дано:
    // Реализация односвязного стека с атомарными операциями.
    // Вопросы к участнику:
    // 1. В чём состоит проблема ABA?
    // 2. Какие подходы для обхода ABA (например, `AtomicStampedReference`)?
    // 3. Нужно ли беспокоиться о проблеме ABA во всех системах?
    // Сложность: Сложная
    import java.util.concurrent.atomic.AtomicReference;

    public class LockFreeStack<T> {
        static class Node<T> {
            final T value;
            final Node<T> next;
            Node(T value, Node<T> next) {
                this.value = value;
                this.next = next;
            }
        }

        private final AtomicReference<Node<T>> head = new AtomicReference<>(null);

        public void push(T value) {
            Node<T> newNode = new Node<>(value, null);
            while (true) {
                Node<T> current = head.get();
                newNode = new Node<>(value, current);
                if (head.compareAndSet(current, newNode)) {
                    break;
                }
            }
        }

        public T pop() {
            while (true) {
                Node<T> current = head.get();
                if (current == null) {
                    return null;
                }
                Node<T> next = current.next;
                if (head.compareAndSet(current, next)) {
                    return current.value;
                }
            }
        }
    }
    ```

42. ```java
    // Дано:
    // Код, который параллельно обрабатывает список строк и ведет подсчёт уникальных слов с помощью `HashSet`.
    // Вопросы к участнику:
    // 1. Почему некорректно использовать `HashSet` внутри параллельного стрима?
    // 2. Какие способы для безопасного подсчёта уникальных элементов?
    // 3. Какую роль играет _недетерминированность_ порядка?
    // Сложность: Сложная
    import java.util.*;

    public class ParallelStreamIssue {
        public static void main(String[] args) {
            List<String> data = List.of("apple", "banana", "apple", "orange", "banana", "peach");
            Set<String> uniqueWords = new HashSet<>();

            data.parallelStream()
                .filter(s -> {
                    uniqueWords.add(s);
                    return s.startsWith("a");
                })
                .forEach(System.out::println);

            System.out.println("Unique words: " + uniqueWords);
        }
    }
    ```

43. ```java
    // Дано:
    // Асинхронные вызовы связаны цепочкой `CompletableFuture`.
    // Часть вычислений завершается с ошибкой, но она не обрабатывается.
    // Вопросы к участнику:
    // 1. Почему исключение не приводит к остановке?
    // 2. Как правильно обрабатывать ошибки в `CompletableFuture`?
    // 3. Как ожидать завершения цепочки?
    // Сложность: Сложная
    import java.util.concurrent.CompletableFuture;

    public class AsyncChain {
        public static void main(String[] args) {
            CompletableFuture.supplyAsync(() -> {
                // имитация ошибки
                throw new RuntimeException("Step1 failed");
            }).thenApply(result -> {
                System.out.println("Step2 received: " + result);
                return result.toUpperCase();
            }).thenAccept(finalResult -> {
                System.out.println("Final: " + finalResult);
            });

            // main завершится сразу
        }
    }
    ```

44. ```java
    // Дано:
    // Упрощённая реализация кольцевого буфера для одного продюсера и одного потребителя с использованием `AtomicLong` для индексов.
    // Вопросы к участнику:
    // 1. Почему в некоторых сценариях (даже в моделе SPSC — один продюсер, один потребитель) могут возникнуть недочёты без `compareAndSet`?
    // 2. Как обеспечить корректность и избежать потерь/дублирования данных, если продюсер и потребитель работают асинхронно и быстро?
    // 3. Какие проблемы могут появиться, если нужно поддерживать несколько продюсеров/потребителей (MPMC)?
    // 4. Какие приёмы можно применить для избежания ложного совместного использования кэша (false sharing)?
    // 5. В чём разница между блокирующим и безблокирующим кольцевым буфером?
    // Сложность: Сложная
    import java.util.concurrent.atomic.AtomicLong;

    public class SingleProducerSingleConsumerRingBuffer<T> {
        private final Object[] buffer;
        private final int size;

        private final AtomicLong head = new AtomicLong(0); // индекс на чтение
        private final AtomicLong tail = new AtomicLong(0); // индекс на запись

        public SingleProducerSingleConsumerRingBuffer(int capacity) {
            this.size = capacity;
            this.buffer = new Object[capacity];
        }

        // Добавление элемента в буфер
        public boolean offer(T element) {
            long currentTail = tail.get();
            long currentHead = head.get();

            // Проверяем, не переполнен ли буфер
            if (currentTail - currentHead >= size) {
                // Буфер полон
                return false;
            }

            int index = (int) (currentTail % size);
            buffer[index] = element;
            tail.set(currentTail + 1); // CAS можно добавить
            return true;
        }

        // Извлечение элемента из буфера
        @SuppressWarnings("unchecked")
        public T poll() {
            long currentHead = head.get();
            long currentTail = tail.get();

            // Проверяем, не пуст ли буфер
            if (currentHead >= currentTail) {
                // Буфер пуст
                return null;
            }

            int index = (int) (currentHead % size);
            T element = (T) buffer[index];
            buffer[index] = null; // освободить ссылку
            head.set(currentHead + 1); // CAS можно добавить
            return element;
        }
    }
    ```


