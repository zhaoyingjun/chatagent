# 项目文档切换

[查看 Chatbot 文档](#chatbot) | [查看 Shopping Agent 文档](#shopping-agent)

---

<span id="chatbot"></span>

# chatbot
ChatGPT带火了聊天机器人，主流的趋势都调整到了GPT类模式，本项目也与时俱进，会在近期更新更新GPT类版本。
这是一个可以使用自己语料进行训练的中文聊天机器人项目，欢迎大家实践交流以及Star、Fork。

## Seq2Seq版本效果参考（训练进度50%）
![img_1.png](img_1.png) ![img_2.png](img_2.png)

## RoadMap:
### V1.1: Update: 2024-09-30
1. 增加MindSpore版本，优先在MindSpore版本上引入GPT模型，RLHF等特性。
2. 整体工程架构分为Seq2Seq和GPT两大分支，继续保持多AI框架版本演进。

### V1.2: Update: 2024-12-30 (Maybe)
1. 实现类似mini-GPT4的功能，可以进行图文多模态的对话，主要提升趣味性和丰富性。
2. 增强分布式集群训练相关能力和RLHF等特性。

## seq2seq版本代码执行顺序
大家可以使用小黄鸡的语料，下载地址：
https://github.com/zhaoyingjun/chatbot/blob/master/chineseChatbotWeb-tf2.0/seq2seqChatbot/train_data/xiaohuangji50w_nofenci.conv

1. 在下载好代码和语料之后，将语料文件放入train_data目录下，超参配置在config/seq2seq.ini文件中配置。
2. 按照数据预处理器（data_utls.py）→ execute.py（执行器）→ app.py（可视化对话模块）的顺序执行就可以了。
3. 大规模分布式训练版本，参照horovod的启动方式：
   ```bash
   horovodrun -np n -H host1_ip:port,host2_ip:port,hostn_ip:port python3 excute.py
   ```

## 建议训练环境配置
- ubuntu==18.04  
- python==3.6  

### TF2.X:
- tensorflow==2.6.0
- flask==0.11.1
- horovod==0.24 (分布式训练)

### pytorch:
- torch==1.11.0
- flask==0.11.1

## 开源交流、联系方式
QQ：934389697

---

[切换到 Shopping Agent 文档](#shopping-agent)

---

<span id="shopping-agent"></span>

# Shopping Agent

## 概述

ShoppingAgent是一个智能订单分析Agent，能够通过浏览器自动化、截图识别和大模型分析，帮助用户快速提取、汇总、生成excel，可以处理所有无法直接导出的订单后台信息。采用FastAPI作为后端服务，Streamlit作为前端界面，借助大模型的能力，实现自动化的Agent功能，提供直观的操作体验和强大的订单处理能力。

## 功能特点

- 自然语言交互：通过聊天界面使用自然语言指令控制订单处理流程
- 浏览器自动化：自动打开网页并进行截图操作
- 灵活截图：支持全屏截图、元素选择截图和手动区域选择截图
- 批量分析：对多张订单截图进行批量处理和信息提取
- 数据导出：将分析结果导出为Excel文件
- 系统配置：可自定义服务器、模型和浏览器等参数

## 快速开始

### 前提条件

- Python 3.8+
- Google Chrome浏览器（版本140.0及以上，建议使用最新稳定版）
- 下载Chrome驱动，下载地址见dist/resourece文件夹下

### 安装步骤

1. 克隆项目代码库到本地

2. 安装依赖包：
   ```bash
   pip install -r requirements.txt
   ```

### 启动应用

1. 启动后端服务：
   ```bash
   python -m app.main
   ```

2. 打开新终端，启动前端界面：
   ```bash
   streamlit run app/frontend.py
   ```

3. 浏览器会自动打开前端界面，或手动访问 http://localhost:8501

## 使用指南

### 基本流程

1. **打开订单页面**：在URL输入框中输入订单页面地址，点击"打开网页"按钮

2. **截取订单截图**：
   - 选择截图方式（默认全屏或手动选择区域）
   - 对于手动选择区域：先获取全页截图，然后在图片上拖动鼠标选择订单区域，最后点击"截取所选区域"

3. **分析订单**：点击"批量分析订单"按钮，系统会自动处理所有截图并提取订单信息

4. **查看和导出结果**：在右侧面板查看分析结果，点击"导出为Excel"保存数据

### 智能交互

在左侧聊天窗口，可以使用自然语言指令控制系统

## 系统配置

在侧边栏选择"系统设置"，可以配置：
- 服务器地址和端口
- AI模型参数和API密钥
- Chrome浏览器和驱动路径

> 注意：配置修改后需要重启应用才能生效

## 目录结构project-root/
├── app/
│   ├── __init__.py
│   ├── main.py          # 后端服务入口
│   ├── frontend.py      # 前端界面
│   ├── config.py        # 配置文件
│   └── utils/           # 工具函数
├── screenshots/         # 截图保存目录
├── excels/              # Excel导出目录
├── .env                 # 环境变量
└── requirements.txt     # 依赖清单
## 常见问题

1. **浏览器无法启动**：
   - 检查CHROME_PATH配置是否正确
   - 确保Chrome浏览器已安装且版本为112.0及以上
   - 尝试更新Chrome浏览器到最新版本
   - Windows系统需确保Chrome安装路径不含中文或特殊字符

2. **截图失败**：
   - 检查网页是否完全加载
   - 尝试使用不同的截图方式
   - 确认浏览器窗口没有被最小化
   - 确保Chrome版本与chromedriver版本匹配

3. **分析结果不准确**：
   - 确保订单截图清晰可见
   - 尝试缩小截图区域，只包含订单信息
   - 检查API密钥和网络连接

4. **前后端连接失败**：
   - 确认后端服务已启动
   - 检查服务器地址配置是否正确
   - 确认端口没有被占用

## 依赖说明

### 基础依赖
- python-dotenv>=1.0.0    # 环境变量管理
- fastapi>=0.103.1        # 后端API框架
- uvicorn>=0.23.2         # ASGI服务器
- streamlit>=1.27.0       # 前端框架
- requests>=2.31.0        # HTTP请求
- pandas>=2.1.1           # 数据处理
- openpyxl>=3.1.2         # Excel处理
- pillow>=10.0.1          # 图像处理

### 浏览器自动化
- selenium>=4.14.0        # 浏览器自动化（兼容Chrome 112+）
- webdriver-manager>=4.0.1 # WebDriver管理（自动匹配Chrome版本）

### 图像处理与多模态理解
- python-multipart>=0.0.6 # 处理multipart表单数据

---

[切换到 Chatbot 文档](#chatbot)
