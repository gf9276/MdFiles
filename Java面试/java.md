# 1. java

# 2. 导包

记不住的话直接 `import java.util.*`

# 3. 读取命令行输入

```java
Scanner scan = new Scanner(System.in);

String s1 = sc.next(); // 以空格键、Tab键或Enter键等视为分隔符或结束符
String s2 =sc.nextLine(); // Enter作为分隔符

// nextInt() 读取整形

// nextDouble() 读取浮点数

```


# 4. Arrays

```java
Arrays.setAll(arr, (e) -> e + 5); // 没用到的话，那就是 _
Arrays.setAll(arr, _ -> e + 5); // 没用到的话，那就是 _
```