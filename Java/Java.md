### Annotations 注解

一种元数据（metadata，关于数据的组织、数据域及其关系的信息），不影响代码执行

#### 预定义注解

```java
@Deprecated           // 过时
@Override             // 重写
@SuppressWarnings     // 抑制警告
@FunctionalInterface  // 匿名函数或函数式接口
```

#### 自定义注解

```java
@interface ClassPreamble {
    String author();
    String date();
    int currentRevision() default 1;
    String lastModified() default "N/A";
    String lastModifiedBy() default "N/A";
    // Note use of array
    String[] reviewers();
}
```

### Reflection 反射

Java是静态语言（运行时结构不可变），反射（Reflection）允许程序在执行期间借助Reflection API取得任何类的内部信息，并直接操作对象内部属性和方法

相关主要API：

- `java.lang.Class`
- `java.lang.reflect.Method`
- `java.lang.reflect.Field`
- `java.lang.reflect.Constructor`

e.g.

```java
public class Test {
    static String testName = "0";
...
try{
            Class<?> c = Class.forName("org.examples.Test");
            Field field = c.getDeclaredField("testName");
            System.out.println(field.get(testName));
            field.set(testName, "1");
            System.out.println(field.get(testName));

            Method[] mc = c.getDeclaredMethods();
            for(Method m: mc){
                System.out.println(m.getName());
                System.out.println(m.getDeclaringClass());
            }
        }catch (ClassNotFoundException | NoSuchFieldException | IllegalAccessException e) {
            throw new RuntimeException(e);
        }
```

```bash
// out
0
1
setTestName
class org.javaparser.examples.Test
getTestName
class org.javaparser.examples.Test
main
class org.javaparser.examples.Test
```

See More at https://www.oracle.com/technical-resources/articles/java/javareflection.html and https://docs.oracle.com/javase/tutorial/reflect/TOC.html

### Servlet

Server Applet，服务端程序
