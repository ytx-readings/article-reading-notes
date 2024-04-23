# Understanding the `finalize()` method in Java

## What is _finalization_ and the `finalize()` method?

**Finalization** is a Java programming language feature that allows you to perform cleanup operations on objects that the garbage collector has found to be unreachable. It is used to reclaim native resources associated with an object.

The finalization action is done by the `finalize()` method, provided by the root `Object` class, which is called the **finalizer**. Finalizers are invoked when the JVM figures out that an object should be garbage-collected. The main purpose of a finalizer is to release resources used by the objects before they are removed from the memory. A finalizer can act as the primary cleanup mechanism, or as a backup mechanism in case other methods fail.

Note that, despite specifically treated by the JVM, a finalizer is just like any other instance method that may run _arbitrary actions_, including bring the object back to life or reachable again (e.g. from a static field); although this is not recommended, it is allowed by the Java programming language.

## What is _finalizable_, and when does the `finalize()` method get called?

## What should and should we not do in the `finalize()` method?

## What are the drawbacks of the `finalize()` method?

## Any alternatives to address these drawbacks?

## When do we _must_ use the `finalize()` method?

## References

* [[Baeldung] A Guide to the finalize Method in Java](https://www.baeldung.com/java-finalize)
* [[GeeksForGeeks] `finalize()` method in Java and How to Override it?](https://www.geeksforgeeks.org/finalize-method-in-java-and-how-to-override-it/)
* [[Oracle] How to Handle Java Finalization's Memory-Retention Issues](https://www.oracle.com/technical-resources/articles/javase/finalization.html)