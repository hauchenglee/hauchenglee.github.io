---
layout: post
title: Java 8 - Stream Pipelines 流管道
category: it
tags: [java]
---

## Pipelines

流的構成：

![](https://hauchenglee.github.io/assets/images/it/java/java8-stream--pipeline-ibm.png)

Ref: [Java 8 中的 Streams API 详解](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/index.html){:target="_blank"}

<br>

> To perform a sequence of operations over the elements of the data source and aggregate 
> their results, three parts are needed - the **source**, **intermediate operation(s)** and a **terminal operation**.
>
> Ref: [The Java 8 Stream API Tutorial - Baeldung](https://www.baeldung.com/java-8-streams){:target="_blank"}

要對數據源的元素執行一系列操作並彙總其結果，需要三部分：源數據、中間操作、終端操作。

中間操作和終端操作：
- 中間操作會再次返回一個流，所以，我們可以鏈接多個中間操作。例如，圖中的`filter`過濾，`map`對象轉換，`sort`進行排序，就屬於中間操作。
- 終端操作是對流操作的一個結束動作，一般返回`void`或者一個非流的結果。圖中的`foreach`循環就是一個終止操作。

![](https://hauchenglee.github.io/assets/images/it/java/java8-stream-pipelines-desc.png)

Ref: [[译] 一文带你玩转 Java8 Stream 流，从此操作集合 So Easy - 掘金](https://juejin.im/post/5cc124a95188252d891d00f2){:target="_blank"}

<br>

## JDK

下面，我們再看接口定義：

```java
public interface Stream<T> extends BaseStream<T, Stream<T>> {
 
	Stream<T> filter(Predicate<? super T> predicate);
 
	<R> Stream<R> map(Function<? super T, ? extends R> mapper);
 
	IntStream mapToInt(ToIntFunction<? super T> mapper);
 
	LongStream mapToLong(ToLongFunction<? super T> mapper);
 
	DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper);
 
	<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
 
	IntStream flatMapToInt(Function<? super T, ? extends IntStream> mapper);
 
	LongStream flatMapToLong(Function<? super T, ? extends LongStream> mapper);
 
	DoubleStream flatMapToDouble(Function<? super T, ? extends DoubleStream> mapper);
 
	Stream<T> distinct();
 
	Stream<T> sorted();
 
	Stream<T> sorted(Comparator<? super T> comparator);
 
	Stream<T> peek(Consumer<? super T> action);
 
	Stream<T> limit(long maxSize);
 
	Stream<T> skip(long n);
 
	void forEach(Consumer<? super T> action);
 
	void forEachOrdered(Consumer<? super T> action);
 
	Object[] toArray();
 
	<A> A[] toArray(IntFunction<A[]> generator);
 
	T reduce(T identity, BinaryOperator<T> accumulator);
 
	Optional<T> reduce(BinaryOperator<T> accumulator);
 
	<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
 
	<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner);
 
	<R, A> R collect(Collector<? super T, A, R> collector);
 
	Optional<T> min(Comparator<? super T> comparator);
 
	Optional<T> max(Comparator<? super T> comparator);
 
	long count();
 
	boolean anyMatch(Predicate<? super T> predicate);
 
	boolean allMatch(Predicate<? super T> predicate);
 
	boolean noneMatch(Predicate<? super T> predicate);
 
	Optional<T> findFirst();
 
	Optional<T> findAny();
 
	public static <T> Builder<T> builder() {
		return new Streams.StreamBuilderImpl<>();
	}
 
	public static <T> Stream<T> empty() {
		return StreamSupport.stream(Spliterators.<T> emptySpliterator(), false);
	}
 
	public static <T> Stream<T> of(T t) {
		return StreamSupport.stream(new Streams.StreamBuilderImpl<>(t), false);
	}
 
	@SafeVarargs
	@SuppressWarnings("varargs") // Creating a stream from an array is safe
	public static <T> Stream<T> of(T... values) {
		return Arrays.stream(values);
	}
 
	public static <T> Stream<T> iterate(final T seed, final UnaryOperator<T> f) {
		Objects.requireNonNull(f);
		final Iterator<T> iterator = new Iterator<T>() {
			@SuppressWarnings("unchecked")
			T t = (T) Streams.NONE;
 
			@Override
			public boolean hasNext() {
				return true;
			}
 
			@Override
			public T next() {
				return t = (t == Streams.NONE) ? seed : f.apply(t);
			}
		};
		return StreamSupport.stream(
				Spliterators.spliteratorUnknownSize(iterator, Spliterator.ORDERED | Spliterator.IMMUTABLE), false);
	}
 
	public static <T> Stream<T> generate(Supplier<T> s) {
		Objects.requireNonNull(s);
		return StreamSupport.stream(new StreamSpliterators.InfiniteSupplyingSpliterator.OfRef<>(Long.MAX_VALUE, s),
				false);
	}
 
	public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b) {
		Objects.requireNonNull(a);
		Objects.requireNonNull(b);
 
		@SuppressWarnings("unchecked")
		Spliterator<T> split = new Streams.ConcatSpliterator.OfRef<>((Spliterator<T>) a.spliterator(),
				(Spliterator<T>) b.spliterator());
		Stream<T> stream = StreamSupport.stream(split, a.isParallel() || b.isParallel());
		return stream.onClose(Streams.composedClose(a, b));
	}
 
	public interface Builder<T> extends Consumer<T> {
		@Override
		void accept(T t);
 
		default Builder<T> add(T t) {
			accept(t);
			return this;
		}
 
		Stream<T> build();
 
	}
}
```

通過抽象方法的定義，看到這個方法可以分為兩種類型，一種返回類型接口本身的`Stream<T>`，另外一種是返回其他對象類型的。
- 返回接口類型的，稱為中間操作
- 返回其他具體類型的，稱為終端操作

下面附一張具體的分類的圖，來自《java8实战》的第五章：

![](https://hauchenglee.github.io/assets/images/it/java/java8-stream-pipelines-interface-table.png)

> ————————————————
>
> 版权声明：本文为CSDN博主「葵花下的獾」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
>
> 原文链接：[https://blog.csdn.net/qq_28410283/article/details/80634725](https://blog.csdn.net/qq_28410283/article/details/80634725){:target="_blank"}

## Reference

- [Java 8 中的 Streams API 详解](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/index.html){:target="_blank"}

---