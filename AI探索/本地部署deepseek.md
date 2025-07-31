# **DeepSeek 本地部署流程**

## **1. 安装 Ollama（基础环境）**

Ollama 是一个轻量级 AI 模型运行框架，支持 DeepSeek 等开源模型。

- **下载安装**：

  - 访问 [Ollama 官网](https://ollama.com/)，选择适合你操作系统的安装包（Windows `.exe` / Mac `.dmg`/Linux）。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ee1b9141c81240f2abc1d78c88c40f0e.png)

  ***

  ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d8daa40aa1cf4263b80235ccb6a27d2e.png)

  - 安装完成后，在终端输入 `ollama list` 或 `ollama -v` 验证是否安装成功。

## **2. 下载 DeepSeek 模型**

DeepSeek 提供多个参数版本，不同版本对硬件要求不同：  
| 模型规模 | 典型用途 | CPU 建议 | GPU 建议 | 内存 (RAM) 建议 | 磁盘空间建议 | 适用场景 |
| ---------------- | ------------ | ------------------------------------- | ------------------------------- | ----------- | ------------- | ------------------- |
| **1.5B (15 亿)** | 小型推理、轻量级任务 | 4 核以上 (Intel i5 / AMD Ryzen 5) | 可选，入门级 GPU (如 GTX 1650, 4GB 显存) | 8GB | 10GB+ SSD | 小型 NLP 任务、文本生成、简单分类 |
| **7B (70 亿)** | 中等推理、通用任务 | 6 核以上 (Intel i7 / AMD Ryzen 7) | 中端 GPU (如 RTX 3060, 12GB 显存) | 16GB | 20GB+ SSD | 中等规模 NLP、对话系统、文本分析 |
| **14B (140 亿)** | 中大型推理、复杂任务 | 8 核以上 (Intel i9 / AMD Ryzen 9) | 高端 GPU (如 RTX 3090, 24GB 显存) | 32GB | 50GB+ SSD | 复杂 NLP、多轮对话、知识问答 |
| **32B (320 亿)** | 大型推理、高性能任务 | 12 核以上 (Intel Xeon / AMD Threadripper) | 高性能 GPU (如 A100, 40GB 显存) | 64GB | 100GB+ SSD | 大规模 NLP、多模态任务、研究用途 |
| **70B (700 亿)** | 超大规模推理、研究任务 | 16 核以上 (服务器级 CPU) | 多 GPU 并行 (如 2x A100, 80GB 显存) | 128GB | 200GB+ SSD | 超大规模模型、研究、企业级应用 |
| **671B (6710 亿)** | 超大规模训练、企业级任务 | 服务器级 CPU (AMD EPYC / Intel Xeon) | 多 GPU 集群 (如 8x A100, 320GB 显存) | 256GB+ | 1TB+ NVMe SSD | 超大规模训练、企业级 AI 平台 |

- **推荐版本**：
  - **普通用户（日常对话、写作）**：`deepseek-r1:8b`（约 5GB）。
  - **高性能显卡用户（16GB+显存）**：`deepseek-r1:16b` 或更高。
- **安装命令**：
  ```bash
  ollama run deepseek-r1:8b  # 下载并运行8B版本
  ```
  首次运行会自动下载模型文件，需耐心等待。

## **3. 测试模型运行**

在终端输入问题，如：

```bash
ollama run deepseek-r1:8b
```

然后输入问题，看 AI 是否正常回应。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/99922da9aca240f1b8b462060e2fad19.png)

## **4. （可选）安装可视化界面**

如果不想用命令行交互，可安装 **Chatbox AI** 或 **Cherry Studio** 等客户端：

- **Chatbox AI 配置**：
  - 下载并安装 [Chatbox](https://chatboxai.app/zh)。
  - 在设置中选择 `Ollama API`，模型名称填写 `deepseek-r1:8b`，接口地址 `http://localhost:11434`。 （根据部署地址填写）
- **Open WebUI**
  open-webui 官方包现在要求的 **Python 版本 >= 3.11 且 < 3.13** - 安装（conda 创建新环境）
  `bash
	conda create -n openwebui python=3.11
	conda activate openwebui
	pip install open-webui
	` - 启动
  `bash 
	 open-webui serve
	 `
  启动后，在浏览器中访问 http://localhost:8080/ 即可进入 Open WebUI 界面。
  ![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f130514bd96d4cec821d7a264730e34f.png)

## **总结**

DeepSeek 本地部署的核心步骤是：  
 **安装 Ollama** → 2. **下载 DeepSeek 模型** → 3. **测试运行** → 4. **（可选）安装可视化界面**。  
这样就能在本地运行 DeepSeek，避免服务器繁忙问题，同时提高数据隐私性。
