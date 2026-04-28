遇到的 `Welcome is not defined` 错误是因为 **MDX 的作用域限制**。让我详细解释这个问题和解决方案：

## 🔍 问题根源

在 MDX 中，当你在 `{}` 中定义组件时，这个组件的**作用域仅限于该表达式内部**，无法在表达式外部访问。

```mdx
{/* 这个 Welcome 只在当前 {} 内有效 */}
{function Welcome(props) {
  return <h2 style={{color: 'blue'}}>欢迎, {props.name}!</h2>;
}}

{/* 这里无法访问 Welcome，因为它在不同的作用域 */}
<Welcome name="Kimi" />  {/* ❌ Welcome is not defined */}
```

## ✅ 解决方案

### 方案1：使用 **export** 导出组件（推荐）

```mdx
export function Welcome(props) {
  return <h2 style={{color: 'blue'}}>欢迎, {props.name}!</h2>;
}

{/* 现在可以在任何地方使用 Welcome */}
<Welcome name="Kimi" />
<Welcome name="Alice" />
```

### 方案2：使用 **const** 导出变量

```mdx
export const Welcome = ({ name }) => (
  <h2 style={{color: 'blue'}}>欢迎, {name}!</h2>
);

<Welcome name="Kimi" />
```

### 方案3：在同一作用域内定义和使用

```mdx
{/* 定义和使用都在同一个 {} 内 */}
{(function() {
  function Welcome(props) {
    return <h2 style={{color: 'blue'}}>欢迎, {props.name}!</h2>;
  }
  
  return (
    <div>
      <Welcome name="Kimi" />
      <Welcome name="Alice" />
    </div>
  );
})()}
```

## 🎯 完整的可行示例

### 定义多个组件并在全局使用

```mdx
export function Welcome(props) {
  return <h2 style={{color: 'blue'}}>欢迎, {props.name}!</h2>;
}

export const UserCard = ({ user, age }) => (
  <div style={{ border: '1px solid #ccc', padding: '10px', margin: '10px' }}>
    <h3>{user}</h3>
    <p>年龄: {age}</p>
  </div>
);

export const Counter = () => {
  const [count, setCount] = React.useState(0);
  return (
    <div>
      <p>计数: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
};

{/* 现在可以自由使用这些组件 */}
<Welcome name="Kimi" />

<UserCard user="Kimi" age={25} />
<UserCard user="Alice" age={30} />

<Counter />
```

### 定义工具函数和变量

```mdx
export const siteName = "我的网站";
export const currentYear = new Date().getFullYear();

export function formatDate(date) {
  return date.toLocaleDateString('zh-CN');
}

export const calculateTotal = (price, quantity) => price * quantity;

{/* 使用导出的变量和函数 */}
# {siteName}

<p>当前年份: {currentYear}</p>
<p>今天日期: {formatDate(new Date())}</p>
<p>总价: {calculateTotal(25, 3)}</p>
```

### 复杂组件组合

```mdx
export const Button = ({ children, onClick, variant = 'primary' }) => (
  <button 
    onClick={onClick}
    style={{
      padding: '10px 20px',
      background: variant === 'primary' ? 'blue' : 'gray',
      color: 'white',
      border: 'none',
      borderRadius: '5px'
    }}
  >
    {children}
  </button>
);

export function UserDashboard() {
  const [users, setUsers] = React.useState([
    { id: 1, name: 'Kimi', active: true },
    { id: 2, name: 'Alice', active: false }
  ]);

  const activateUser = (id) => {
    setUsers(users.map(user => 
      user.id === id ? { ...user, active: true } : user
    ));
  };

  return (
    <div>
      <h2>用户管理</h2>
      {users.map(user => (
        <div key={user.id} style={{ margin: '10px 0' }}>
          <span>{user.name} - {user.active ? '活跃' : '非活跃'}</span>
          {!user.active && (
            <Button variant="primary" onClick={() => activateUser(user.id)}>
              激活
            </Button>
          )}
        </div>
      ))}
    </div>
  );
}

{/* 使用组件 */}
<Button onClick={() => alert('点击!')}>点击我</Button>
<UserDashboard />
```

## ⚠️ 注意事项

### 1. **导入 React**
如果使用 React Hooks，需要导入 React：

```mdx
import React from 'react';

export function MyComponent() {
  const [state, setState] = React.useState(null);
  return <div>内容</div>;
}
```

### 2. **默认导出限制**
每个 MDX 文件只能有一个默认导出：

```mdx
export default function Layout({ children }) {
  return <div className="layout">{children}</div>;
}

export const ComponentA = () => <div>A</div>;  // ✅ 命名导出
export const ComponentB = () => <div>B</div>;  // ✅ 命名导出
```

### 3. **作用域最佳实践**
```mdx
{/* ❌ 避免：作用域受限 */}
{function LocalComponent() { return <div>局部</div>; }}

{/* ✅ 推荐：全局可用 */}
export function GlobalComponent() { return <div>全局</div>; }

{/* 使用 */}
<GlobalComponent />  {/* ✅ 正常工作 */}
{/* <LocalComponent /> */}  {/* ❌ 无法工作 */}
```

## 🎪 高级用法

### 组件组合模式
```mdx
export const Container = ({ children, title }) => (
  <div style={{ border: '1px solid #ddd', padding: '20px', margin: '10px' }}>
    {title && <h3>{title}</h3>}
    {children}
  </div>
);

export const Feature = ({ title, description }) => (
  <Container title={title}>
    <p>{description}</p>
  </Container>
);

{/* 使用组合 */}
<Feature 
  title="特性1" 
  description="这是一个很棒的特性" 
/>
```

### 上下文提供者模式
```mdx
import React, { createContext, useContext } from 'react';

export const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = React.useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function ThemedButton() {
  const { theme, setTheme } = useContext(ThemeContext);
  
  return (
    <button 
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
      style={{
        background: theme === 'light' ? 'white' : 'black',
        color: theme === 'light' ? 'black' : 'white'
      }}
    >
      切换主题
    </button>
  );
}

{/* 使用 */}
<ThemeProvider>
  <ThemedButton />
</ThemeProvider>
```

## 💡 总结

**关键要点：**
- 使用 `export` 关键字定义组件、函数和变量
- 避免在 `{}` 内定义需要外部访问的内容
- 每个 MDX 文件可以有多个命名导出，但只能有一个默认导出
- 导入 React 以使用 Hooks

这样你就可以在 MDX 中自由定义和使用组件了！