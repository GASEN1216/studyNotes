```xml
<dependency>
2    <groupId>com.github.javafaker</groupId>
3    <artifactId>javafaker</artifactId>
4    <version>最新版本号</version> <!-- 替换为当前最新的版本号 -->
5</dependency>
```

```java
import com.github.javafaker.Faker;
2
3public class Main {
4    public static void main(String[] args) {
5        Faker faker = new Faker();
6
7        // 生成一个英文名字
8        String name = faker.name().fullName();
9        System.out.println("Name: " + name);
10
11        // 生成一个地址
12        String address = faker.address().streetAddress();
13        System.out.println("Address: " + address);
14
15        // 生成一个Email地址
16        String email = faker.internet().emailAddress();
17        System.out.println("Email: " + email);
18
19        // 生成一个电话号码
20        String phoneNumber = faker.phoneNumber().cellPhone();
21        System.out.println("Phone Number: " + phoneNumber);
22    }
23}
```
