Ollama 可以自定义安装路径，但具体方式因操作系统而异。根据最新公开资料（截至2026年5月），以下是主流平台的实现方法：

---

Windows 系统
- 安装程序自定义路径：  
  在命令行中运行安装包时，使用 `/DIR=` 参数指定目标目录。例如：
  ```cmd
  OllamaSetup.exe /DIR="D:\Ollama"
  ```
  此方法适用于 Ollama 官方提供的 `.exe` 安装包 。

- 环境变量配置（可选）：  
  若需全局使用 `ollama` 命令，需将安装目录下的 `bin` 文件夹（如 `D:\Ollama\bin`）添加到系统 `PATH` 环境变量中 。

- 模型存储路径自定义：  
  通过设置系统环境变量 `OLLAMA_MODELS` 指定模型保存位置，例如：
  ```
  OLLAMA_MODELS=D:\OllamaModels
  ```
  重启终端或系统后生效 。

---

Linux / macOS 系统
- 解压安装包到自定义目录：  
  下载 Ollama 的压缩包（如 `.tar.gz` 或 `.deb`）后，手动解压至目标路径，例如：
  ```bash
  mkdir -p /opt/ollama
  tar -xzf ollama-*.tar.gz -C /opt/ollama
  ```

- 配置环境变量：  
  编辑 shell 配置文件（如 `~/.bashrc` 或 `~/.zshrc`），添加：
  ```bash
  export PATH="/opt/ollama/bin:$PATH"
  export OLLAMA_HOME="/opt/ollama"
  ```
  然后执行 `source ~/.bashrc` 使配置生效 。

- 模型存储路径自定义：  
  通过环境变量 `OLLAMA_MODELS` 或修改配置文件 `~/.ollama/config.yaml` 指定模型目录：
  ```yaml
  storage:
    models: /mnt/data/ollama/models
  ```
  

---
在 Ollama 中自定义本地模型存放路径，主要通过设置环境变量 `OLLAMA_MODELS` 实现。以下是具体操作方法：

自定义模型存放路径步骤

- Windows 系统
  1. 右键“此电脑” → “属性” → “高级系统设置” → “环境变量”。
  2. 在“用户变量”或“系统变量”区域点击“新建”。
  3. 变量名填写：`OLLAMA_MODELS`  
     变量值填写：自定义路径（如 `D:\AI_Models\ollama`）。
  4. 点击“确定”保存，重启 Ollama 服务或终端使配置生效。
  5. （可选）若已下载模型，需将原 `.ollama\models` 下的 `blobs` 和 `manifests` 文件夹复制到新路径，否则仅新模型会存到新位置 。

- Linux / macOS 系统
  1. 打开终端，执行以下命令设置环境变量（以 Bash 为例）：
     ```bash
     export OLLAMA_MODELS="/mnt/large_disk/ollama_models"
     ```
  2. 为永久生效，将上述行添加到 shell 配置文件（如 `~/.bashrc` 或 `~/.zshrc`）：
     ```bash
     echo 'export OLLAMA_MODELS="/mnt/large_disk/ollama_models"' >> ~/.bashrc
     source ~/.bashrc
     ```
  3. 重启 Ollama 服务或重新登录终端 。

注意事项
- 路径建议：使用英文路径，避免空格和特殊字符（推荐用下划线，如 `my_models_2024`）。
- 权限要求：确保目标目录具有读写权限，Linux/macOS 下可使用 `chmod` 和 `chown` 调整 。
- 验证是否生效：运行 `echo $OLLAMA_MODELS`（Linux/macOS）或 `echo %OLLAMA_MODELS%`（Windows）查看变量值 。
- 已有模型迁移：若已下载模型，需手动迁移 `blobs` 和 `manifests` 文件夹，否则新路径仅对后续下载有效 。

其他配置方式（进阶）
- 启动参数临时指定：  
  `ollama serve --models /tmp/custom_models` 。
- 符号链接（软链接）：  
  适用于不想修改环境变量的情况，如 Linux/macOS 中使用 `ln -s` 。
- 配置文件方式：  
  编辑 `$OLLAMA_HOME/config.yaml`，添加 `model_store: default_path: "/custom/path"` 。

> 更多详情可参考官方文档：[Ollama 官方文档](https://ollama.com/docs) 。
注意事项
- 安装后需重启终端或系统，以确保环境变量和命令路径生效 。
- 模型文件体积较大（如 Qwen2.5 系列占数 GB 至数十 GB），建议将模型存储路径与程序安装路径分离，避免占用系统盘空间 。
- 若使用图形化安装程序（如 Windows），默认不提供路径选择界面，必须通过命令行参数自定义 。

如需官方最新安装指南，可访问 [Ollama 官网](https://ollama.com/)。

---

