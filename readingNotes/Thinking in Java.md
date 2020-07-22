#初始化与清理

##this关键字

```
class Banana { void peel(int i) { /* ... */ }}

public class BananaPeel {
    public static void main(String[] args){
        Banana a = new Banana();
               b = new Banana();
        a.peel(1);
        b.peel(2);
    }
}
```

编译器做了一些幕后工作，它暗自把“所操作对象的引用” 作为第一个参数传递给peel()。所以，上述两个方法的调用变成了这样：

```
Banana.peel(a, 1);
Banana.peel(b, 2);
```

**this代表传入第一个参数a和b的对象的引用。**

###在构造器中调用构造器

可以通过this加参数列表在一个构造器中调用另一个构造器，但在同一个构造器里不能调用两次，且调用构造器的的代码必须放在最前面，否则编译会报错