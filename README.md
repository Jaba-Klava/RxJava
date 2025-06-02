# Курсовая работа: Реализация RxJava-подобной библиотеки

## Описание проекта
Реализация упрощенной версии библиотеки реактивного программирования, аналогичной RxJava, включая:
- Базовые компоненты Observable/Observer
- Операторы преобразования данных
- Управление потоками через Schedulers
- Обработку ошибок и отмену подписок

## Реализованные компоненты

### 1. Базовые компоненты
```java
public interface Observer<T> {
    void onNext(T item);
    void onError(Throwable t);
    void onComplete();
}

public class Observable<T> {
    public static <T> Observable<T> create(ObservableOnSubscribe<T> source) { ... }
    public void subscribe(Observer<? super T> observer) { ... }
}
```
*Соответствие требованиям:* Полная реализация паттерна "Наблюдатель" с поддержкой создания потоков.

### 2. Операторы преобразования

| Оператор | Реализация	                                                                 |Тесты |
|----------|-----------------------------------------------------------------------------|------|
| map      | public final <R> Observable<R> map(Function<? super T, ? extends R> mapper) |   ✅   |
| filter   | public final Observable<T> filter(Predicate<? super T> predicate)           |   ✅   |
| flatMap  | flatMap	public final <R> Observable<R> flatMap(Function<T, Observable<R>> mapper)   |   ✅    |

*Пример использования:*
```java
Observable.range(1, 10)
.filter(x -> x % 2 == 0)
.map(x -> x * 10)
.subscribe(...);
```

### 3. Управление потоками

Реализованные Schedulers:

**IOThreadScheduler** (CachedThreadPool)
**ComputationScheduler** (FixedThreadPool)
**SingleThreadScheduler** (SingleThreadExecutor)

Методы управления:

```java
public final Observable<T> subscribeOn(Scheduler scheduler) { ... }
public final Observable<T> observeOn(Scheduler scheduler) { ... }
```
### 4. Дополнительные функции

* Disposable интерфейс для отмены подписок
* Полноценная обработка ошибок через onError
* Цепочка операторов с поддержкой backpressure

## Тестирование

Реализованы юнит-тесты для:

1. Базовых сценариев подписки
2. Комбинации операторов (map + filter + flatMap)
3. Многопоточных сценариев
4. Обработки ошибок

```java
@Test
public void testMapOperator() {
    Observable.just(1, 2, 3)
        .map(x -> x * 2)
        .test()
        .assertValues(2, 4, 6);
}
```

## Архитектура системы

```java
classDiagram
    class Observable {
        +subscribe()
        +map()
        +filter()
        +flatMap()
    }
    class Observer {
        <<interface>>
        +onNext()
        +onError()
        +onComplete()
    }
    class Scheduler {
        <<interface>>
        +execute()
    }
    Observable --> Observer
    Observable --> Scheduler
```

## Как использовать

1. Клонировать репозиторий:

```bash
git clone https://github.com/DanisNorbu/RxJava_HW.git
```
2. Собрать проект:

```bash
./gradlew build
```
3. Запустить тесты:

```bash
./gradlew test
```
## Отчетные материалы

* Полное описание архитектуры в директории **/docs**
* Примеры использования в **/examples**
* Отчет по тестированию в **TESTING.md**

