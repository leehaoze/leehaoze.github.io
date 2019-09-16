---
title: Spring-Boot 实现时间戳参数与LocalDateTime的互转
date: 2019-09-16 16:11:13
tags:
- Spring
- Java
---
# 背景
数据库中包括实体类对于时间的储存使用的是LocalDateTime类型，而前端接受和返回的都是时间戳类型，所以需要实现在Spring Boot框架下时间戳与LocalDateTime的互转

# 实现方式
实现LocalDateTime的json序列化与反序列化类，通过注解使用

# 使用方式
在VO的属性中，加入序列化、反序列化的注解即可实现时间戳与LocalDateTime的互转
```
public class Test {
    @JsonDeserialize(using = LocalDateTimeDeserialize.class)
    @JsonSerialize(using = LocalDateTimeSerializer.class)
    private LocalDateTime lastOnlineTime;
}
```

# 具体实现
分别继承 `JsonDeserializer` 与 `JsonSerializer`即可
- LocalDateTimeDeserialize
```
public class LocalDateTimeDeserialize extends JsonDeserializer<LocalDateTime> {
    public final int MILLISECOND_TIMESTAMP_LENGTH = 13;

    public final int SECOND_TIMESTAMP_LENGTH  = 10;

    @Override
    public LocalDateTime deserialize(JsonParser jsonParser, DeserializationContext deserializationContext) throws IOException, JsonProcessingException {
        String text = jsonParser.getText().trim();
        long timestamp;
        if(text.length() == MILLISECOND_TIMESTAMP_LENGTH){
            timestamp = Long.parseLong(text) / 1000;
        }
        else{
            timestamp = Long.parseLong(text);
        }
        System.out.println(timestamp);

        return LocalDateTime.ofEpochSecond(timestamp,0, ZoneOffset.ofHours(8));
    }
}
```
- LocalDateTimeSerializer
```
public class LocalDateTimeSerializer extends JsonSerializer<LocalDateTime> {

    @Override
    public void serialize(LocalDateTime value, JsonGenerator gen, SerializerProvider serializers) throws IOException {
        gen.writeNumber(value.toInstant(ZoneOffset.of("+8")).toEpochMilli());
    }
}

```
