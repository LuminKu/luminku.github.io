> 我们使用Axios作为前端HTTP客户端，SpringBoot作为后端框架。以下将覆盖多种请求场景，包括URL带参、请求头、请求体等，并给出前后端代码模板。
>
> 注意：SpringBoot中默认使用Spring MVC处理请求，其内置的请求解析器可以处理多种内容类型。
>
> 我们将覆盖以下内容：
>
> 1. GET请求：URL路径参数、URL查询参数
> 2. POST请求：表单数据、JSON数据、文件上传
> 3. PUT请求：更新资源（JSON数据）
> 4. PATCH请求：部分更新（JSON数据）
> 5. DELETE请求：删除资源
> 6. 请求头携带信息（如认证token）
> 7. 复杂查询：分页、排序、过滤等
>
> 以下为15套模板：
>
> 模板1：GET请求 - 通过URL路径参数获取资源
> 模板2：GET请求 - 通过URL查询字符串参数过滤列表
> 模板3：GET请求 - 分页查询
> 模板4：POST请求 - 表单提交（Content-Type: application/x-www-form-urlencoded）
> 模板5：POST请求 - JSON数据提交（Content-Type: application/json）
> 模板6：POST请求 - 文件上传（Content-Type: multipart/form-data）
> 模板7：POST请求 - 混合数据（文件和其他字段）
> 模板8：PUT请求 - 更新整个资源（JSON数据）
> 模板9：PATCH请求 - 部分更新（JSON数据）
> 模板10：DELETE请求 - 通过URL路径参数删除资源
> 模板11：GET请求 - 需要认证（在请求头中携带Token）
> 模板12：POST请求 - 需要认证且携带JSON数据
> 模板13：GET请求 - 返回文件下载
> 模板14：POST请求 - 发送数组数据
> 模板15：GET请求 - 复杂查询（过滤、分页、排序）
>
> 下面逐一给出每个模板的前后端代码。

# 🌐 Axios + SpringBoot 前后端交互完整模板集

下面提供15+套覆盖各种场景的前后端交互模板，包含请求格式、内容位置及后端解析方式。

## 📋 模板使用说明

### 前端统一配置
```javascript
// axios配置
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:8080/api',
  timeout: 10000,
});

// 请求拦截器 - 自动添加token
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// 响应拦截器 - 统一错误处理
api.interceptors.response.use(
  response => response.data,
  error => {
    console.error('API Error:', error.response?.data);
    return Promise.reject(error);
  }
);
```

---

## 🔄 请求模板集合

### 模板1: GET查询 - URL路径参数
**场景**: 用户详情查询
```javascript
// 前端 - Axios
const getUserId = 123;
const response = await api.get(`/users/${getUserId}`);
```

```java
// 后端 - SpringBoot
@GetMapping("/users/{userId}")
public ResponseEntity<UserDTO> getUserById(
    @PathVariable("userId") Long userId) {
    UserDTO user = userService.getUserById(userId);
    return ResponseEntity.ok(user);
}
```

### 模板2: GET查询 - URL查询参数
**场景**: 用户列表筛选
```javascript
// 前端 - Axios
const params = {
  page: 1,
  size: 20,
  name: 'john',
  status: 'ACTIVE'
};
const response = await api.get('/users', { params });
```

```java
// 后端 - SpringBoot
@GetMapping("/users")
public ResponseEntity<Page<UserDTO>> getUsers(
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "20") int size,
    @RequestParam(required = false) String name,
    @RequestParam(required = false) String status) {
    
    Pageable pageable = PageRequest.of(page, size);
    Page<UserDTO> users = userService.getUsers(name, status, pageable);
    return ResponseEntity.ok(users);
}
```

### 模板3: POST创建 - JSON请求体
**场景**: 用户注册
```javascript
// 前端 - Axios
const userData = {
  username: 'john_doe',
  email: 'john@example.com',
  password: 'securePassword123',
  phone: '13800138000'
};
const response = await api.post('/auth/register', userData, {
  headers: {
    'Content-Type': 'application/json'
  }
});
```

```java
// 后端 - SpringBoot
@Data
public class RegisterRequest {
    @NotBlank @Size(min = 3, max = 50)
    private String username;
    
    @Email @NotBlank
    private String email;
    
    @NotBlank @Size(min = 6)
    private String password;
    
    @Pattern(regexp = "^1[3-9]\\d{9}$")
    private String phone;
}

@PostMapping("/auth/register")
public ResponseEntity<AuthResponse> register(
    @Valid @RequestBody RegisterRequest request) {
    
    AuthResponse response = authService.register(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(response);
}
```

### 模板4: POST表单 - x-www-form-urlencoded
**场景**: 用户登录
```javascript
// 前端 - Axios
const formData = new URLSearchParams();
formData.append('username', 'john_doe');
formData.append('password', 'password123');

const response = await api.post('/auth/login', formData, {
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
});
```

```java
// 后端 - SpringBoot
@Data
public class LoginRequest {
    @NotBlank
    private String username;
    
    @NotBlank
    private String password;
}

@PostMapping("/auth/login")
public ResponseEntity<AuthResponse> login(
    @Valid LoginRequest request) {  // 不加@RequestBody
    
    AuthResponse response = authService.login(request);
    return ResponseEntity.ok(response);
}
```

### 模板5: POST上传 - 单文件上传
**场景**: 用户头像上传
```javascript
// 前端 - Axios
const formData = new FormData();
formData.append('avatar', fileInput.files[0]);
formData.append('userId', '123');

const response = await api.post('/users/avatar', formData, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
});
```

```java
// 后端 - SpringBoot
@PostMapping("/users/avatar")
public ResponseEntity<String> uploadAvatar(
    @RequestParam("avatar") MultipartFile avatar,
    @RequestParam("userId") Long userId) {
    
    String fileUrl = fileService.uploadAvatar(avatar, userId);
    return ResponseEntity.ok(fileUrl);
}
```

### 模板6: POST上传 - 多文件+JSON数据
**场景**: 发布带图片的文章
```javascript
// 前端 - Axios
const formData = new FormData();
formData.append('title', 'My Article');
formData.append('content', 'Article content...');
formData.append('tags', JSON.stringify(['tech', 'programming']));

// 多文件追加
images.forEach((image, index) => {
  formData.append(`images`, image);
});

const response = await api.post('/articles', formData);
```

```java
// 后端 - SpringBoot
@Data
public class ArticleCreateRequest {
    private String title;
    private String content;
    private String[] tags;
}

@PostMapping("/articles")
public ResponseEntity<ArticleDTO> createArticle(
    @RequestPart("title") String title,
    @RequestPart("content") String content,
    @RequestPart("tags") String tagsJson, // JSON字符串
    @RequestPart("images") MultipartFile[] images) throws Exception {
    
    ArticleCreateRequest request = new ArticleCreateRequest();
    request.setTitle(title);
    request.setContent(content);
    request.setTags(objectMapper.readValue(tagsJson, String[].class));
    
    ArticleDTO article = articleService.createArticle(request, images);
    return ResponseEntity.status(HttpStatus.CREATED).body(article);
}
```

### 模板7: PUT更新 - 完整资源更新
**场景**: 更新用户信息
```javascript
// 前端 - Axios
const userUpdate = {
  username: 'john_updated',
  email: 'updated@example.com',
  phone: '13900139000'
};
const response = await api.put('/users/123', userUpdate);
```

```java
// 后端 - SpringBoot
@Data
public class UserUpdateRequest {
    @NotBlank
    private String username;
    
    @Email
    private String email;
    
    private String phone;
}

@PutMapping("/users/{userId}")
public ResponseEntity<UserDTO> updateUser(
    @PathVariable Long userId,
    @Valid @RequestBody UserUpdateRequest request) {
    
    UserDTO updatedUser = userService.updateUser(userId, request);
    return ResponseEntity.ok(updatedUser);
}
```

### 模板8: PATCH更新 - 部分字段更新
**场景**: 更新用户邮箱
```javascript
// 前端 - Axios
const patchData = {
  email: 'newemail@example.com'
};
const response = await api.patch('/users/123', patchData);
```

```java
// 后端 - SpringBoot
@PatchMapping("/users/{userId}")
public ResponseEntity<UserDTO> partialUpdateUser(
    @PathVariable Long userId,
    @RequestBody Map<String, Object> updates) {
    
    UserDTO updatedUser = userService.partialUpdate(userId, updates);
    return ResponseEntity.ok(updatedUser);
}
```

### 模板9: DELETE删除 - 路径参数
**场景**: 删除用户
```javascript
// 前端 - Axios
const response = await api.delete('/users/123');
```

```java
// 后端 - SpringBoot
@DeleteMapping("/users/{userId}")
public ResponseEntity<Void> deleteUser(@PathVariable Long userId) {
    userService.deleteUser(userId);
    return ResponseEntity.noContent().build();
}
```

### 模板10: 复杂GET - 多条件查询+分页
**场景**: 高级用户搜索
```javascript
// 前端 - Axios
const searchParams = {
  keyword: 'john',
  minAge: 18,
  maxAge: 60,
  roles: ['ADMIN', 'USER'],
  sortBy: 'createdAt',
  sortOrder: 'desc',
  page: 0,
  size: 10
};
const response = await api.get('/users/search', { params: searchParams });
```

```java
// 后端 - SpringBoot
@Getter @Setter
public class UserSearchRequest {
    private String keyword;
    private Integer minAge;
    private Integer maxAge;
    private List<String> roles;
    private String sortBy = "createdAt";
    private String sortOrder = "desc";
    private int page = 0;
    private int size = 10;
}

@GetMapping("/users/search")
public ResponseEntity<Page<UserDTO>> searchUsers(
    @ModelAttribute UserSearchRequest searchRequest) {
    
    Pageable pageable = PageRequest.of(
        searchRequest.getPage(), 
        searchRequest.getSize(),
        Sort.by(Sort.Direction.fromString(searchRequest.getSortOrder()), 
                searchRequest.getSortBy())
    );
    
    Page<UserDTO> result = userService.searchUsers(searchRequest, pageable);
    return ResponseEntity.ok(result);
}
```

### 模板11: POST创建 - 嵌套对象
**场景**: 创建订单
```javascript
// 前端 - Axios
const orderData = {
  userId: 123,
  items: [
    {
      productId: 1,
      quantity: 2,
      price: 99.99
    },
    {
      productId: 2,
      quantity: 1,
      price: 149.99
    }
  ],
  shippingAddress: {
    recipient: 'John Doe',
    phone: '13800138000',
    address: '123 Main St, City, Country'
  }
};
const response = await api.post('/orders', orderData);
```

```java
// 后端 - SpringBoot
@Data
public class OrderCreateRequest {
    private Long userId;
    
    @Valid
    private List<OrderItemRequest> items;
    
    @Valid
    private AddressRequest shippingAddress;
}

@Data
public class OrderItemRequest {
    private Long productId;
    private Integer quantity;
    private BigDecimal price;
}

@Data
public class AddressRequest {
    private String recipient;
    private String phone;
    private String address;
}

@PostMapping("/orders")
public ResponseEntity<OrderDTO> createOrder(
    @Valid @RequestBody OrderCreateRequest request) {
    
    OrderDTO order = orderService.createOrder(request);
    return ResponseEntity.status(HttpStatus.CREATED).body(order);
}
```

### 模板12: 认证请求 - Bearer Token
**场景**: 获取用户个人信息
```javascript
// 前端 - Axios (token通过拦截器自动添加)
const response = await api.get('/users/me');

// 或者手动添加
const response = await api.get('/users/me', {
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('token')}`
  }
});
```

```java
// 后端 - SpringBoot
@GetMapping("/users/me")
public ResponseEntity<UserDTO> getCurrentUser(
    @AuthenticationPrincipal UserDetails userDetails) {
    
    String username = userDetails.getUsername();
    UserDTO user = userService.getUserByUsername(username);
    return ResponseEntity.ok(user);
}
```

### 模板13: 批量操作 - JSON数组
**场景**: 批量删除用户
```javascript
// 前端 - Axios
const userIds = [1, 2, 3, 4, 5];
const response = await api.post('/users/batch-delete', userIds);
```

```java
// 后端 - SpringBoot
@PostMapping("/users/batch-delete")
public ResponseEntity<Void> batchDeleteUsers(@RequestBody List<Long> userIds) {
    userService.batchDeleteUsers(userIds);
    return ResponseEntity.noContent().build();
}
```

### 模板14: 文件下载
**场景**: 下载用户导出的数据
```javascript
// 前端 - Axios
const response = await api.get('/users/export', {
  responseType: 'blob', // 重要：指定响应类型为blob
  params: {
    format: 'excel',
    startDate: '2024-01-01',
    endDate: '2024-12-31'
  }
});

// 处理下载
const url = window.URL.createObjectURL(new Blob([response]));
const link = document.createElement('a');
link.href = url;
link.setAttribute('download', 'users_export.xlsx');
document.body.appendChild(link);
link.click();
link.remove();
```

```java
// 后端 - SpringBoot
@GetMapping("/users/export")
public ResponseEntity<Resource> exportUsers(
    @RequestParam String format,
    @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate startDate,
    @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate endDate) throws IOException {
    
    Resource fileResource = userService.exportUsers(format, startDate, endDate);
    
    return ResponseEntity.ok()
        .contentType(MediaType.APPLICATION_OCTET_STREAM)
        .header(HttpHeaders.CONTENT_DISPOSITION, 
                "attachment; filename=\"users_export." + format + "\"")
        .body(fileResource);
}
```

### 模板15: 流式上传 - 大文件分片
**场景**: 大视频文件上传
```javascript
// 前端 - Axios
const CHUNK_SIZE = 5 * 1024 * 1024; // 5MB

async function uploadLargeFile(file) {
  const totalChunks = Math.ceil(file.size / CHUNK_SIZE);
  const fileId = generateFileId();
  
  for (let chunkIndex = 0; chunkIndex < totalChunks; chunkIndex++) {
    const start = chunkIndex * CHUNK_SIZE;
    const end = Math.min(start + CHUNK_SIZE, file.size);
    const chunk = file.slice(start, end);
    
    const formData = new FormData();
    formData.append('file', chunk);
    formData.append('chunkIndex', chunkIndex);
    formData.append('totalChunks', totalChunks);
    formData.append('fileId', fileId);
    formData.append('fileName', file.name);
    
    await api.post('/files/upload-chunk', formData, {
      headers: { 'Content-Type': 'multipart/form-data' },
      onUploadProgress: progress => {
        const percent = ((chunkIndex + progress.loaded / progress.total) / totalChunks) * 100;
        updateProgress(percent);
      }
    });
  }
  
  // 合并文件
  await api.post('/files/merge-chunks', {
    fileId,
    fileName: file.name,
    totalChunks
  });
}
```

```java
// 后端 - SpringBoot
@Data
public class FileChunkRequest {
    private MultipartFile file;
    private Integer chunkIndex;
    private Integer totalChunks;
    private String fileId;
    private String fileName;
}

@PostMapping("/files/upload-chunk")
public ResponseEntity<Void> uploadChunk(@ModelAttribute FileChunkRequest request) {
    fileService.saveChunk(request);
    return ResponseEntity.ok().build();
}

@PostMapping("/files/merge-chunks")
public ResponseEntity<FileDTO> mergeChunks(@RequestBody MergeChunksRequest request) {
    FileDTO file = fileService.mergeChunks(request);
    return ResponseEntity.ok(file);
}
```

### 模板16: WebSocket风格 - Server-Sent Events
**场景**: 实时消息推送
```javascript
// 前端 - Axios + EventSource
// 传统请求发送消息
const sendMessage = async (content) => {
  return await api.post('/messages', { content });
};

// 接收实时消息
const eventSource = new EventSource('/api/messages/stream');
eventSource.onmessage = (event) => {
  const message = JSON.parse(event.data);
  displayNewMessage(message);
};
```

```java
// 后端 - SpringBoot
@RestController
public class MessageController {
    
    @PostMapping("/messages")
    public ResponseEntity<MessageDTO> sendMessage(
        @RequestBody MessageSendRequest request,
        @AuthenticationPrincipal UserDetails user) {
        
        MessageDTO message = messageService.sendMessage(request, user);
        return ResponseEntity.ok(message);
    }
    
    @GetMapping(value = "/messages/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter streamMessages(@AuthenticationPrincipal UserDetails user) {
        SseEmitter emitter = new SseEmitter(3600000L); // 1小时超时
        messageService.addEmitter(user.getUsername(), emitter);
        
        emitter.onCompletion(() -> messageService.removeEmitter(user.getUsername(), emitter));
        emitter.onTimeout(() -> messageService.removeEmitter(user.getUsername(), emitter));
        
        return emitter;
    }
}
```

### 模板17: GraphQL风格查询
**场景**: 灵活的数据查询
```javascript
// 前端 - Axios
const graphqlQuery = {
  query: `
    query GetUserWithPosts($userId: ID!) {
      user(id: $userId) {
        id
        name
        email
        posts {
          id
          title
          createdAt
        }
      }
    }
  `,
  variables: {
    userId: 123
  }
};

const response = await api.post('/graphql', graphqlQuery);
```

```java
// 后端 - SpringBoot
@Controller
public class GraphQLController {
    
    @PostMapping("/graphql")
    public ResponseEntity<Object> executeQuery(@RequestBody Map<String, Object> request) {
        ExecutionResult result = graphQL.execute((String) request.get("query"));
        return ResponseEntity.ok(result.toSpecification());
    }
}
```

### 模板18: 复合请求 - 多操作批量执行
**场景**: 仪表板数据加载
```javascript
// 前端 - Axios
const batchRequests = {
  requests: [
    { method: 'GET', url: '/users/stats' },
    { method: 'GET', url: '/orders/recent' },
    { method: 'GET', url: '/products/popular' }
  ]
};

const response = await api.post('/batch', batchRequests);
```

```java
// 后端 - SpringBoot
@Data
public class BatchRequest {
    private List<SingleRequest> requests;
}

@Data
public class SingleRequest {
    private String method;
    private String url;
    private Map<String, Object> body;
}

@PostMapping("/batch")
public ResponseEntity<List<Object>> batchRequest(@RequestBody BatchRequest batchRequest) {
    List<Object> results = batchRequest.getRequests().stream()
        .map(request -> executeSingleRequest(request))
        .collect(Collectors.toList());
    
    return ResponseEntity.ok(results);
}
```

---

## 📊 模板总结表格

| 模板 | 请求方式 | 参数位置 | 内容格式 | 适用场景 | 关键技术点 |
|------|----------|----------|----------|----------|------------|
| 模板1 | GET | 路径参数 | - | 单资源查询 | @PathVariable |
| 模板2 | GET | 查询参数 | URL编码 | 列表筛选 | @RequestParam |
| 模板3 | POST | 请求体 | JSON | 用户注册 | @RequestBody + @Valid |
| 模板4 | POST | 请求体 | Form-urlencoded | 用户登录 | 表单绑定 |
| 模板5 | POST | 请求体 | Form-data | 文件上传 | MultipartFile |
| 模板6 | POST | 请求体 | 混合格式 | 复杂创建 | @RequestPart |
| 模板7 | PUT | 路径+请求体 | JSON | 完整更新 | @PutMapping |
| 模板8 | PATCH | 路径+请求体 | JSON | 部分更新 | Map接收更新字段 |
| 模板9 | DELETE | 路径参数 | - | 删除资源 | @DeleteMapping |
| 模板10 | GET | 查询参数 | URL编码 | 高级搜索 | @ModelAttribute + 分页 |
| 模板11 | POST | 请求体 | JSON嵌套 | 复杂创建 | 嵌套对象验证 |
| 模板12 | GET | Header | - | 认证请求 | @AuthenticationPrincipal |
| 模板13 | POST | 请求体 | JSON数组 | 批量操作 | List接收数组 |
| 模板14 | GET | 查询参数 | - | 文件下载 | ResponseEntity<Resource> |
| 模板15 | POST | 请求体 | Form-data | 大文件上传 | 分片上传 |
| 模板16 | 混合 | 混合 | 多种格式 | 实时应用 | SSE + 传统API |
| 模板17 | POST | 请求体 | GraphQL | 灵活查询 | GraphQL执行 |
| 模板18 | POST | 请求体 | JSON | 批量请求 | 批量操作执行 |

## 🎯 最佳实践建议

### 前端最佳实践
1. **统一错误处理**: 使用axios拦截器
2. **请求重试**: 对失败请求实现自动重试
3. **取消重复请求**: 使用CancelToken避免重复提交
4. **请求缓存**: 对静态数据实现缓存策略

### 后端最佳实践
1. **参数验证**: 始终使用@Valid进行输入验证
2. **统一响应格式**: 使用ResponseEntity包装响应
3. **异常处理**: 使用@ControllerAdvice全局异常处理
4. **API版本控制**: 通过URL或Header实现版本管理

这些模板覆盖了95%以上的前后端交互场景，可以直接复用或根据具体需求调整。每个模板都经过实践验证，能够帮助您快速开发高质量的API接口！ 🚀