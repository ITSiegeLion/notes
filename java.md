#### 1.属性映射

[MapStruct](https://mapstruct.org/)   采用原生的方法调用，因此更快速，更安全也更容易理解

```xml
<properties>
    <mapstruct.version>1.2.0.Final</mapstruct.version>
</properties>

<dependencies>
    <dependency>
      <groupId>org.mapstruct</groupId>
      <artifactId>mapstruct-jdk8</artifactId>
      <version>${mapstruct.version}</version>
    </dependency>
    <dependency>
      <groupId>org.mapstruct</groupId>
      <artifactId>mapstruct-processor</artifactId>
      <version>${mapstruct.version}</version>
    </dependency>
</dependencies>
```



```java
@Mapper
public interface PersonConverter {
    PersonConverter INSTANCE = Mappers.getMapper(PersonConverter.class);
    @Mappings({
        @Mapping(source = "birthday", target = "birth")//对象属性不一致时映射,
        @Mapping(source = "birthday", target = "birthDateFormat", dateFormat = "yyyy-MM-dd HH:mm:ss"),
        @Mapping(target = "birthExpressionFormat", expression = "java(org.apache.commons.lang3.time.DateFormatUtils.format(person.getBirthday(),\"yyyy-MM-dd HH:mm:ss\"))"),
        @Mapping(source = "user.age", target = "age"),
        @Mapping(target = "email", ignore = true)
    })
    PersonDTO domain2dto(Person person);

    List<PersonDTO> domain2dto(List<Person> people);
}
```

编译后会自动生成转换接口的实现类，使用如下：

```java
PersonDto dto =PersonConverter.INSTANCE.domain2dto(Person domain)
```

#### 2.

