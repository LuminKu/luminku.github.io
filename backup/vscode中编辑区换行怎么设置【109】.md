在 VS Code 中设置编辑区换行，主要通过 `editor.wordWrap` 配置项实现。以下是几种常用方式：

快速临时切换（仅当前文件）
- Windows/Linux：按 `Alt + Z`  
- macOS：按 `Option + Z`  
此操作会立即在当前编辑器中开启或关闭换行，不修改任何设置，适合临时查看长行内容（如日志、JSON）。

---

永久全局设置（推荐）
让所有文件默认自动换行：

1. 打开设置：
   - Windows/Linux：`Ctrl + ,`
   - macOS：`Cmd + ,`
2. 在搜索框输入 `word wrap`
3. 找到 `Editor: Word Wrap`，将其值设为：
   - `on`：按编辑器宽度自动换行（最常用）
   - `wordWrapColumn`：按指定列数换行（需配合 `editor.wordWrapColumn` 设置列数，如 80 或 120）
   - `bounded`：取编辑器宽度与指定列数中的较小值换行

> 修改后立即生效，无需重启。已打开的文件可能需要按一次快捷键刷新渲染。

---

配置文件方式（settings.json）
直接编辑用户级配置文件，更稳定可靠：

1. 打开命令面板：`Ctrl + Shift + P`（Windows/Linux）或 `Cmd + Shift + P`（macOS）
2. 输入 `Open Settings (JSON)` 并选择
3. 添加或修改以下行：
   ```json
   {
     "editor.wordWrap": "on"
   }
   ```
4. 保存文件即可生效。

---

注意事项
- 不要使用 `inherit` 或 `auto`：前者行为不可控，后者几乎等同于关闭。
- 换行仅影响显示，不会修改文件内容，不影响 Git 提交或编译。
- 若设置无效，检查是否被语言特定设置或工作区配置覆盖。

如需进一步优化长代码阅读体验，可结合启用 Minimap（小地图）、Code Folding（代码折叠） 等功能。