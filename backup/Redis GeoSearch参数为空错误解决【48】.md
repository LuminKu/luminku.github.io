从错误日志来看，问题出现在 `RedisGeoCommands.GeoSearchCommandArgs.limit()` 方法的调用中，具体是 `Count has to positive value`，即传入的 `limit` 参数值必须是正数。这表明在调用 `GeoSearch` 方法时，`limit` 参数的值不正确。

以下是可能的原因和解决方案：

---

### 1. **`limit` 参数值不正确**
在 `ShopServiceImpl.queryShopByType` 方法中，`limit` 参数的值是通过计算得到的：
```java
int to = from * SystemConstants.DEFAULT_PAGE_SIZE;
```
从错误日志来看，`limit` 的值可能是非正数（如 `0` 或负数）。这可能是因为 `from` 的值不正确，导致 `to` 的值也不正确。

#### 解决方法：
- 确保 `from` 和 `to` 的计算逻辑正确。例如：
  ```java
  int from = (current - 1) * SystemConstants.DEFAULT_PAGE_SIZE;
  int to = from + SystemConstants.DEFAULT_PAGE_SIZE;
  ```
  这样可以确保 `to` 始终是一个正数。

---

### 2. **Spring Data Redis 版本与 Redis 服务器版本不兼容**
虽然错误日志中没有直接提到版本不兼容的问题，但 `RedisGeoCommands.GeoSearchCommandArgs.limit()` 方法在某些版本中可能存在问题。例如，某些 Redis 服务器版本可能不支持某些参数或命令。

#### 解决方法：
- **检查 Redis 服务器版本**：确保 Redis 服务器版本支持 `GEOSEARCH` 命令。`GEOSEARCH` 是 Redis 6.2+ 的新特性，如果服务器版本低于 6.2，可能会出现问题。
- **检查 Spring Data Redis 版本**：确保 Spring Data Redis 的版本与 Redis 服务器版本兼容。例如，`spring-data-redis` 2.6.2 版本可能需要 Redis 6.2+。
- **升级版本**：如果可能，升级 Redis 服务器到 6.2 或更高版本，并确保 Spring Data Redis 版本与之兼容。

---

### 3. **代码逻辑问题**
从代码中可以看到，`limit` 的值是通过 `to` 计算的：
```java
RedisGeoCommands.GeoSearchCommandArgs.newGeoSearchArgs().includeDistance().limit(to)
```
如果 `to` 的值为 `0` 或负数，就会导致 `IllegalArgumentException`。

#### 解决方法：
- 在调用 `limit()` 方法之前，添加一个检查：
  ```java
  if (to <= 0) {
      to = 1; // 或者设置为一个合理的默认值
  }
  ```

---

### 4. **分页逻辑问题**
从代码来看，分页逻辑可能存在问题。例如，当 `current` 为 `1` 时，`from` 的值为 `0`，而 `to` 的值可能为 `0`，导致 `limit` 参数为 `0`。

#### 解决方法：
- 确保分页逻辑正确。例如：
  ```java
  int from = (current - 1) * SystemConstants.DEFAULT_PAGE_SIZE;
  int to = from + SystemConstants.DEFAULT_PAGE_SIZE;
  if (to <= 0) {
      to = SystemConstants.DEFAULT_PAGE_SIZE; // 默认至少返回一页数据
  }
  ```

---

### 5. **调试和日志**
为了更好地定位问题，可以在调用 `GeoSearch` 方法之前打印相关参数的值：
```java
log.info("current: {}, from: {}, to: {}", current, from, to);
```
通过日志可以确认参数值是否正确。

---

### 总结
根据错误日志，问题的根本原因是 `limit` 参数值不正确。可以通过以下步骤解决：
1. 确保分页逻辑正确，避免 `limit` 参数为非正数。
2. 检查 Redis 服务器版本和 Spring Data Redis 版本是否兼容。
3. 在调用 `limit()` 方法之前，添加参数值的检查。

如果问题仍未解决，可以提供更多上下文信息（如 Redis 服务器版本、Spring Data Redis 版本等），以便进一步分析。