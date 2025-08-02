# MySQL

[JavaGuide-MySQL 面试](https://javaguide.cn/database/mysql/mysql-questions-01.html#mysql-%E5%9F%BA%E7%A1%80)

## Q1：varchar 的大小分配

1. 在实际使用中，我在创建表时设置了 `varchar(100)`，在编写业务代码使用时，发现插入数据错误，出现了对字符串大小评估不准确的问题。
2. 是按照字符计数，并不是按照字节计数，所以存放一个中文和英文的个数所消耗的字符个数是相同的。

## Q2：关于 Decimal 类型和 float/double 类型

1. `Decimal`类型存放的是定点数（存储精确的小数值），`float/double`是浮点数（只能存放近似值）。
2. Java 中如何使用数据库中对应的 Decimal 呢？
   ```java
   BigDecimal a = new BigDecimal("0.1"); // 精确
   // 方法
   a.add(b);
   a.subtract(b);
   a.multiply(b);
   BigDecimal x = new BigDecimal("10");
   BigDecimal y = new BigDecimal("3");
   // 必须指定舍入模式，否则可能抛出 ArithmeticException（除不尽时）
   BigDecimal result = x.divide(y, 4, RoundingMode.HALF_UP); // 10 ÷ 3 ≈ 3.3333
   ```

## Q3：不推荐使用 text 和 blob 类型，用什么方法代替呢？

1. 使用 OSS 云存储的方式，在数据库中只存放文件的 url。
