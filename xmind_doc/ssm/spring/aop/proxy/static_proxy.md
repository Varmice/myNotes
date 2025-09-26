# 静态代理

## 什么是静态代理

静态代理是一种设计模式，通过为目标对象创建一个代理类，在不修改目标对象的前提下，扩展目标对象的功能。

## 静态代理的组成

1. **抽象主题接口**：定义真实对象和代理对象的公共接口
2. **真实主题类**：实现抽象接口，定义真实对象
3. **代理类**：实现抽象接口，内部持有真实对象的引用，并可以在调用真实对象方法前后添加额外逻辑

## 代码示例

### 1. 定义抽象接口

```java
public interface UserService {
    void addUser(String username);
    void deleteUser(String username);
}
```

### 2. 真实主题类

```java
public class UserServiceImpl implements UserService {
    @Override
    public void addUser(String username) {
        System.out.println("添加用户：" + username);
    }

    @Override
    public void deleteUser(String username) {
        System.out.println("删除用户：" + username);
    }
}
```

### 3. 代理类

```java
public class UserServiceProxy implements UserService {
    private UserService userService;

    public UserServiceProxy(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void addUser(String username) {
        // 前置增强
        System.out.println("开始添加用户...");
        long startTime = System.currentTimeMillis();

        // 调用真实对象的方法
        userService.addUser(username);

        // 后置增强
        long endTime = System.currentTimeMillis();
        System.out.println("添加用户完成，耗时：" + (endTime - startTime) + "ms");
    }

    @Override
    public void deleteUser(String username) {
        // 权限检查
        if (!hasPermission()) {
            System.out.println("权限不足，无法删除用户");
            return;
        }

        System.out.println("开始删除用户...");
        userService.deleteUser(username);
        System.out.println("删除用户完成");
    }

    private boolean hasPermission() {
        // 模拟权限检查
        return true;
    }
}
```

### 4. 测试代码

```java
public class ProxyTest {
    public static void main(String[] args) {
        // 创建真实对象
        UserService userService = new UserServiceImpl();

        // 创建代理对象
        UserService proxy = new UserServiceProxy(userService);

        // 通过代理对象调用方法
        proxy.addUser("张三");
        proxy.deleteUser("李四");
    }
}
```

## 静态代理的优缺点

### 优点
- 可以在不修改目标对象的情况下扩展功能
- 符合开闭原则
- 代码结构清晰，易于理解

### 缺点
- 每个目标对象都需要创建对应的代理类，代码冗余
- 当接口方法较多时，代理类会变得臃肿
- 维护成本高，接口变更时代理类也需要修改

## 应用场景

1. **日志记录**：在方法执行前后记录日志
2. **性能监控**：统计方法执行时间
3. **权限控制**：在方法执行前进行权限检查
4. **事务管理**：在方法执行前后处理事务
5. **缓存处理**：对方法结果进行缓存

## 与动态代理的区别

| 特性 | 静态代理 | 动态代理 |
|------|----------|----------|
| 编译时期 | 代理类在编译时确定 | 代理类在运行时生成 |
| 代理类数量 | 每个目标类需要一个代理类 | 一个代理类可以代理多个目标类 |
| 灵活性 | 较低 | 较高 |
| 性能 | 较高 | 略低（需要反射） |
| 代码维护 | 维护成本高 | 维护成本低 |

## 总结

静态代理是理解代理模式的基础，虽然存在代码冗余的问题，但其思想在很多框架中都有体现。在实际开发中，通常使用动态代理来解决静态代理的不足。