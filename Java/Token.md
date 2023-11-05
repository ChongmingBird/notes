# 【Token】

> Token(令牌)是服务端生成的一串字符串，以作客户端进行请求的一个令牌。
>
> 第一次登录时，服务器生成一个Token，将此Token返回给客户端，以后客户端只需带上这个Token前来请求数据即可，无需再次带上用户名和密码。
>
> **作用机制：**
>
> ​	服务器并不保存Token，而是通过数据签名的方法，对数据用算法（如SHA-256）与私钥进行签名后作为Token。
>
> ​	当Token发送给服务器时，服务会通过相同的算法与密钥进行签名，如果和Token中的签名相同，服务器就知道用户已经登录过了，并且可以直接得到用户的userID。（服务器用cpu的计算时间来换取了储存空间）

# 后端操作

## Token生成

```java
/**
 * @author Chongming
 * @description 用于生成Token:TOKEN+ID
 * @date 2022年05月17日 10:58
 */
public class TokenUtil {

    // 0对应大写
    private final static int RAND_UPPER = 0;
    // 1对应小写
    private final static int RAND_LOWER = 1;
    // 随机数范围
    private final static int RAND_RANGE = 2;

    // Token的前缀
    private final static String TOKEN_PREFIX = "ANSWER";

    private static Random random = new Random();

    public static String getToken(){
        // 返回一个UUID的字符串，将这个字符串的"-"替换成空字符，再把所有的大写字母转成小写字母
        String uuid = UUID.randomUUID().toString().replace("-","").toLowerCase();
        StringBuilder stringBuilder = new StringBuilder();
        for (int i = 0; i < uuid.length(); i++) {
            // 生成一个随机数大小介于0~2之间，又因为是整数，所以只会是0、1两种，0对应大写，1对应小写
            int rand = random.nextInt(RAND_RANGE);
            if (rand == RAND_UPPER) {
                stringBuilder.append(String.valueOf(uuid.charAt(i)).toUpperCase());
            } else if (rand == RAND_LOWER) {
                stringBuilder.append(String.valueOf(uuid.charAt(i)).toLowerCase());
            }
        }
        // 返回添加了前缀的Token
        return TOKEN_PREFIX + stringBuilder.toString();
    }
}
```

## Token存储

**把得到的Token令牌存入Redis数据库**

```java
// 得到user对象
User user = userService.getUserByLogin(userName, password);
// 生成Token令牌
String token = TokenUtil.getToken();
// 存入Redis数据库
StringRedisTemplate redisTemplate = new StringRedisTemplate();
/**
 * key:token令牌 
 * value: user对象(转换为JSON)
 */
redisTemplate.opsForValue().set(token, JsonUtil.toJson(user), Duration.ofMinutes(30L));
```

**使用Jackson序列化对象：**

```java
public static String toJson(Object o) {
    ObjectMapper objectMapper = new ObjectMapper();
    return o instanceof String ? o.toString() : objectMapper.writeValueAsString(o);
}
```

## 获取登录用户

**从Redis中获取登录用户：**

```java
// 获取请求头中的token
String token = request.getHeader(key);
// 从redis中获取用户的JSON数据
String userJson = redisTemplate.opsForValue().get(token);
// 判断user是否存在
if(userJson != null) {...}
// 反序列化对象
User user = JsonUtil.toObject(userJson, User.class);
```

**使用Jackson反序列化对象**

```java
public static<T> T toObject(String json, Class<T> clazz) {
    ObjectMapper objectMapper = new ObjectMapper();
    return clazz.equals(String.class) ? (T) json : objectMapper.readValue(json, clazz);
}
```

## 登录拦截器

**通过登录拦截器，拦截未传递或已失效token的请求**

```java

```

