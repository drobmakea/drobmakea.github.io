## 获得 OpenAI API Key

### 获得 OpenAI 账号

如果您之前使用过 ChatGPT，那么 ChatGPT 的账号就是您的 OpenAI 账号，可以直接使用。

如果没有账号，可以通过以下方式注册：

- 由于国内访问限制，注册可能较为复杂。可以参考网上的注册教程。
- 如果觉得麻烦，可以通过第三方平台购买账号，或者直接购买带额度的 API Key。

### 获取 OpenAI API Key

1. 登录 OpenAI 后，鼠标移动到页面左侧，弹出侧边栏。
2. 点击侧边栏中的 **“API Keys”** 进入 API Keys 页面。
   ![API Key 页面](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)
3. 点击 **“Create new secret key”** 创建新的 API Key，命名后点击确认。
4. 弹出的对话框中会显示新创建的 Key，请务必立即保存，因为关闭对话框后将无法再次查看。
5. 保存完成后，点击 **Done**，即可在页面中看到新创建的 API Key。

---

## 获取 API 使用额度

### 额度查询

1. 点击侧边栏中的 **“Usage”** 进入使用页面。
2. 页面左侧显示每日花费，右侧显示额度。
   ![额度页面](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)
3. 在右侧 **Credit Grants** 区域：
   - 灰色：未使用额度。
   - 绿色：已使用额度。
   - 红色：已过期额度。
4. 只有未使用的额度（灰色状态）才能成功调用 API。

### 额度充值

1. 点击侧边栏中的 **“Setting”** 下的 **“Billing”** 进入账单页面。
2. 在该页面中可以管理充值相关事项：
   - 添加支付方式：点击 **Payment methods**，添加 Visa 或其他支持的支付方式。
   - 如果国内 Visa 卡无法使用，可以尝试国外卡或虚拟卡。
3. 推荐使用 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard) 提供的虚拟卡，开卡费 $15.99，充值手续费 3.5%，操作便捷。
4. 添加支付方式后，回到 **Overview** 页面，点击 **Add to credit balance** 即可充值。
5. 充值完成后，回到 **Usage** 页面即可查看可用额度的变化。

---

## Python 使用测试

### 配置 Python 环境

- Python 版本需为 3.7.1 以上。
- 推荐使用 Anaconda 创建虚拟环境以便管理依赖。

### 安装 OpenAI 库

bash
pip install openai


### 设置 API Key

OpenAI 默认从环境变量中读取 `OPENAI_API_KEY`，设置方法如下：

1. **为所有项目设置**  
   在系统环境变量中添加 `OPENAI_API_KEY`。完成后可通过命令行运行以下命令检查是否设置成功：
   bash
   echo %OPENAI_API_KEY%
   

2. **为单个项目设置**  
   在项目根目录创建 `.env` 文件（需在 `.gitignore` 中忽略），内容如下：
   env
   OPENAI_API_KEY=你的APIKey
   

### 发送请求测试

以下是一个简单的 GPT-3.5 Chat 请求示例：

python
import os
import openai
from dotenv import load_dotenv

load_dotenv()

openai.api_key = os.getenv("OPENAI_API_KEY")

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a poetic assistant, skilled in explaining complex programming concepts with creative flair."},
        {"role": "user", "content": "Compose a poem that explains the concept of recursion in programming."}
    ]
)

print(response['choices'][0]['message']['content'])


运行后，您可以在 [Usage 页面](https://platform.openai.com/usage) 查看请求的花费和 Token 数量（可能有延迟）。

---

## 功能介绍（以 Python 为例）

### 文本生成（Text Generation）

官方教程：[OpenAI 文本生成指南](https://platform.openai.com/docs/guides/text-generation)

OpenAI 模型可以理解语言（GPT-4 还支持图像输入），并返回文字结果。以下是一个简单的对话示例：

python
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Who won the world series in 2020?"},
        {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
        {"role": "user", "content": "Where was it played?"}
    ]
)

print(response['choices'][0]['message']['content'])


#### 输入说明

- **messages**：一个消息对象的列表，每个对象包含以下字段：
  - **role**：
    - `system`（可选）：设置 AI 的行为。
    - `user`：用户输入。
    - `assistant`：AI 的回复。
  - **content**：消息内容。

#### 输出说明

- **finish_reason**：表示回复的完成原因：
  - `stop`：正常完成。
  - `length`：达到最大 Token 限制。
  - `content_filter`：内容被过滤。
  - `null`：尚未完成。

---

### 图像输入（Image Input）

GPT-4 的 Vision 版本支持图像输入。在 `user` 消息的 `content` 中添加 `type` 为 `image_url` 的图像 URL 即可。

示例图像：
![示例图像](data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7)

输出示例：
text
这张图片展示了一个现代化城市的黄昏景象。天边的云朵被夕阳映照得呈现出橙色与蓝色的层次，远处的天空还带有一抹粉红。城市的天际线由多座高层建筑构成，其中几座摩天大楼高耸入云，灯火辉煌。前景是繁忙的立交桥和公路，车辆的灯光勾勒出道路的轮廓，形成光的流动。整个场景透露出城市在夜幕降临时的宁静与活力并存的氛围。


---

### 图像生成（Image Generation）

OpenAI 还支持通过 API 生成图像，具体使用方法请参考官方文档。

---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)