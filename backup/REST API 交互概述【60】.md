# [REST API 交互概述](https://www.runoob.com/restfulapi/restful-api-tutorial.html)

[1](https://www.runoob.com/restfulapi/restful-api-tutorial.html)
[2](https://blog.csdn.net/mng123/article/details/146473344)
[3](https://www.cnblogs.com/uwakeme/articles/18892684)

>REST API（表述性状态转移应用程序接口）是一种基于 HTTP 协议的架构风格，用于实现客户端与服务器之间的交互。它通过资源导向的方式，将系统功能抽象为资源，并使用标准的 HTTP 方法（如 GET、POST、PUT、DELETE）对资源进行操作。

## 核心概念与原则

REST API 的核心在于资源的设计。每个资源都有唯一的 URI（统一资源标识符），例如 /users/123 表示 ID 为 123 的用户。资源可以通过多种表述形式（如 JSON、XML）进行传输，客户端通过 Accept 头指定所需格式。

REST API 遵循无状态性原则，即每个请求都包含完成该请求所需的所有信息，服务器不保存客户端状态。这种设计使得系统更易扩展和负载均衡。

## HTTP 方法的使用

REST API 利用 HTTP 方法的语义来描述操作：

```plain text
GET - 获取资源（安全且幂等）
POST - 创建资源（非幂等）
PUT - 完整更新资源（幂等）
PATCH - 部分更新资源（非幂等）
DELETE - 删除资源（幂等）
```

例如：

GET /users 获取所有用户。

POST /users 创建新用户。

DELETE /users/123 删除 ID 为 123 的用户。

## 状态码与错误处理

REST API 使用标准 HTTP 状态码来表示请求结果：

2xx：成功（如 200 OK, 201 Created）。

4xx：客户端错误（如 400 Bad Request, 404 Not Found）。

5xx：服务器错误（如 500 Internal Server Error）。

错误响应应包含详细的错误信息，例如：
```json
{
"error": {
"code": "VALIDATION_ERROR",
"message": "Invalid input data",
"details": [
{ "field": "email", "message": "Must be a valid email address" }
]
}
}
```
## 安全性与性能优化

REST API 的安全性可以通过 HTTPS 加密、令牌认证（如 JWT）、输入验证等方式保障。性能优化包括使用缓存（如 ETag）、分页、筛选和异步处理等技术。例如，分页可以通过 GET /users?page=1&size=10 实现。

## 设计示例

以下是一个用户管理系统的 REST API 示例：

获取用户列表：GET /users

创建用户：POST /users

获取单个用户：GET /users/{id}

更新用户：PUT /users/{id}

删除用户：DELETE /users/{id}

返回的 JSON 数据示例：
```json
{
"id": 123,
"name": "John Doe",
"email": "john.doe@example.com",
"created_at": "2023-01-01T12:00:00Z"
}
```
REST API 以其简单性、可扩展性和跨平台兼容性，成为现代 Web 开发中最常用的接口设计方式。