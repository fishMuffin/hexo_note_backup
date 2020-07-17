---

title: jdk8特性:Stream流操作
date: 2020-06-27 18:04:46
tags: [java,JDK8,lambda]


---





```java
package demo2019_11_23;

import demo2019_11_20.DemoVo;
import org.junit.Before;
import org.junit.Test;

import java.util.*;
import java.util.function.Predicate;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class StreamDemo {
    private List<DemoVo> list;
    private List<String> strList;
    private List<Integer> emptyList;

    @Before
    public void init() {
        list = new ArrayList<>();
        list.add(new DemoVo(1, "劳务费", "税前收入", "李1"));
        list.add(new DemoVo(9, "稿费", "税后收入", "李2"));
        list.add(new DemoVo(3, "劳务费", "税前收入", "张3"));
        list.add(new DemoVo(4, "稿费", "税后收入", "李4"));
        list.add(new DemoVo(5, "稿费", "税后收入", "李5"));

        strList = Arrays.asList("java", "python", "java", "python", "go", "js");
        emptyList = new ArrayList<>();
    }


    @Test
    public void StreamConcatTest(){
        //合并流
        Stream<Double> limit = Stream.generate(Math::random).limit(10);
        Stream<Integer> limit1 = Stream.iterate(0, n -> n + 1).limit(10);
        Stream<? extends Number> concat = Stream.concat(limit, limit1);
        concat.forEach(System.out::println);
    }

    @Test
    public void StreamEmptyTest(){
        /**
         * 创建一个空的流
         */
        Stream<Object> empty = Stream.empty();
    }

    @Test
    public void StreamIterateTest(){
        /**
         * 通过Stream.iterate 迭代的去创建流，同上也是无限流，
         * 不同的地方在于这个函数有2个参数，第一个参数是基值，第二个参数是基于第一个参数的运算函数
         */
        Stream.iterate(0,n->n+1).limit(10).forEach(System.out::println);
    }

    @Test
    public void StreamGenerateTest() {
        /**
         * 通过Stream.generate 静态方法去创建一个流。
         * 这个特别需要注意，这个方法是创建无限流，如果不在调用这个函数后调用limit方法去限制流的长度，则会无限创建下去
         */
        Stream.generate(Math::random).forEach(System.out::println);
    }

    @Test
    public void StreamBuilderTest() {
        Stream.Builder<Object> builder = Stream.builder();
        builder.accept(1);
        builder.add(2);
        builder.build().forEach(System.out::println);
    }

    @Test
    public void StreamOfTest() {
        Stream<Integer> integerStream = Stream.of(1, 2, 3, 4);
        Stream<DemoVo> demoVoStream = Stream.of(new DemoVo(10, "稿费", "税后收入", "张四"));
        integerStream.forEach(s -> System.out.println(s));
        demoVoStream.forEach(System.out::println);
    }

    @Test
    public void StreamNoneMatchTest() {
        boolean b = list.stream().noneMatch(new Predicate<DemoVo>() {
            @Override
            public boolean test(DemoVo demoVo) {
                return "张三".equals(demoVo.getName());
            }
        });
        System.out.println(b);
    }

    @Test
    public void StreamSortedTest() {
        List<DemoVo> collect = list.stream().sorted(Comparator.comparing(DemoVo::getId)).collect(Collectors.toList());
        collect.forEach(s -> System.out.println(s));
    }


    @Test
    public void StreamSkipTest() {
        list.stream().skip(1).limit(2).forEach(System.out::println);
    }


    @Test
    public void StreamReduceTest() {
        List<Integer> integers = Arrays.asList(1, 2, 3, 4);
        //求元素和
        Integer reduce = list.stream().map(DemoVo::getId).reduce(0, Integer::sum);
        System.out.println(reduce);
        integers.stream().reduce((x, y) -> x + y).ifPresent(System.out::println);
        //求最大值
        integers.stream().reduce(Integer::max).ifPresent(System.out::println);
        //求最小值
        integers.stream().reduce(Integer::min).ifPresent(System.out::println);
        integers.stream().reduce((i, j) -> i > j ? j : i).ifPresent(System.out::println);
        //取出偶数求乘积
        Integer reduce1 = integers.stream().filter(i -> i % 2 == 0).reduce(1, (i, j) -> i * j);
        System.out.println(reduce1);
    }

    @Test
    public void StreamLimitTest() {
        list.stream().limit(2).forEach(p -> System.out.println(p));
    }


    @Test
    public void StreamForEachOrderedTest() {
        /**
         * 其实两者完成的功能类似，主要区别在并行处理上，forEach是并行处理的，
         * forEachOrder是按顺序处理的，显然前者速度更快。
         */
        list.stream().forEachOrdered(p -> System.out.println(p));
    }

    @Test
    public void StreamFLatMapToIntTest() {
        List<List<String>> listOfLists = Arrays.asList(
                Arrays.asList("1", "2"),
                Arrays.asList("5", "6"),
                Arrays.asList("3", "4")
        );
        IntStream intStream = listOfLists.stream().flatMapToInt(child -> child.stream().mapToInt(Integer::new));
        int sum = intStream.peek(System.out::println).sum();
        System.out.println(sum);
//        IntStream intStream = list.stream().flatMapToInt(p -> list.stream().mapToInt(DemoVo::getId));
//        int sum = intStream.peek(System.out::println).sum();
    }

    @Test
    public void StreamFlatMapTest() {
        /**
         * FlatMap主要是用于stream合并，
         * 这个功能非常实用，他是默认实现多CPU并行执行的，
         * 所以我们合并集合优先实用这种方式。
         */
        List<?> collect = Stream.of(list, strList).flatMap(p -> p.subList(0, 1).stream()).collect(Collectors.toList());
        System.out.println(collect);
    }


    @Test
    public void StreamFindFirstTest() {
        DemoVo demoVo = list.stream().findFirst().get();
        System.out.println(demoVo);
    }

    @Test
    public void StreamFindAnyTest() {
        DemoVo demoVo = list.stream().findAny().get();
        System.out.println(demoVo);
        System.out.println(emptyList.stream().findAny().get());
    }

    @Test
    public void StreamDistinctTest() {
        List<String> collect = strList.stream().distinct().collect(Collectors.toList());
        System.out.println(collect);
    }

    @Test
    public void StreamAnyMatchTest() {
        boolean b = list.stream().anyMatch(new Predicate<DemoVo>() {
            @Override
            public boolean test(DemoVo demoVo) {
                return "张三".equals(demoVo.getName());
            }
        });
        System.out.println(b);
    }

    @Test
    public void StreamAllMatchTest() {
        boolean b = list.stream().allMatch(new Predicate<DemoVo>() {
            @Override
            public boolean test(DemoVo demoVo) {
                return "李四".equals(demoVo.getName());
            }
        });
        System.out.println(b);
    }

    @Test
    public void StreamPeekTest() {
        /**
         * 除去用于调试，peek()在需要修改元素内部状态的场景也非常有用，
         * 比如我们想将所有Person的名字修改为大写，
         * 当然也可以使用map()和flatMap实现，但是相比来说peek()更加方便，
         * 因为我们并不想替代流中的数据。
         */
        //将list集合中的所有对象id取出来加一再设置回去
        list.stream().peek(p -> p.setId(p.getId() + 1)).forEach(System.out::println);
    }

    @Test
    public void StreamCountTest() {
        long count = list.stream().count();
        System.out.println(count);
    }

    @Test
    public void StreamMinTest() {
        Optional<String> min = list.stream().map(DemoVo::getIncomeType).min(Comparator.comparing(String::length));
        System.out.println(min.get());
    }

    @Test
    public void StreamMaxTest() {
        Optional<DemoVo> max = list.stream().max(Comparator.comparing(DemoVo::getId));
        System.out.println(max.get());
    }

    @Test
    public void StreamFilterDemo() {
        List<DemoVo> collect = list.stream().filter(new Predicate<DemoVo>() {
            @Override
            public boolean test(DemoVo demoVo) {
                return "劳务费".equals(demoVo.getIncomeType());
            }
        }).collect(Collectors.toList());
        collect.stream().forEach(s -> System.out.println(s));
    }


}
```