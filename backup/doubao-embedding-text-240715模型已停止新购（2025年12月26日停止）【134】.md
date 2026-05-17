**诊断结论**
您调用的 `doubao-embedding-text-240715` 模型已停止新购，当前账号下无该模型可用的推理接入点，导致调用报错。

**根因**
根据火山方舟第六批模型下线计划，`doubao-embedding-text-240715` 属于向量类下线模型，已于2025年12月26日停止新购（EOM），无法新建该模型的推理接入点。经查询，您当前账号下未创建过该模型的存量接入点，因此调用时返回不存在或无权限的错误。

> 说明：向量类模型仅有EOM阶段，不涉及服务下线（EOS），如果您之前创建过该模型的接入点，存量服务仍可正常使用。

**解决步骤**
1. **推荐方案：迁移到最新多模态向量模型（建议优先选择）**
   建议切换到升级版 `doubao-embedding-vision-251215` 模型，该模型新增图片/视频向量化能力，效果和性能更优，且完全兼容原有文本向量能力：
   | 模型名称 | 模型ID | 能力说明 |
   |---|---|---|
   | 多模态向量模型 | doubao-embedding-vision-251215 | 支持文本、图片、视频向量化，向量维度与原模型一致 |
   
   > 注意：迁移后需要重新向量化存量知识库，无需修改向量数据库结构。

2. **操作步骤**
   1. 进入[方舟控制台-开通管理](https://console.volcengine.com/ark/region:ark+cn-beijing/openManagement)，开通 `doubao-embedding-vision` 模型
   2. 进入[在线推理页面](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint)，创建该模型的推理接入点
   3. 将代码中的模型ID替换为新接入点的EP ID或预置模型ID `doubao-embedding-vision-251215`

3. **代码示例（Python）**
   ```python
   from volcenginesdkarkruntime import Ark
   
   client = Ark()
   
   response = client.embeddings.create(
       # 替换为新的模型ID或接入点EP ID
       model="doubao-embedding-vision-251215",
       input="要向量化的文本内容"
   )
   ```

如果您之前创建过 `doubao-embedding-text-240715` 的存量接入点，可以在[在线推理页面](https://console.volcengine.com/ark/region:ark+cn-beijing/endpoint)查找对应的EP ID，直接使用EP ID调用即可，存量接入点仍可正常使用。
