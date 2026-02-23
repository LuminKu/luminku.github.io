> 在使用Ubuntu24系统中，命令终端的提示前缀如何自定义，请全方面且深入地进行参数的讲解和配置，再举例不同场景下推荐使用的前缀规范（并给出具体呈现），最后用一张表总结进行快速访问，给使用者几句使用建议。

要求：有丰富的emoji表情，内容要全面，讲解要透彻。
例如下面的前缀格式:
xiaxincan@xiaxincan-Lenovo:~$
xiaxincan@192.168.0.105:~$

---

# 🚀 Ubuntu 24.04 终端提示符(PS1) 终极定制指南

## 🎯 核心概念：PS1 变量

终端提示符由环境变量 `PS1`（Prompt String 1）控制，修改 `~/.bashrc` 文件即可永久生效。

```bash
# 查看当前配置
echo $PS1
```

---

## 🧩 参数详解大全

### 📊 基础信息类

| 参数 | 含义 | 示例输出 |
|:---:|:---|:---|
| `\u` | 👤 当前用户名 | `xiaxincan` |
| `\h` | 💻 主机名（短） | `xiaxincan-Lenovo` |
| `\H` | 🌐 主机名（完整FQDN） | `xiaxincan-Lenovo.local` |
| `\w` | 📂 当前完整路径 | `/home/xiaxincan/projects` |
| `\W` | 📁 当前目录名（仅最后一级） | `projects` |
| `\$` | 🔐 提示符（root显示`#`，普通用户`$`） | `$` 或 `#` |
| `\!` | 🔢 历史命令序号 | `42` |
| `\#` | 🔢 当前命令序号 | `1` |
| `\@` | 🕐 12小时制时间 | `03:15 PM` |
| `\t` | 🕐 24小时制时间（HH:MM:SS） | `15:15:30` |
| `\T` | 🕐 12小时制时间 | `03:15:30` |
| `\d` | 📅 日期（星期 月 日） | `Mon Feb 23` |
| `\D{format}` | 📅 自定义日期格式 | `\D{%Y-%m-%d}` → `2024-02-23` |

### 🎨 颜色与样式类（ANSI转义序列）

**格式结构：** `\[\e[属性;前景色;背景色m\]` 

#### 🌈 颜色代码表

| 颜色 | 前景色 | 背景色 | Emoji |
|:---:|:---:|:---:|:---:|
| 黑色 | `30` | `40` | ⬛ |
| 红色 | `31` | `41` | 🟥 |
| 绿色 | `32` | `42` | 🟩 |
| 黄色 | `33` | `43` | 🟨 |
| 蓝色 | `34` | `44` | 🟦 |
| 紫色/洋红 | `35` | `45` | 🟪 |
| 青色 | `36` | `46` | 🩵 |
| 白色 | `37` | `47` | ⬜ |

#### ✨ 文本属性

| 属性值 | 效果 | 说明 |
|:---:|:---|:---|
| `0` | 重置/正常 | 清除所有样式 |
| `1` | **粗体/高亮** | 让文字更醒目 |
| `2` | 暗淡 | 降低亮度 |
| `3` | *斜体* | 部分终端支持 |
| `4` | <u>下划线</u> | 添加下划线 |
| `5` | 闪烁 | ⚠️ 慎用，可能很刺眼 |
| `7` | 反色 | 前景背景互换 |
| `8` | 隐藏 | 文字不可见（密码输入可用） |

#### 🎭 256色扩展（现代终端）

```bash
# 格式：\[\e[38;5;颜色码m\]  # 前景色
# 格式：\[\e[48;5;颜色码m\]  # 背景色

# 常用颜色码：
# 196 = 鲜红 🔴  |  82 = 鲜绿 🟢  |  226 = 鲜黄 🟡
# 21 = 天蓝 🔵  |  201 = 粉红 💗  |  208 = 橙色 🟠
```

---

## 🔧 高级参数与技巧

### 🔄 条件判断类

| 参数 | 功能 | 场景 |
|:---|:---|:---|
| `$(command)` | 嵌入命令输出 | 显示git分支、负载等 |
| `\[` 和 `\]` | 标记非打印字符 | **必须包裹颜色代码，否则换行错乱** |
| `\n` | 换行 | 多行提示符 |
| `\r` | 回车 | 覆盖行首 |
| `\a` | 响铃 | 提示音 |
| `\e` | ESC字符 | 颜色序列起始 |
| `\\` | 反斜杠 | 显示`\`本身 |

### 🌿 实用函数片段

```bash
# 获取Git分支名 🌿
parse_git_branch() {
    git branch 2>/dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}

# 显示退出状态码 🔴
exit_status() {
    local status=$?
    if [ $status -ne 0 ]; then
        echo "❌[$status] "
    fi
}

# 电池电量 🔋（笔记本适用）
battery_status() {
    upower -i $(upower -e | grep BAT) 2>/dev/null | grep percentage | awk '{print $2}'
}

# 负载监控 📊
load_avg() {
    uptime | awk -F'load average:' '{print $2}' | cut -d, -f1
}
```

---

## 💡 场景化配置实战

### 🏠 场景一：极简主义（适合新手/清爽派）

**配置目标：** 只保留核心信息，视觉干净

```bash
# ~/.bashrc 添加
PS1='\[\e[32m\]➜ \[\e[36m\]\W\[\e[0m\] \$ '
```

**呈现效果：**
```bash
➜ ~ $ cd projects
➜ projects $ 
```

**特点：** ✅ 绿色箭头引导，青色目录，极简高效

---

### 🎨 场景二：开发者专业版（Git感知）

**配置目标：** 显示Git分支、Python虚拟环境、退出状态

```bash
# 添加到 ~/.bashrc

# 虚拟环境显示 🐍
venv_info() {
    if [[ -n "$VIRTUAL_ENV" ]]; then
        echo "🐍$(basename "$VIRTUAL_ENV") "
    fi
}

# Git分支显示 🌿
git_info() {
    local branch=$(git symbolic-ref --short HEAD 2>/dev/null)
    if [[ -n "$branch" ]]; then
        local status=$(git status --porcelain 2>/dev/null)
        if [[ -n "$status" ]]; then
            echo " 🌿\[\e[31m\]$branch*\[\e[0m\]"  # 有未提交更改显示红色
        else
            echo " 🌿\[\e[32m\]$branch\[\e[0m\]"   # 干净状态显示绿色
        fi
    fi
}

# 退出状态 🔴
last_cmd_status() {
    local code=$?
    if [ $code -ne 0 ]; then
        echo "\[\e[41;37m\] ✘ $code \[\e[0m\] "
    fi
}

PS1='$(last_cmd_status)$(venv_info)\[\e[34m\]📁 \w\[\e[0m\]$(git_info)\n\[\e[33m\]➜\[\e[0m\] '
```

**呈现效果：**
```bash
# 正常状态
📁 ~/projects/webapp 🌿main
➜ 

# 有未提交更改
📁 ~/projects/webapp 🌿develop*
➜ 

# 命令执行失败（返回码1）
 ✘ 1 📁 ~/projects/webapp 🌿main
➜ 
```

---

### 🖥️ 场景三：服务器运维版（IP显示）

**配置目标：** 远程SSH时显示IP、负载、时间

```bash
# 获取IP地址 🌐
get_ip() {
    hostname -I | awk '{print $1}'
}

# 系统负载 📊
get_load() {
    cut -d' ' -f1-3 /proc/loadavg
}

PS1='\[\e[40;33m\] 🖥️ \u\[\e[0m\]@\[\e[40;36m\]\h($(get_ip))\[\e[0m\] \[\e[35m\][$(get_load)]\[\e[0m\] \[\e[32m\]\w\[\e[0m\]\n\[\e[31m\]#\[\e[0m\] '
```

**呈现效果：**
```bash
 🖥️ xiaxincan@xiaxincan-Lenovo(192.168.0.105) [0.52 0.58 0.59] /home/xiaxincan/projects
# 
```

---

### 🌈 场景四：彩虹渐变版（视觉冲击）

**配置目标：** 每个部分不同颜色，适合宽屏

```bash
PS1='\[\e[38;5;196m\]❤\[\e[38;5;202m\] \u\[\e[38;5;208m\]@\[\e[38;5;214m\]\h \[\e[38;5;220m\]⏰ \t\[\e[38;5;82m\] 📂 \w\[\e[38;5;45m\] \$\[\e[0m\] '
```

**呈现效果：**
```bash
❤ xiaxincan@xiaxincan-Lenovo ⏰ 15:30:25 📂 /home/xiaxincan $
```

**颜色渐变：** 红→橙→黄→绿→青，像彩虹一样 🌈

---

### 🧙 场景五：魔法极客版（全功能）

**配置目标：** 包含时间、git、虚拟环境、电池、多行

```bash
# 电池信息 🔋
battery_info() {
    if command -v acpi &> /dev/null; then
        local batt=$(acpi -b 2>/dev/null | head -1)
        if [[ "$batt" == *"Charging"* ]]; then
            echo "🔌 "
        elif [[ "$batt" == *"Discharging"* ]]; then
            local percent=$(echo "$batt" | grep -oP '\d+%')
            echo "🔋$percent "
        fi
    fi
}

# 完整配置 ✨
build_ps1() {
    local RESET='\[\e[0m\]'
    local BOLD='\[\e[1m\]'
    local DIM='\[\e[2m\]'
    
    # 第一行：系统信息栏
    local top_line="${DIM}┌─[${RESET} $(battery_info)\[\e[36m\]\d \t\[\e[0m\] ${DIM}]${RESET}"
    
    # 第二行：位置信息
    local middle_line="${DIM}├─[${RESET} \[\e[33m\]\u\[\e[0m\]@\[\e[35m\]\H\[\e[0m\] ${DIM}:${RESET} \[\e[32m\]\w\[\e[0m\] ${DIM}]${RESET}"
    
    # Git信息（如果有）
    local git_line=""
    if git rev-parse --git-dir > /dev/null 2>&1; then
        local branch=$(git symbolic-ref --short HEAD 2>/dev/null)
        git_line="\n${DIM}├─[${RESET} 🌿 \[\e[31m\]$branch\[\e[0m\] ${DIM}]${RESET}"
    fi
    
    # 第三行：输入提示
    local bottom_line="\n${DIM}└─▶${RESET} "
    
    echo -e "$top_line$middle_line$git_line$bottom_line"
}

PROMPT_COMMAND='PS1="$(build_ps1)"'
```

**呈现效果：**
```bash
┌─[ 🔋85% Mon Feb 23 15:30:25 ]
├─[ xiaxincan@xiaxincan-Lenovo : /home/xiaxincan/projects/webapp ]
├─[ 🌿 feature-login ]
└─▶ 
```

---

## 📋 快速参考总表

| 类别 | 参数/代码 | 说明 | 示例 |
|:---|:---|:---|:---|
| **👤 用户** | `\u` | 用户名 | `root` |
| **🖥️ 主机** | `\h` / `\H` | 短/长主机名 | `ubuntu` / `ubuntu.local` |
| **📂 路径** | `\w` / `\W` | 完整/简短路径 | `/home/user` / `user` |
| **⏰ 时间** | `\t` / `\T` / `\@` | 24h/12h/AMPM | `14:30:00` / `02:30:00` / `02:30 PM` |
| **📅 日期** | `\d` / `\D{fmt}` | 日期/自定义 | `Mon Feb 23` / `2024-02-23` |
| **🔢 序号** | `\!` / `\#` | 历史/命令号 | `42` / `1` |
| **💲 提示符** | `\$` | #或$ | `#` (root) / `$` |
| **🎨 颜色** | `\[\e[32m\]` | 绿色开始 | 包裹文本 |
| **🔄 重置** | `\[\e[0m\]` | 恢复默认 | 必须加在末尾 |
| **📦 嵌入** | `$(cmd)` | 命令输出 | `$(date +%H)` |
| **🆕 换行** | `\n` | 新行 | 多行提示 |
| **⬛ 背景** | `\e[40m`~`47m` | 黑红绿黄蓝紫青白 | `\e[44m`=蓝底 |

### 🎨 常用颜色组合速查

| 效果 | 代码 | 用途 |
|:---|:---|:---|
| 绿色高亮 | `\[\e[1;32m\]` | 成功/正常状态 ✅ |
| 红色警告 | `\[\e[1;31m\]` | 错误/危险 ❌ |
| 黄色提醒 | `\[\e[1;33m\]` | 警告/注意 ⚠️ |
| 蓝色信息 | `\[\e[1;34m\]` | 信息/路径 ℹ️ |
| 紫色优雅 | `\[\e[1;35m\]` | 主机名/特殊 💜 |
| 青色科技 | `\[\e[1;36m\]` | 时间/科技风 🩵 |
| 黑底白字 | `\[\e[40;37m\]` | 状态栏背景 ⬛ |

---

## 💎 使用建议

> 🎯 **新手入门**：从场景一开始，逐步添加元素，避免一次性配置过于复杂
> 
> 🧪 **测试技巧**：在终端直接输入 `PS1='...'` 可实时预览，满意后再写入 `~/.bashrc`
> 
> 💾 **持久化**：修改后执行 `source ~/.bashrc` 或重新打开终端生效
> 
> 🎨 **颜色协调**：建议不超过3-4种颜色，避免彩虹爆炸 💥
> 
> 📏 **长度控制**：提示符过长会挤压输入空间，善用 `\n` 换行
> 
> 🔧 **必备包裹**：**所有颜色代码必须用 `\[` 和 `\]` 包裹**，否则换行会错乱！
> 
> 🐛 **调试方法**：如果显示异常，先 `PS1='$ '` 重置，再逐步排查

---

## 🚀 一键应用脚本

```bash
# 保存为 setup_prompt.sh 并执行
cat >> ~/.bashrc << 'EOF'

# ==========================================
# 🎨 自定义终端提示符配置
# ==========================================

# 选择你想要的场景，取消注释即可：

# 🏠 场景一：极简
# PS1='\[\e[32m\]➜ \[\e[36m\]\W\[\e[0m\] \$ '

# 🖥️ 场景三：服务器（推荐用于SSH）
PS1='\[\e[40;33m\] 🖥️ \u\[\e[0m\]@\[\e[40;36m\]\h(\$(hostname -I | awk "{print \$1}"))\[\e[0m\] \[\e[32m\]\w\[\e[0m\]\n\[\e[31m\]#\[\e[0m\] '

EOF

source ~/.bashrc
echo "✅ 提示符配置已生效！"
```

现在，你的终端就是独一无二的艺术品了！🎨✨
