> 
> 在MDX中，我们可以使用一些JavaScript代码，但是需要注意的是，MDX文件通常被编译为React组件，因此它支持在模块顶层导入（import）和导出（export），以及内联的JSX表达式（即用花括号{}包裹的JavaScript表达式）。但是，MDX并不支持在模块顶层直接定义变量、函数或组件，因为这样会破坏编译过程。
>
> 然而，我们可以通过以下方式在MDX中定义变量、函数和组件：
>
> 1. 使用导入（import）语句：可以从其他模块导入变量、函数、组件等。
> 2. 使用导出（export）语句：可以导出一些内容，这通常用于元数据（如标题、作者等）。
> 3. 在内联表达式中使用立即执行函数表达式（IIFE）来定义变量和函数，但这种方式定义的变量和函数只能在该表达式内部使用。
> 4. 使用MDX提供的特殊组件，例如`<MDXProvider />`来包装自定义组件。
>
> 但是，在MDX文件的顶层（即不在花括号内）直接写`const name = 'Kimi';`这样的语句是不被允许的，因为MDX的编译会将顶层内容解析为React元素，而JavaScript语句不能直接出现在这里。
>
> 下面我们通过一些例子来说明如何在MDX中定义变量、函数和组件：
>
> ## 1. 导入（import）
>
> 我们可以导入已经定义好的组件、变量或函数。
>
> mdx
>
> ```
> import { MyComponent } from './MyComponent'
> import someData from './data.json'
> 
> 现在我们可以使用导入的组件和数据：<MyComponent data={someData} />
> ```
>
> 
>
> ## 2. 导出（export）
>
> 我们可以导出一些元数据，这些元数据可以被处理MDX的工具读取。
>
> mdx
>
> ```
> export const title = '我的MDX文档'
> export const author = 'Kimi'
> 
> 这些导出项不会被直接渲染在页面上，但可以被用于生成文档信息。
> ```
>
> 
>
> ## 3. 使用内联表达式（IIFE）
>
> 我们可以在花括号内使用IIFE来定义变量和函数，并立即使用它们。
>
> mdx
>
> ```
> {(function() {
>   const name = 'Kimi';
>   return <div>你好，{name}！</div>;
> })()}
> ```
>
> 
>
> 但是，这样定义的变量和函数只在IIFE内部有效，无法在同一个MDX文件的其他地方使用。
>
> ## 4. 使用自定义组件
>
> 我们可以通过`<MDXProvider />`在外部提供自定义组件，然后在MDX中使用它们。例如，在Next.js中，我们可以这样包装MDX内容：
>
> jsx
>
> ```
> // 在_next.js的页面中
> import { MDXProvider } from '@mdx-js/react'
> 
> const components = {
>   h1: (props) => <h1 style={{ color: 'red' }} {...props} />,
>   MyComponent: (props) => <div>这是一个自定义组件</div>,
> }
> 
> export default function Post({ children }) {
>   return (
>     <MDXProvider components={components}>
>       {children}
>     </MDXProvider>
>   )
> }
> ```
>
> 
>
> 然后在MDX中，我们可以直接使用`<h1>`和`<MyComponent>`，它们会被替换为我们提供的组件。
>
> ## 5. 在MDX中定义组件（不推荐）
>
> 虽然MDX不支持在顶层定义组件，但我们可以通过内联表达式和`React.createElement`来定义组件，但这通常很麻烦且不易阅读。
>
> mdx
>
> ```
> {(() => {
>   const MyComponent = () => <div>Hello from inline component</div>;
>   return <MyComponent />;
> })()}
> ```
>
> 
>
> 但是，这样定义的组件只能在当前表达式内使用。
>
> ## 6. 使用备注（注释）
>
> 我们还可以在MDX中使用JavaScript注释，它们不会渲染到页面上。
>
> mdx
>
> ```
> {/* 这是一个注释 */}
> ```
>
> 
>
> ## 总结
>
> 在MDX中，我们不能在顶层直接定义变量、函数或组件，但可以通过导入、导出和内联表达式（IIFE）来实现类似的功能。对于复杂的逻辑，建议将代码放在外部文件中，然后通过导入的方式引入。
>
> 因此，如果你需要在MDX中多次使用同一个变量或函数，最好的做法是：
>
> - 将它们定义在一个外部JavaScript/TypeScript文件中，然后导入。
> - 或者，使用MDXProvider来提供全局可用的组件。
>
> 希望这些例子能帮助你在MDX中更好地使用变量、函数和组件。

# MDX 中定义变量、函数和组件的完整指南

在 MDX 中，你**可以**定义变量、函数和组件，但需要遵循特定的语法规则。以下是详细的说明和示例：

## ✅ 可以定义的内容及方法

### 1. **变量定义**

#### 方法一：使用立即执行函数表达式 (IIFE)
```mdx
{(function() {
  const userName = 'Kimi';
  const age = 25;
  const isLoggedIn = true;
  
  return `用户: ${userName}, 年龄: ${age}, 登录状态: ${isLoggedIn}`;
})()}
```

#### 方法二：使用块作用域
```mdx
{(() => {
  const message = 'Hello World!';
  const count = 42;
  
  return (
    <div>
      <p>{message}</p>
      <p>计数: {count}</p>
    </div>
  );
})()}
```

### 2. **函数定义**

#### 方法一：IIFE 中定义函数
```mdx
{(function() {
  function formatDate(date) {
    return date.toLocaleDateString('zh-CN');
  }
  
  function calculate(a, b) {
    return a * b;
  }
  
  return (
    <div>
      <p>当前日期: {formatDate(new Date())}</p>
      <p>计算结果: {calculate(6, 7)}</p>
    </div>
  );
})()}
```

#### 方法二：箭头函数
```mdx
{(() => {
  const greet = (name) => `你好, ${name}!`;
  const double = (x) => x * 2;
  
  return (
    <div>
      <p>{greet('Kimi')}</p>
      <p>两倍: {double(21)}</p>
    </div>
  );
})()}
```

### 3. **组件定义**

#### 方法一：内联函数组件
```mdx
{function Welcome(props) {
  return <h2 style={{color: 'blue'}}>欢迎, {props.name}!</h2>;
}}

<Welcome name="Kimi" />
```

#### 方法二：箭头函数组件
```mdx
{const UserCard = ({ user, age }) => (
  <div style={{ 
    border: '1px solid #ccc', 
    padding: '10px', 
    margin: '10px',
    borderRadius: '5px'
  }}>
    <h3>{user}</h3>
    <p>年龄: {age}</p>
    <p>注册时间: {new Date().toLocaleString()}</p>
  </div>
);}

<UserCard user="Kimi" age={25} />
<UserCard user="Alice" age={30} />
```

#### 方法三：带状态的组件（使用 React Hooks）
```mdx
{function Counter() {
  const [count, setCount] = React.useState(0);
  
  return (
    <div>
      <p>当前计数: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        增加
      </button>
      <button onClick={() => setCount(count - 1)}>
        减少
      </button>
    </div>
  );
}}

<Counter />
```

### 4. **复杂逻辑和数据处理**

```mdx
{(() => {
  // 数据数组
  const users = [
    { id: 1, name: 'Kimi', role: 'admin' },
    { id: 2, name: 'Alice', role: 'user' },
    { id: 3, name: 'Bob', role: 'user' }
  ];
  
  // 过滤函数
  const filterUsers = (users, role) => 
    users.filter(user => user.role === role);
  
  // 映射函数
  const userList = filterUsers(users, 'user').map(user => 
    <li key={user.id}>{user.name}</li>
  );
  
  return (
    <div>
      <h3>用户列表:</h3>
      <ul>{userList}</ul>
      <p>总用户数: {users.length}</p>
    </div>
  );
})()}
```

### 5. **条件渲染**

```mdx
{function ConditionalExample({ isVisible, userType }) {
  const [isLoggedIn, setIsLoggedIn] = React.useState(true);
  
  return (
    <div>
      {isVisible && <p>这个内容可见</p>}
      
      {userType === 'admin' ? (
        <div style={{background: 'gold', padding: '10px'}}>
          <h3>管理员面板</h3>
          <button>管理用户</button>
        </div>
      ) : (
        <p>普通用户视图</p>
      )}
      
      {isLoggedIn ? (
        <button onClick={() => setIsLoggedIn(false)}>退出登录</button>
      ) : (
        <button onClick={() => setIsLoggedIn(true)}>登录</button>
      )}
    </div>
  );
}}

<ConditionalExample isVisible={true} userType="admin" />
```

## ❌ **不能直接定义的内容及原因**

### 1. **顶层的变量/函数声明**
```mdx
// ❌ 错误：这会破坏 MDX 解析
const name = 'Kimi';
function sayHello() { return 'Hello'; }

// ✅ 正确：必须包裹在花括号中
{(function() {
  const name = 'Kimi';
  function sayHello() { return 'Hello'; }
  return sayHello() + ' ' + name;
})()}
```

**原因**：MDX 将顶层内容解析为 Markdown 或 JSX 元素，直接的 JavaScript 语句会被当作文本处理。

### 2. **导入非组件模块（在表达式内部）**
```mdx
// ❌ 错误：不能在表达式内使用 import
{(function() {
  import { useState } from 'react'; // 语法错误
  return null;
})()}

// ✅ 正确：import 必须在文件顶层
import { useState } from 'react';

{function MyComponent() {
  const [state, setState] = useState(null);
  return <div>内容</div>;
}}
```

## 🎯 **最佳实践模式**

### 模式一：复杂逻辑封装
```mdx
{function DataProcessor({ data }) {
  // 复杂的数据处理逻辑
  const processedData = data
    .filter(item => item.active)
    .map(item => ({
      ...item,
      formattedDate: new Date(item.date).toLocaleDateString()
    }))
    .sort((a, b) => new Date(b.date) - new Date(a.date));
  
  return (
    <div>
      {processedData.map(item => (
        <div key={item.id}>
          <h4>{item.name}</h4>
          <p>日期: {item.formattedDate}</p>
        </div>
      ))}
    </div>
  );
}}

<DataProcessor data={[
  { id: 1, name: '项目A', active: true, date: '2024-01-15' },
  { id: 2, name: '项目B', active: false, date: '2024-01-20' }
]} />
```

### 模式二：工具函数集合
```mdx
{const utils = {
  formatCurrency: (amount) => `¥${amount.toFixed(2)}`,
  truncateText: (text, length) => 
    text.length > length ? text.substring(0, length) + '...' : text,
  generateId: () => Math.random().toString(36).substr(2, 9)
};}

{function ProductList({ products }) {
  return (
    <ul>
      {products.map(product => (
        <li key={utils.generateId()}>
          <strong>{utils.truncateText(product.name, 20)}</strong>
          <span> - {utils.formatCurrency(product.price)}</span>
        </li>
      ))}
    </ul>
  );
}}

<ProductList products={[
  { name: '笔记本电脑非常高端的型号', price: 5999.99 },
  { name: '无线鼠标', price: 129.50 }
]} />
```

## ⚡ **实用技巧**

### 1. **错误边界**
```mdx
{function SafeComponent({ content }) {
  try {
    return <div>{content}</div>;
  } catch (error) {
    return <div style={{color: 'red'}}>组件渲染错误</div>;
  }
}}

<SafeComponent content="正常内容" />
```

### 2. **性能优化**
```mdx
{const MemoizedComponent = React.memo(function({ data }) {
  // 昂贵的计算
  const processed = React.useMemo(() => 
    data.map(item => expensiveCalculation(item)), [data]);
  
  return <div>{processed.join(', ')}</div>;
});}

<MemoizedComponent data={[1, 2, 3, 4, 5]} />
```

## 🎪 **高级模式**

### 1. **自定义 Hook（需要 React 支持）**
```mdx
import { useState, useEffect } from 'react';

{function useApiData(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);
  
  return { data, loading };
}}

{function ApiComponent() {
  const { data, loading } = useApiData('/api/data');
  
  if (loading) return <div>加载中...</div>;
  
  return (
    <div>
      <h3>API 数据:</h3>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}}

<ApiComponent />
```

通过以上模式，你可以在 MDX 中实现几乎所有的 JavaScript 功能，同时保持代码的整洁和可维护性。记住关键原则：**所有 JavaScript 逻辑都必须包裹在 `{}` 中**。