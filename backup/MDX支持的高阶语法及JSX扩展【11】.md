> 我们讨论的是MDX，它支持标准的Markdown语法，并且允许在Markdown中直接使用JSX（即JavaScript XML，类似于HTML的语法）以及JavaScript表达式。MDX实际上是Markdown和JSX的结合。
>
> 在MDX中，你可以使用大多数HTML标签，并且可以自定义React组件。但是，需要注意的是，MDX是在JSX的基础上运行的，因此它遵循JSX的规则。例如，所有标签必须闭合（包括自闭合标签），并且可以使用JavaScript表达式。
>
> 以下是一些MDX支持的高阶语法和特性：
>
> 1. 使用JSX：你可以在Markdown中插入任何有效的JSX代码，包括自定义的React组件。
> 2. 使用JavaScript表达式：在JSX中，你可以使用花括号`{}`来嵌入JavaScript表达式。
> 3. 导入（Import）和导出（Export）：你可以在MDX文件中使用ES模块的import和export语法，例如导入其他组件或库。
> 4. 布局（Layouts）：你可以通过导入布局组件并将其包装在MDX内容周围来为MDX文档提供布局。
> 5. 自定义组件：你可以通过导入自定义的React组件并在MDX中使用它们来扩展Markdown的功能。
>
> 关于HTML元素，MDX支持大多数标准的HTML元素，但需要注意的是，有些元素在JSX中可能有不同的属性名（例如，`class`要写成`className`，`for`要写成`htmlFor`）。此外，MDX也支持自定义组件，所以你可以使用任何你定义的或从外部库导入的组件。
>
> 下面是一些在MDX中可用的HTML元素和属性的示例，但请注意，在JSX中，属性通常使用camelCase，并且有些属性名与HTML不同：
>
> - div: 可用属性包括className, id, style, onClick等。
> - h1, h2, h3, h4, h5, h6: 可用属性同上。
> - p: 段落标签。
> - a: 链接，属性有href, target, rel等。
> - img: 图片，属性有src, alt, width, height等。
> - ul, ol, li: 列表。
> - table, thead, tbody, tr, th, td: 表格。
> - form, input, button, textarea, select, option: 表单元素。
> - 等等。
>
> 但是，MDX并不支持所有HTML属性，特别是那些在React中不被支持的属性（例如，一些SVG属性可能需要使用React的命名方式）。此外，MDX还支持事件处理属性（如onClick、onChange等）和样式属性（style属性接受一个JavaScript对象）。
>
> 另外，MDX允许使用JSX的展开属性（spread attributes），即你可以将一个对象的所有属性作为JSX元素的属性传递。
>
> 下面是一个MDX文件的示例，展示了多种高阶语法：
>
> mdx
>
> ```
> import { MyComponent } from './MyComponent'
> 
> # 这是一个标题
> 
> 这是一个段落，里面有一个<strong>加粗</strong>的文本。
> 
> 下面是一个自定义组件：
> 
> <MyComponent prop1="value1" prop2="value2" />
> 
> 下面是一个使用内联样式的div：
> 
> <div style={{ color: 'red', fontSize: 20 }}>
>   这个div的文本是红色的，字体大小是20px。
> </div>
> 
> 下面是一个按钮，带有事件处理：
> 
> <button onClick={() => alert('Hello!')}>点击我</button>
> 
> 下面使用JavaScript表达式：1 + 1 = {1+1}
> 
> 下面使用展开属性：
> 
> const props = { className: 'my-class', id: 'my-id' };
> <div {...props}>这个div有my-class和my-id</div>
> ```
>
> 
>
> 需要注意的是，MDX文件通常需要被特定的解析器（例如@mdx-js/loader）处理，以便在支持JSX的环境（如React）中使用。
>
> 另外，MDX并不支持所有的HTML原生元素和属性，因为JSX本身并不支持所有HTML属性（例如，一些非标准属性）。但是，对于绝大多数常见的HTML元素和属性，都是可以正常使用的。
>
> 最后，由于MDX允许嵌入JavaScript表达式，所以你可以使用任何有效的JavaScript代码，包括条件渲染、循环等。
>
> 希望这些信息对你有帮助！

MDX 支持丰富的 JSX 语法和 HTML 元素扩展，以下是主要支持的高阶语法：

## 🎯 JSX 元素和组件支持

### 1. 自定义组件
```mdx
import { Chart, Table } from './components'

# 数据分析

<Chart data={chartData} type="line" />
<Table rows={tableData} pagination />
```

### 2. 内联 JSX 与表达式
```mdx
当前时间：{new Date().toLocaleString()}

计算结果：{2 + 3 * 4}

条件渲染：{isVisible && <span>显示的内容</span>}
```

## 🎨 样式和布局组件

### 1. 容器和布局
```mdx
<div className="grid grid-cols-2 gap-4">
  <div>左侧内容</div>
  <div>右侧内容</div>
</div>

<section style={{ padding: '20px', background: '#f5f5f5' }}>
  带样式的区块
</section>
```

### 2. 高级文本样式
```mdx
<text fontSize="24px" fontWeight="bold" color="#ff0000">
  自定义文本
</text>

<p className="text-primary font-mono text-lg">
  带样式的段落
</p>
```

## 🔧 交互式组件

### 1. 按钮和交互
```mdx
<button 
  onClick={() => alert('点击了!')}
  style={{ 
    padding: '10px 20px', 
    background: 'blue', 
    color: 'white',
    border: 'none',
    borderRadius: '5px'
  }}
>
  点击我
</button>
```

### 2. 表单元素
```mdx
<input 
  type="text" 
  placeholder="输入内容"
  onChange={(e) => setValue(e.target.value)}
  value={inputValue}
/>

<select onChange={(e) => handleSelect(e.target.value)}>
  <option value="1">选项1</option>
  <option value="2">选项2</option>
</select>
```

## 📊 媒体和嵌入内容

### 1. 图片和视频
```mdx
<img 
  src="/image.jpg" 
  alt="描述文字"
  width="400"
  height="300"
  loading="lazy"
/>

<video controls width="100%">
  <source src="/video.mp4" type="video/mp4" />
</video>
```

### 2. 嵌入内容
```mdx
<iframe 
  src="https://example.com" 
  width="100%" 
  height="400"
  frameBorder="0"
/>

<embed 
  src="/document.pdf" 
  type="application/pdf"
  width="100%"
  height="600"
/>
```

## 🎪 高级布局组件

### 1. 网格和弹性布局
```mdx
<div className="flex justify-between items-center">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>

<div className="grid grid-cols-3 md:grid-cols-6 gap-2">
  {items.map(item => <div key={item.id}>{item.name}</div>)}
</div>
```

### 2. 卡片和面板
```mdx
<Card 
  title="标题"
  footer={<button>操作</button>}
  className="shadow-lg"
>
  卡片内容
</Card>

<Accordion>
  <AccordionItem title="部分1">
    折叠内容1
  </AccordionItem>
  <AccordionItem title="部分2">
    折叠内容2
  </AccordionItem>
</Accordion>
```

## 🔮 动态内容

### 1. 状态管理
```mdx
import { useState, useEffect } from 'react'

function Counter() {
  const [count, setCount] = useState(0)
  
  return (
    <div>
      计数: {count}
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  )
}

<Counter />
```

### 2. 数据获取
```mdx
function DataFetcher() {
  const [data, setData] = useState(null)
  
  useEffect(() => {
    fetch('/api/data')
      .then(res => res.json())
      .then(setData)
  }, [])
  
  return data ? <pre>{JSON.stringify(data, null, 2)}</pre> : <div>加载中...</div>
}

<DataFetcher />
```

## 📋 支持的 HTML 元素及属性

### 常用元素及扩展属性：
```mdx
<!-- 文本相关 -->
<h1 id="custom-id" className="title" data-custom="value">标题</h1>
<p style={{ lineHeight: 1.6 }} data-testid="paragraph">段落</p>
<blockquote cite="来源" className="custom-quote">引用</blockquote>

<!-- 列表 -->
<ul className="list-disc list-inside">
  <li data-index="1">项目1</li>
  <li data-index="2">项目2</li>
</ul>

<!-- 表格 -->
<table border="1" cellPadding="8" className="styled-table">
  <thead>
    <tr>
      <th width="200">列1</th>
      <th width="300">列2</th>
    </tr>
  </thead>
</table>

<!-- 链接 -->
<a 
  href="/page" 
  target="_blank" 
  rel="noopener noreferrer"
  className="link"
  title="链接标题"
>
  链接文本
</a>
```

### 表单元素完整属性：
```mdx
<input
  type="text|email|password|number|date|checkbox|radio"
  name="fieldName"
  value={value}
  defaultValue="初始值"
  placeholder="占位符"
  required
  disabled
  readOnly
  min="0"
  max="100"
  step="1"
  pattern="[A-Za-z]{3}"
  autoComplete="on|off"
  autoFocus
/>

<textarea
  rows="4"
  cols="50"
  wrap="hard|soft"
  maxLength="500"
  onChange={handleChange}
/>
```

## 🎭 特殊组件模式

### 1. 插槽模式
```mdx
<Layout
  header={<Header title="页面标题" />}
  sidebar={<Navigation items={navItems} />}
  footer={<Footer copyright="2024" />}
>
  主要内容区域
</Layout>
```

### 2. 条件渲染
```mdx
{isLoggedIn ? (
  <Dashboard user={currentUser} />
) : (
  <LoginForm onLogin={handleLogin} />
)}

{hasItems && (
  <ItemList items={filteredItems} />
)}
```

### 3. 循环渲染
```mdx
{posts.map(post => (
  <Article 
    key={post.id}
    title={post.title}
    content={post.content}
    date={post.date}
  />
))}
```

## ⚡ 性能优化属性

```mdx
<img 
  src="/image.jpg" 
  loading="lazy" 
  decoding="async"
  alt="优化图片"
/>

<video preload="metadata" controlsList="nodownload">
  <source src="/video.mp4" type="video/mp4" />
</video>
```

这些高阶语法让 MDX 超越了传统 Markdown，提供了接近完整 React 应用的开发体验，同时保持了内容的可读性和易写性。
