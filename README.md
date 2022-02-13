## Разбор кода с точки зрения работы JVM.

```java

Класс JvmComprehension из файла с байткодом обрабатывается подсистемой загрузчиков классов . Один из class loader, в зависимости от определенного типа класса, отправляет класс в выделенную в памяти область Metaspace. Импортируются данные для типа его имени. Происходит связывание, т.е. проверка правильности импортируемого класса, выделение памяти для переменных (статических) и их значений, а также преобразование символьных ссылок в прямые. После происходит сопоставление перменных класса их правильным начальным значеням (инициализация). Далее работает компонент Runtime Memory/Data Area.

public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1 После создания в Stack Memory фрейма для метода Main в него помещается значение примитивного типа int
        Object o = new Object();        // 2 Оператором new выделается память в heap и с помощью контсруктора без параметров создается экземпляр класса Object, расположенный в выделенной области. Cсылка "o" на объект Object из heap помещается в Stack Memory. 
        Integer ii = 2;                 // 3 Создается экземпляр класса Integer в heap. Ссылка на объект и его значение в области heap помещается в тот же фрейм "main()".
        printAll(o, i, ii);             // 4 Создается новый фрейм printAll() в Stack Memory, в него перадются ссылки на o, i, ii из heap
        System.out.println("finished"); // 7 Создается новый фрейм в Stack Memory. В данный фрейм передается ссылка на экземпляр класса String с содержимым "finished", расположенный в heap.
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5 В созданном ранее фрейме printAll() помещается ссылка uselessVar на эземпляр класса Integer и его значение расположенные в heap.
        System.out.println(o.toString() + i + ii);  // 6 Создается новый фрейм System.out.println() в Stack Memory. В него помещаются ссылки на "i", "ii" и ссылка на новый объект полученный при вызове toString. 
    }
}

```

После обрабоботки байткода JVM и загрузки его в основную память он выполняется при помощи Интерпертатора и JIT-компилятора. Интерпретатор интерпретирует байт код строка за строкой и затемвыполняет его, а JIT - компилятор преобразует код в машинный (компилирует) и сохраняет в кэше.
