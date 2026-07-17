# bool变量命名的艺术


## 4种推荐的命名前缀

1. IS：身份与状态 (Identity and State)
当描述事物当前处于什么状态或是什么时，使用 is。它通常与形容词搭配。

    * 推荐： isActive（是否活跃）、isDeleted（是否已删除）、isEmpty（是否为空）。
    * 不推荐： isAccess（语法不匹配，应该用 hasAccess）。

2. HAS：包含与特征 (Containment and Features)
当描述所有权或是否包含某物时，使用 has。它通常与名词搭配。

    * 推荐： hasAccess（是否有权限）、hasChildren（是否有子项）、hasValidationErrors（是否有校验错误）。
    * 不推荐： hasActive（语法不匹配，应该用 isActive）。

3. CAN：能力与权限 (Capability)
使用 can 来检查权限或潜在的动作（能否执行某操作）。

    * 推荐： canEdit（是否可编辑）、canDelete（是否可删除）、canRetry（是否可重试）。
    * 不推荐： canAdmin（含义模糊，应该用 isAdmin 或是 canAdminister）。

4. SHOULD：意图与决策 (Intent)
使用 should 来表示业务规则或系统需要做出的决策。它能很好地把“我们能做什么（capability）”和“我们想做什么（intent）”区分开。

    * 推荐： shouldRetry（是否应该重试）、shouldCacheResponse（是否应该缓存响应）。
    * 不推荐： shouldUser（表意不完整，是想表达 shouldCreateUser 吗？）。

## 不建议在变量中使用否定词(增加阅读者大脑认知负担)


* 避免使用 `isNotEnabled`, `hasNoAccess`, or `isDisabled`.

## 方法参数中bool值的处理

* 结论:不建议方法参数中使用bool变量

```
schemaExport.Execute(false, true, false);
```

* 处理方式

    1. 方法隔离
    ```
    // Bad
    email.Send(message, true); // is true "immediate"? "high priority"?

    // Good
    email.SendImmediately(message);
    email.SendQueued(message);
    ```

    2. 使用枚举

    ```
    // Bad
    file.Write(data, true);

    // Good
    file.Write(data, WriteMode.Append);
    ```
    3. 使用配置对象
    ```
    // Bad
    export.Execute(false, true, false);

    // Good
    export.Execute(new ExportOptions {
        Script = false,
        Export = true,
        JustDrop = false
    });
    ```
