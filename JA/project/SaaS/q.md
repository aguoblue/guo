

## 未解决
放入回收站后的每日统计
放入回收站后的访问失效



## 分库分表用户id就不能用int，而是要用bigint
用雪花算法生成用户id，雪花算法一般是64个二进制位(有效位63个)，而在mysql中，int只占4个字节存不下，bigint占8个字节可以存下。



## 海量用户查询用户名
1 查数据库
2 redis 永久字符串
3 redis 布隆过滤器


## 短链接怎么生成的


# 其他

### **Lombok问题**

如果你使用`@RequiredArgsConstructor`但是字段未声明为`final`，Lombok不会为这些字段生成构造函数，导致Spring无法通过构造函数注入实例。

- **解决办法**：确保使用`@RequiredArgsConstructor`时，需要注入的依赖声明为`private final`。

## BeanUtils.copyProperties
在使用BeanUtils.copyProperties时，如果复制的源对象为null，那么抛异常
java.lang.IllegalArgumentException: Source must not be null


## 在mysql workbench里插入和在程序里插入导致布隆过滤器和数据库数据不一致









```sql
CREATE TABLE `t_link` (   `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',   `domain` varchar(128) DEFAULT NULL COMMENT '域名',   `short_uri` varchar(8) DEFAULT NULL COMMENT '短链接',   `full_short_url` varchar(128) DEFAULT NULL COMMENT '完整短链接',   `origin_url` varchar(1024) DEFAULT NULL COMMENT '原始链接',     `click_num` int(11) DEFAULT 0 COMMENT '点击量',     `gid` varchar(32) DEFAULT NULL COMMENT '分组标识',     `enable_status` tinyint(1) DEFAULT NULL COMMENT '启用标识 0：未启用 1：已启用',     `created_type` tinyint(1) DEFAULT NULL COMMENT '创建类型 0：控制台 1：接口',     `valid_date_type` tinyint(1) DEFAULT NULL COMMENT '有效期类型 0：永久有效 1：用户自定义',     `valid_date` datetime DEFAULT NULL COMMENT '有效期',     `describe` varchar(1024) DEFAULT NULL COMMENT '描述',   `create_time` datetime DEFAULT NULL COMMENT '创建时间',   `update_time` datetime DEFAULT NULL COMMENT '修改时间',   `del_flag` tinyint(1) DEFAULT NULL COMMENT '删除标识 0：未删除 1：已删除',   PRIMARY KEY (`id`),   UNIQUE KEY `idx_unique_full_short_url` (`full_short_url`) USING BTREE ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```
- `id`: 主键，自动递增的长整型（bigint）字段，用于唯一标识每条记录。每条记录的ID都是唯一的，用于区分表中的每一行。
- `domain`: 可变长度字符串（varchar），最大长度为128个字符，存储短链接所使用的域名。
- `short_uri`: 可变长度字符串，最大长度为8个字符，存储生成的短链接的路径部分。
- `full_short_url`: 可变长度字符串，最大长度为128个字符，存储完整的短链接，包括域名和路径。
- `origin_url`: 可变长度字符串，最大长度为1024个字符，存储原始的长链接。
- `click_num`: 整型（int），默认值为0，用于记录链接被点击的次数。
- `gid`: 可变长度字符串，最大长度为32个字符，表示链接的分组标识，用于将链接分类或分组。
- `enable_status`: 小整型（tinyint），表示链接是否启用，0表示未启用，1表示已启用。
- `created_type`: 小整型，表示链接的创建方式，0代表通过控制台创建，1代表通过API接口创建。
- `valid_date_type`: 小整型，表示链接的有效期类型，0表示永久有效，1表示用户自定义有效期。
- `valid_date`: 日期时间类型（datetime），存储链接的有效截止日期。
- `describe`: 可变长度字符串，最大长度为1024个字符，用于存储链接的描述信息。
- `create_time`: 日期时间类型，存储记录的创建时间。
- `update_time`: 日期时间类型，存储记录的最后修改时间。
- `del_flag`: 小整型，表示记录的删除状态，0表示未删除，1表示已删除，用于实现逻辑删除。

此外，表定义中还包含了一个唯一键 `idx_unique_full_short_url`，它保证了 `full_short_url` 字段的值在整个表中是唯一的，避免生成重复的短链接。

整个表设计考虑了短链接服务的基本需求，包括链接的管理、分类、状态控制、访问统计和有效期处理，同时通过逻辑删除实现了数据的安全保留。
### 保留 `full_short_url` 的理由：

1. **直接访问**：如果系统中的短链接会在不同的域名下使用，`full_short_url` 可以直接提供完整的URL，便于快速访问和分享。用户或系统无需手动拼接域名和短链接标识符（`short_uri`）。
    
2. **唯一性验证**：该字段作为一个唯一键（`UNIQUE KEY`）确保了每个短链接的完整URL是唯一的，避免了不同域名下相同路径的冲突问题。
    
3. **性能优化**：在某些查询场景下，直接通过完整的短链接进行查找可能比拼接域名和路径更高效。
    

### 不保留 `full_short_url` 的理由：

1. **冗余数据**：如果所有的短链接都使用同一个域名，那么 `domain` 和 `short_uri` 字段已足够重构出完整的URL，`full_short_url` 可能会造成数据的冗余。
    
2. **灵活性降低**：将完整的短链接存储为一个字段，在需要更改域名时会更加麻烦，因为需要更新数据库中每条记录的 `full_short_url` 字段。
    
3. **存储空间**：虽然现代数据库系统的存储效率很高，但是在极大规模的数据集中，减少冗余数据可以节省存储空间，尤其是当每个字节的存储成本都计算在内时。
    

综上所述，是否需要 `full_short_url` 字段应根据短链接服务的具体设计目标和使用场景来决定。如果你的服务需要在多个域名下操作，或者强调直接通过数据库记录快速访问短链接的方便性，那么保留这个字段可能是有益的。反之，如果所有链接都在同一域名下，为了数据库的简洁和灵活性，可以考虑不使用 `full_short_url` 字段。


## 在短链接创建时的布隆过滤器情况
存在一种情况，短链接入库成功，但是并没有添加到布隆过滤器中（可能因为进程挂掉等等原因，由于没加事务，短链接入库不会回滚）。也就是说实际上入库了，但布隆过滤器显示短链不存在，此时再次插入该短链不就越过布隆过滤器，然后被唯一索引给拦截了。 因为这种情况出现的概率极低，所以把唯一索引称为兜底策略

1.布隆过滤器判断不存在，就一定不存在，这个不会误判；判断存在，实际有可能不存在，这个会误判。
2.generateSuffix若正常返回shortUri，一定是不存在缓存里的(如果重复10次就抛异常了)。然而shortUri有可能在数据库里(出了数据库、缓存不一致bug)，因此数据库里有必要引入唯一索引，这个是兜底机制。所以createShortLink里面try, catch其实就是加个保险



同一个原始链接，不同的用户，只有一个人能创建对应的短链接？？？
在generateSuffix方法中，给originUrl字符串添加了时间导致每个originUrl都不会相同，包括同一个originUrl，那么同一个originUrl可以创建多次短链接，生成的shortUri都不相同，一会就会把布隆过滤器撑爆。？？？
## 缓存击穿
缓存击穿是指当缓存中没有某个数据时，因为这个数据非常热门，同时有大量的请求这个数据，这些请求都会穿过缓存去查询数据库，导致数据库压力突然增大，有可能会造成数据库短时间内崩溃。缓存击穿的典型情况是对于某个特定的key，它在缓存中不存在（可能是因为没缓存过、缓存过期了），而查询这个key的请求非常频繁。

使用Redis作为缓存系统来举例，解决缓存击穿问题的策略有以下几种：

### 1. 设置热点数据永不过期

对于那些访问非常频繁的热点数据，可以设置它们在缓存中永不过期。这样就可以保证这部分数据总是可以在缓存中被命中。但这种方法需要业务上有一个很好的机制来保证这些数据是最新的，否则可能会出现缓存数据过时的问题。

### 2. 使用互斥锁

当缓存中没有命中数据时，不是所有查询这个数据的请求都去数据库查询，而是使用某种机制（例如分布式锁）确保只有一个请求去数据库中查询数据并回填到缓存中，其他的请求则等待或者稍后重试。

pythonCopy code

```
if (cache.get(key) == null) {     
	if (获取锁成功) {         
		从数据库中加载数据并回填到缓存;         
		释放锁;     
	} else {         
	等待一段时间后重试;     
	}
}
```
`
### 3. 设置缓存降级

在极端情况下，如果数据库真的因为压力过大而无法提供服务，可以临时采取缓存降级策略，比如直接返回一些默认值或者提示信息，避免数据库被进一步压垮。

### 4. 使用布隆过滤器

在请求查询某个key之前，先用布隆过滤器检查这个key是否可能存在。布隆过滤器可以用来判断一个元素是否一定不存在或者可能存在。如果布隆过滤器判断这个key一定不存在，那么就不需要查询数据库，直接返回即可。这样可以进一步减少数据库的压力。不过布隆过滤器有一定的误判率，需要根据实际情况调整。

通过这些策略的合理使用，可以有效地缓解缓存击穿问题，保证系统的稳定性。


双重检查锁定（Double-Checked Locking）策略主要用于减少在单例模式或懒加载对象创建中的同步开销，它通过在同步代码之外提供一个初步检查来避免不必要的同步。理论上，这种模式可以用于缓存击穿问题的解决方案中，特别是在处理高并发环境下对热点数据的访问时。

缓存击穿是指大量并发请求查询同一个key时，该key在缓存中不存在（可能因为过期或从未被缓存），导致所有的请求都直接落到数据库上，可能会造成数据库的压力骤增。如果使用双重检查锁定策略，可以在一定程度上减缓这种压力

如果缓存中没有找到数据，我们不是直接去数据库加载数据，而是先进行一次加锁检查。这种方法可以确保即使在高并发的场景下，对于同一个key的数据库访问也只会发生一次，因为在锁内部会再次检查缓存是否已经被更新。

尽管双重检查锁定可以在一定程度上减少缓存击穿问题造成的数据库压力，但它不是万能的。在处理高并发的缓存系统时，最佳实践通常是结合使用多种策略，包括设置合理的缓存过期策略、使用布隆过滤器预判、以及合理利用分布式锁等。



## 缓存穿透
缓存穿透是指查询一个在数据库中不存在的数据，由于缓存系统通常只缓存存在的数据，这种不存在的数据的查询请求就会穿过缓存，直接打到数据库上。如果有大量这样的查询，数据库会面临巨大压力，甚至可能导致数据库服务崩溃。

### 缓存穿透问题示例

假设一个应用有一个获取用户信息的接口，用户信息被缓存在Redis中。正常情况下，用户发起请求获取自己的信息时，系统会先在Redis缓存中查询；如果缓存中有数据，则直接返回，不需要查询数据库。但如果某个攻击者不断请求不存在的用户信息，由于这些用户信息既不在缓存中，也不在数据库中，每次请求都会穿过缓存查询数据库，造成不必要的数据库负载。

### 缓存穿透的解决方案

1. **空对象缓存**：当查询数据库未找到数据时，依然将这个“空”的结果缓存起来，并设置一个较短的过期时间。这样，对同一个不存在的数据的查询就能在后续一段时间内直接由缓存来应答，避免对数据库的查询。需要注意的是，这种方法可能会导致缓存被大量不存在的数据填满，因此要设置合理的过期时间。
    
2. **布隆过滤器**：布隆过滤器是一种数据结构，可以用来检测一个元素是否在一个集合中。将所有可能查询的数据的标识（比如用户ID）提前加载到一个布隆过滤器中，查询请求首先通过布隆过滤器进行检查，如果布隆过滤器判断数据不可能存在，那么就直接返回，不再查询数据库或缓存。布隆过滤器有一定的误判率，但不会漏判，适用于在误判率可接受的前提下减少数据库的查询压力。
    
3. **接口校验**：在应用层面增加校验逻辑，如请求参数的校验，不合法或者明显不可能存在的请求直接拦截，不进行缓存或数据库的查询。
    
4. **限流与监控**：通过设置合理的限流策略来防止恶意请求达到数据库层，同时通过实时监控来发现异常流量并采取措施。
    
5. **使用锁**：使用锁来避免多个相同请求同时访问数据库，只让一个请求去加载数据，可以解决数据库压力过大问题，但是用户等待锁，等待时间过长，不推荐



## 用户上下文

使用了 Alibaba 的 `TransmittableThreadLocal`，它是 `ThreadLocal` 的一个改进版，更适合用在线程池等异步执行环境中。这种机制允许每个线程访问自己专有的、线程局部（thread-local）变量。具体到你的例子，用户上下文信息（如用户ID、用户名和用户真实姓名等）被存储在每个线程独有的空间里，这样即便是在多线程环境中，每个线程看到的只是它自己设置的用户上下文信息，从而避免了数据的并发访问问题。


### 工作流程如下：

1. **设置用户至上下文（`setUser(UserInfoDTO user)`）**：
    
    - 当用户登录后，用户的信息会被包装成一个 `UserInfoDTO` 对象，并通过 `setUser` 方法保存到 `USER_THREAD_LOCAL` 中。这里的 `USER_THREAD_LOCAL` 是通过 `TransmittableThreadLocal` 实例化的，它保证了在当前线程及其派生的子线程中，这些信息都是可访问的。
2. **获取上下文中的用户信息**：
    
    - `getUserId()`, `getUsername()`, 和 `getRealName()` 方法分别用于获取当前线程上下文中的用户ID、用户名和用户真实姓名。这些方法首先从 `USER_THREAD_LOCAL` 中获取当前线程的 `UserInfoDTO` 对象，然后通过 `Optional` 类的 `map` 方法安全地访问用户信息。如果用户信息不存在，则返回 `null`。
3. **清理用户上下文（`removeUser()`）**：
    
    - 当用户登出或者需要清除线程中保存的用户信息时，调用 `removeUser()` 方法。该方法会从 `USER_THREAD_LOCAL` 中移除当前线程的用户信息，避免内存泄露。

### `TransmittableThreadLocal` 的特点：

- **透明传递**：能够解决在使用线程池等会复用线程的执行模式时，`ThreadLocal` 中的值可能会出现错乱的问题。`TransmittableThreadLocal` 通过包装 `Runnable` 和 `Callable` 等任务，确保线程之间的值传递是正确的。
- **异步执行环境中的友好性**：在使用异步编程模式，特别是在 Spring Boot 中经常会用到的异步处理和任务调度时，`TransmittableThreadLocal` 确保上下文能够正确管理和传递。




## 拦截器和过滤器

在 Spring Boot 应用中，将用户信息设置到用户上下文通常会在请求处理的早期阶段进行，以确保后续处理流程中都能访问到这些信息。在决定使用拦截器（Interceptor）还是过滤器（Filter）时，需要考虑以下几点：

### 过滤器（Filter）

- 过滤器是基于 Servlet 规范定义的，运行于请求进入Servlet之前和响应发送给客户端之后的过程中。
- 过滤器对所有请求都生效，它们是容器级别的处理，不依赖于Spring应用上下文。因此，它们在Spring的`DispatcherServlet`处理请求之前就已经执行。
- 过滤器适合处理一些跨越整个应用的跨域、安全、日志记录等关注点。

### 拦截器（Interceptor）

- 拦截器是Spring框架的一部分，它们依赖于Spring的Web MVC框架。拦截器只能对Controller发起的请求进行拦截。
- 拦截器提供了更细粒度的控制，如只对某些URL模式生效，支持前处理和后处理（请求处理前后以及渲染视图前）。
- 拦截器可以利用Spring框架的功能，如依赖注入（DI），访问Spring管理的bean等。

### 选择建议

- **如果你需要在整个请求处理流程中访问Spring上下文中的bean，或者你需要的控制粒度较细（比如只对特定的URL路径进行处理），推荐使用拦截器。**
- **如果你需要在请求被Spring的`DispatcherServlet`处理之前进行一些预处理，且这些处理是全局性的，不依赖于Spring的Web MVC特性，可以考虑使用过滤器。**

### 实现示例（使用拦截器）

假设用户携带了信息`username`，以下是如何使用拦截器将其设置到用户上下文的简单示例：

```java
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class UserContextInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String username = request.getParameter("username"); // 假设username通过请求参数传递
        if (username != null) {
            UserInfoDTO userInfo = new UserInfoDTO();
            userInfo.setUsername(username);
            // 假设UserContext是你的用户上下文管理类
            UserContext.setUser(userInfo);
        }
        return true; // 继续执行后续的处理流程
    }
}
```

1 将拦截器注册到Spring MVC配置中：
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private UserContextInterceptor userContextInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(userContextInterceptor);
    }
}
```
