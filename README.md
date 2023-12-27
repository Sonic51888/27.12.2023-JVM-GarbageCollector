# 27.12.2023-JVM-GarbageCollector

// Application ClassLoader дилегирует поиск в Platform ClassLoader
// Platform ClassLoader делегирует поиск в Bootstrap ClassLoader
// Bootstrap ClassLoader ничего не находит, делегирует поиск в Platform ClassLoader
// Platform ClassLoader ничего не находит, передает поиск в Application ClassLoader
// Application ClassLoader находит класс, так как он созданный нами и подгружает его.

public class JvmComprehension {

    public static void main(String[] args) { // на стеке создается Фрейм вызова метода main
        int i = 1;                      // 1 инициализируется на стеке переменная i и присваивается значение 1
        Object o = new Object();        // 2 оператор new выделит в куче память под создаваемый объект в куче Object далее повторится цикл подгрузки ClassLoader, после выполнится конструктор который проведет инициализацию объекта и после передается ссылка на объект в фрейм main в переменную о
        Integer ii = 2;                 // 3 в куче создается объект Integer, в метод main передается ссылка на объект в переменную ii, объекту присваивается число 2.
        printAll(o, i, ii);             // 4 на стеке создается фрейм вызова метода printAll, далее в переменную о фрейма printAll передается ссылка из переменной о фрейма Main
        // далее в переменную i фрейма printAll копируется значение из переменной i фрейма Main
        // далее в переменную ii фрейма printAll передается ссылка из переменной ii фрейма Main
        System.out.println("finished"); // 7 закрывает фрейм метода System.out.println  выполняющийся в фрейме printAll  и удаляется ссылка на объект String вызванный методом toString(), закрывается фрейм printAll. Удаляются переменныя i, переменные с ссылками о и ii, uselessVar на объекты Integer и String.  на стеке создается фрейм вызова метода System.out.println, выделяется строка в стринг пул записывается и передается ссылка в фрейм сразу на печать.
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5 в куче создается объект Integer, в метод pritnAll передается ссылка на объект в переменную iselessVar, объекту присваивается значение 700
        System.out.println(o.toString() + i + ii);  // 6 на стеке создается фрейм вызова метода System.out.println 
        //далее в переменную о фрейма System.out.println передается ссылка из переменной о фрейма printAll
        // далее в переменную i фрейма System.out.println копируется значение из переменной i фрейма printAll
        // далее в переменную ii фрейма System.out.println передается ссылка из переменной ii фрейма printAll
        // методом o.toString() в куче создается объект String, ссылка на который передается в качестве параметра в метод System.out.println
    }
}
