## Virtual threads deliver what they promise: making concurrency easy to use. 


**The change is minimal, instead of classic ThreadPool you can use newVirtualThreadPerTaskExecutor**. That's all!


Thanks to Java 19 is now possible to create high-throughput applications using a lightweight concurrency model (similar to coroutines in Kotlin). This would be accomplished via virtual threads.


Spring Boot already supports Java 19 but we have to be patient cause still in preview mode. 

## Demo Java

https://github.com/jorgetovar/java-loom-project

*Blocking task*
```java
    public Integer call() throws InterruptedException {
        Thread.sleep(1000);
        return number;
    }
```
*Process 1_000 blocking task*

```java
    public void process(String threadPoolType) throws InterruptedException, ExecutionException {

        try (ExecutorService executor = executorService) {
            List<Task> tasks = IntStream.range(0, 1_000)
                    .mapToObj(Task::new)
                    .collect(Collectors.toList());

            long time = System.currentTimeMillis();

            List<Future<Integer>> futures = executor.invokeAll(tasks);

            long sum = 0;
            for (Future<Integer> future : futures) {
                sum += future.get();
            }

            time = System.currentTimeMillis() - time;
            log.info("Result = {} ThreadPool sum: {} in {} ms", threadPoolType, sum, time);
            executor.shutdown();
        }
    }
```

*Process with Loom & Classic ThreadPool task**

```java
    @Override
    public void run(String... args) throws Exception {
        ExecutorService loom = Executors.newVirtualThreadPerTaskExecutor();
        ExecutorService classic = Executors.newFixedThreadPool(100);
        VirtualThreads virtualThreads = new VirtualThreads(classic);
        virtualThreads.process("classic");
        virtualThreads = new VirtualThreads(loom);
        virtualThreads.process("loom");
    }
```
*Output*
```shell
- : Started DemoApplication in 0.373 seconds (JVM running for 0.66)
- : Result = classic ThreadPool sum: 499500 in 10062 ms
- : Result = loom ThreadPool sum: 499500 in 1015 ms
```

## Demo Kotlin

*Coroutines*
```kotlin
    measureTimeMillis {
        runBlocking {
            repeat(1_000) { i ->
                launch {
                    call(i)
                }
            }
        }
    }.also {
        print("Result = Coroutines finished in $it ms")
    }

```
*Output*
```shell
Result = Coroutines finished in 1064 ms
```
## Conclusion

Project Loom is a game changer for Java. It will allow developers to create high concurrent applications without changing the paradigm or making big changes to the current code. 

