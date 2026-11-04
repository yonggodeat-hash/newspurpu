# newspurpu 修改后模型与部署包

本仓库保存已冻结的当前模型交付包，默认加载修改后的参数，每次输出三组不同的合法双色球完整组合。

## 下载

- [模型与部署包](newspurpu-v14-modified-top3.zip)
- [SHA-256 校验值](SHA256SUMS)

ZIP 包含当前假设二有限贝叶斯概率混合模型的完整推理参数（`checkpoint/edited.npz` 和必需概率基底）、Python 源码、CLI、HTTP/WSGI 接口、Dockerfile、依赖声明和说明。

当前模型为 5,000 参数的小型概率模型，其中 2,790 项已直接改写。CPU 即可运行。模型身份和检查点记录见包内 `model.json`；本交付包对应该小型模型。

## 启动

```bash
unzip newspurpu-v14-modified-top3.zip
cd newspurpu-v14-modified-top3
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python verify_release.py
python -m deploy.cli --input deploy/synthetic_history.json
```

示例文件为合成历史，仅用于工程检查。真实推理需要按时间先后提供至少 128 期已公布的双色球开奖，每行六红一蓝。详细输入与部署说明见包内 README。

本地解压加载及接口工程检查已通过；尚未证实真实彩票预测优势。模型概率不是实测中奖率。探索已按用户要求停止，当前仓库用于交付与部署。

ZIP SHA-256：`0602b936c94d1b1e0ee91862b13acb74948c7a25a79a29a40488b6c97189aef0`。
