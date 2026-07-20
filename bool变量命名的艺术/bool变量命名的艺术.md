# 布尔变量命名的艺术

> 💡 **译者注**：本文翻译自国外优秀技术博客，原文链接：[Stop Naming Your Variables "flag" — The Art of Boolean Prefixes][original-article]。

良好的布尔值（Boolean）命名能够让代码实现“自解释”，大幅提升可读性。以下是关于布尔变量命名与处理的佳实践指南。

---

## 💡 4 种推荐的命名前缀

### 1. `is`：身份与状态 (Identity and State)
当描述事物当前处于什么状态或是什么时使用，通常与**形容词**或**被动动词**搭配。
* **推荐：** `isActive`（是否活跃）、`isDeleted`（是否已删除）、`isEmpty`（是否为空）。
* **不推荐：** `isAccess`（名词搭配，语法不通，应改为 `hasAccess`）。

### 2. `has`：包含与特征 (Containment and Features)
当描述所有权、是否包含某物时使用，通常与**名词**搭配。
* **推荐：** `hasAccess`（是否有权限）、`hasChildren`（是否有子项）、`hasValidationErrors`（是否有校验错误）。
* **不推荐：** `hasActive`（形容词搭配，语法不通，应改为 `isActive`）。

### 3. `can`：能力与权限 (Capability)
用于检查权限或某个动作是否允许被执行（能够执行某操作）。
* **推荐：** `canEdit`（是否可编辑）、`canDelete`（是否可删除）、`canRetry`（是否可重试）。
* **不推荐：** `canAdmin`（含义模糊，应改为 `isAdmin` 或 `canAdminister`）。

### 4. `should`：意图与决策 (Intent)
用于表示业务规则或系统需要做出的决策。它能很好地把“**能做什么（can）**”和“**想做什么（should）**”区分开。
* **推荐：** `shouldRetry`（是否应该重试）、`shouldCacheResponse`（是否应该缓存响应）。
* **不推荐：** `shouldUser`（表意不完整，应明确为具体动作如 `shouldCreateUser`）。

---

## 🚫 避免在变量中使用否定词

> ⚠️ **核心原则**：双重否定（如 `!isDisabled`）会极大增加大脑的认知负担，请务必保持变量名的正向表达。

* **避免使用：** `isNotEnabled`, `hasNoAccess`, `isDisabled`
* **推荐改为：** `isEnabled`, `hasAccess`, `isEnabled` （视具体业务而定）

---

## 🛠️ 方法参数中布尔值的处理

> 💡 **核心结论**：尽量**不要**在方法的参数列表中直接传递布尔字面量。

当你看到下面这行代码时，很难一眼看出每个参数的含义：
```csharp
schemaExport.Execute(false, true, false); // 这些 true/false 到底控制什么？
```

### 推荐的 3 种重构方式

#### 1. 方法隔离 (Method Isolation)
当布尔值用于控制完全不同的行为时，拆分为独立的方法。

```csharp
// ❌ 糟糕的写法：不知道 true 代表立即发送还是高优先级
email.Send(message, true);

// ✅ 推荐的写法：意图清晰直观
email.SendImmediately(message);
email.SendQueued(message);
```

#### 2. 使用枚举 (Using Enums)
当布尔值表达的是某种“模式”或“状态”时，用枚举替代。

```csharp
// ❌ 糟糕的写法：true 的含义隐晦
file.Write(data, true);

// ✅ 推荐的写法：一眼看出是“追加模式”
file.Write(data, WriteMode.Append);
```

#### 3. 使用配置对象 (Using Parameter Objects)
当方法包含多个布尔参数时，将它们封装进一个显式的配置对象中。

```csharp
// ❌ 糟糕的写法
export.Execute(false, true, false);

// ✅ 推荐的写法：通过属性名实现自解释
export.Execute(new ExportOptions {
    Script = false,
    Export = true,
    JustDrop = false
});
```

---

## 📄 声明与版权

* **原文标题**：Stop Naming Your Variables "flag" — The Art of Boolean Prefixes
* **文章作者**：ThatAmazingProgrammer
* **原文链接**：[点击阅读原文][original-article]

> ⚠️ **声明**：本篇译文基于原作者文章进行翻译与排版优化，版权归原作者所有。商业转载请联系原作者获得授权。

[original-article]: https://thatamazingprogrammer.com/posts/stop-naming-your-variables-flag-the-art-of-boolean-prefixes/
