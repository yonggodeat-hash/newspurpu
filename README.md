# newspurpu — 修改后的完整 TimesFM-3

完整模型与部署包已上传至本私有仓库的 Release：

- [完整模型发布页](https://github.com/yonggodeat-hash/newspurpu/releases/tag/v9-timesfm3-modified-top3)
- [下载单个 ZIP 部署包](https://github.com/yonggodeat-hash/newspurpu/releases/download/v9-timesfm3-modified-top3/newspurpu-timesfm3-v9-modified-top3.zip)

包内包含完整 TimesFM-3 原生核心（330,710,976 参数、445 个状态张量），已经合入既有 V9 quantile_width_09 修改，并附带原有概率头、配置、固定上游源码、三组合法号码解码、CLI/WSGI 部署代码、Dockerfile、许可证和核验记录。

ZIP 大小：1,231,871,901 字节。默认输出三组不同的完整号码。

ZIP SHA-256：`c27a418821069a6074eeeb119820bbe237c09852098a9bc4530aa6aa9bebac86`。GitHub 附件显示的摘要已与本地核对一致。

修改后核心 SHA-256：`871b43eda6a72f03dd039cf59e686121dc57b7954f14fbd69b4b7e916b1ba402`。全部445张量及ZIP内核心字节校验通过。

解压后的 README 包含启动方式。输入需要至少128期已公布历史和期号；部署环境需支持 torch.nn.RMSNorm。本轮没有继续训练或预测策略探索；新部署环境的原生前向尚待验收，既有GPU核验记录已随包保留。

误传的小模型 ZIP 已从 main 撤下，当前交付对象为上述完整 TimesFM-3。
