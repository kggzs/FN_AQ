# FN_AQ
飞牛NAS论坛自动签到

# FN论坛自动签到脚本-FN_AQ 签到脚本

这是一个用于FN论坛(club.fnnas.com)的自动签到脚本，可以实现自动登录、检查签到状态、执行签到并记录签到信息。

## 功能特点

- 自动登录FN论坛账号
- 检测当前签到状态
- 自动执行签到操作
- 获取并记录签到信息（最近签到时间、本月签到天数、连续签到天数等）
- 保存Cookie到本地，下次运行时优先使用Cookie登录
- 验证码自动识别功能，使用百度OCR API识别验证码
- 详细的日志记录
- 完善的错误处理和重试机制
- **GitHub Actions 自动运行支持**
- **IYUU 通知功能，签到成功/失败时自动发送通知**

## 依赖安装

脚本依赖以下Python库：

```bash
pip install requests beautifulsoup4
```

## 使用方法

### ⚠️ 重要提示

**脚本现在仅支持通过环境变量配置，不再支持在代码中直接修改配置。**

**必须设置以下环境变量：**
- `USERNAME`: FN论坛用户名（必需）
- `PASSWORD`: FN论坛密码（必需）
- `API_KEY`: 百度OCR API Key（必需，用于验证码识别）
- `SECRET_KEY`: 百度OCR Secret Key（必需，用于验证码识别）

**可选环境变量：**
- `IYUU_TOKEN`: IYUU 通知令牌（可选，用于接收签到通知）

### 本地运行

1. 确保已安装所需依赖
2. **必须通过环境变量设置配置信息**（见下方详细说明）
3. 运行脚本

```bash
python fnclub_signer.py
```

### 环境变量配置（必需）

脚本在启动前会自动检查必需的环境变量，如果未设置，脚本将无法运行。

**Windows 系统**

**方法一：命令行临时设置（CMD）**
```bash
set USERNAME=your_username
set PASSWORD=your_password
set API_KEY=your_api_key
set SECRET_KEY=your_secret_key
set IYUU_TOKEN=your_iyuu_token
python fnclub_signer.py
```

**方法二：PowerShell 临时设置**
```powershell
$env:USERNAME="your_username"
$env:PASSWORD="your_password"
$env:API_KEY="your_api_key"
$env:SECRET_KEY="your_secret_key"
$env:IYUU_TOKEN="your_iyuu_token"
python fnclub_signer.py
```

**方法三：系统环境变量（永久设置，推荐）**
1. 右键点击"此电脑" → 选择"属性"
2. 点击"高级系统设置"
3. 点击"环境变量"
4. 在"用户变量"或"系统变量"中点击"新建"
5. 添加以下变量：

   **必需变量：**
   - 变量名：`USERNAME`，变量值：`你的FN论坛用户名`
   - 变量名：`PASSWORD`，变量值：`你的FN论坛密码`
   - 变量名：`API_KEY`，变量值：`你的百度OCR API Key`
   - 变量名：`SECRET_KEY`，变量值：`你的百度OCR Secret Key`
   
   **可选变量：**
   - 变量名：`IYUU_TOKEN`，变量值：`你的IYUU Token`（用于接收签到通知）

6. 点击"确定"保存
7. **重要：** 重新打开命令行窗口，使环境变量生效

**Linux/Mac 系统**

**方法一：命令行临时设置**
```bash
export USERNAME=your_username
export PASSWORD=your_password
export API_KEY=your_api_key
export SECRET_KEY=your_secret_key
export IYUU_TOKEN=your_iyuu_token
python fnclub_signer.py
```

**方法二：永久设置（推荐）**
编辑 `~/.bashrc`（或 `~/.zshrc`）文件：
```bash
# 使用 nano 编辑器
nano ~/.bashrc

# 或使用 vim 编辑器
vim ~/.bashrc
```

在文件末尾添加：
```bash
# FN论坛签到脚本环境变量
export USERNAME="your_username"      # 必需：FN论坛用户名
export PASSWORD="your_password"      # 必需：FN论坛密码
export API_KEY="your_api_key"        # 必需：百度OCR API Key
export SECRET_KEY="your_secret_key"  # 必需：百度OCR Secret Key
export IYUU_TOKEN="your_iyuu_token"  # 可选：IYUU 通知令牌
```

保存后执行：
```bash
source ~/.bashrc
```

**方法三：使用 .env 文件（Python 需安装 python-dotenv）**
1. 在脚本目录创建 `.env` 文件
2. 添加以下内容：
```
USERNAME=your_username
PASSWORD=your_password
API_KEY=your_api_key
SECRET_KEY=your_secret_key
IYUU_TOKEN=your_iyuu_token
```
3. 安装 python-dotenv：`pip install python-dotenv`
4. 修改脚本开头，添加：`from dotenv import load_dotenv; load_dotenv()`

**注意：** 方法三需要修改脚本代码，不推荐使用。建议使用方法一或方法二。

## 环境变量说明

脚本现在仅支持通过环境变量配置，所有配置信息必须通过环境变量设置。

### 必需的环境变量

| 变量名 | 说明 | 示例 |
|--------|------|------|
| `USERNAME` | FN论坛用户名 | `your_username` |
| `PASSWORD` | FN论坛密码 | `your_password` |
| `API_KEY` | 百度OCR API Key（用于验证码识别） | `your_api_key` |
| `SECRET_KEY` | 百度OCR Secret Key（用于验证码识别） | `your_secret_key` |

### 可选的环境变量

| 变量名 | 说明 | 示例 |
|--------|------|------|
| `IYUU_TOKEN` | IYUU 通知令牌（用于接收签到通知） | `your_iyuu_token` |
| `DEBUG` | 调试模式（设置为 `1` 启用） | `1` |

### 环境变量检查

脚本在启动前会自动检查必需的环境变量：
- 如果必需的环境变量未设置，脚本会输出错误信息并退出
- 如果可选的环境变量未设置，脚本会给出提示但继续运行

**错误示例：**
```
错误：以下必需的环境变量未设置：USERNAME, PASSWORD, API_KEY, SECRET_KEY
请通过环境变量设置这些值后再运行脚本。
详细配置方法请参考 README.md 文件。
```

### 百度OCR API配置

1. 访问[百度AI开放平台](https://ai.baidu.com/)注册账号
2. 创建文字识别应用，获取API Key和Secret Key
3. **必须通过环境变量 `API_KEY` 和 `SECRET_KEY` 设置**（见上方环境变量配置说明）

### IYUU 通知配置

1. 访问 [IYUU](https://iyuu.cn/) 获取通知令牌
2. **通过环境变量 `IYUU_TOKEN` 设置**（见上方环境变量配置说明）
3. 脚本会在以下情况发送通知：
   - 签到成功时
   - 今日已签到（提醒）
   - 登录失败时
   - 签到失败时
   - 其他异常情况

通知接口说明：
- 接口URL: `https://iyuu.cn/{您的IYUU令牌}.send`
- 请求方式: POST
- Content-Type: `application/x-www-form-urlencoded; charset=UTF-8`
- 请求参数:
  - `text`: 通知标题（必填）
  - `desp`: 通知内容（必填）

## 日志说明

脚本会在同目录下创建`logs`文件夹，并生成格式为`sign_YYYYMMDD.log`的日志文件，记录签到过程中的各种信息。

### 日志优化

- 所有HTML响应内容输出时都会添加清晰的分隔符，便于查看
- 分隔符格式：`【描述 - 开始】` 和 `【描述 - 结束】`
- 使用 `=====` 作为分隔线，便于在日志中搜索

### 调试模式

可以通过设置环境变量启用调试模式，获取更详细的日志信息：

```bash
# Windows (CMD)
set DEBUG=1
python fnclub_signer.py

# Windows (PowerShell)
$env:DEBUG="1"
python fnclub_signer.py

# Linux/Mac
export DEBUG=1
python fnclub_signer.py
```

## 自动化部署

### 方式一：GitHub Actions（推荐）

使用 GitHub Actions 可以免费自动化运行签到脚本，无需自己的服务器。

#### 第一步：Fork 仓库

1. 登录 GitHub 账号
2. 访问本仓库：https://github.com/your-username/FN_AQ（替换为实际仓库地址）
3. 点击右上角的 "Fork" 按钮
4. 选择要 Fork 到的账号/组织
5. 等待 Fork 完成

#### 第二步：设置 GitHub Secrets（环境变量）

1. 进入你 Fork 后的仓库页面
2. 点击仓库顶部的 **"Settings"**（设置）选项卡
3. 在左侧菜单中选择 **"Secrets and variables"** → **"Actions"**
4. 点击右上角的 **"New repository secret"** 按钮
5. 依次添加以下 Secrets：

   **必填项：**
   
   - **Name**: `USERNAME`
     - **Value**: 你的 FN论坛用户名
   
   - **Name**: `PASSWORD`
     - **Value**: 你的 FN论坛密码
   
   **可选项（建议配置）：**
   
   - **Name**: `API_KEY`
     - **Value**: 百度OCR API Key（用于验证码识别，如果论坛需要验证码时使用）
   
   - **Name**: `SECRET_KEY`
     - **Value**: 百度OCR Secret Key（用于验证码识别）
   
   - **Name**: `IYUU_TOKEN`
     - **Value**: IYUU 通知令牌（用于接收签到通知，获取方式见下方"IYUU 通知配置"）

6. 每添加一个 Secret 后，点击 **"Add secret"** 保存
7. 重复以上步骤，直到添加完所有需要的 Secrets

**注意：**
- Secrets 是加密存储的，不会在代码中显示
- 可以随时在 Settings → Secrets 中修改或删除
- Secret 名称必须完全匹配（区分大小写）

#### 第三步：启用 GitHub Actions

1. 在仓库页面，点击顶部的 **"Actions"** 选项卡
2. 如果是第一次使用 Actions，可能会看到提示，点击 **"I understand my workflows, go ahead and enable them"**（我了解我的工作流，继续启用它们）
3. 在左侧菜单中选择 **"自动签到"** 工作流
4. 脚本将每天**北京时间 0:00**自动运行

#### 第四步：手动触发测试（可选）

1. 在 Actions 页面，选择 **"自动签到"** 工作流
2. 点击右侧的 **"Run workflow"** 按钮
3. 选择分支（默认 main 或 master）
4. 点击 **"Run workflow"** 确认
5. 等待工作流运行完成，查看运行日志

#### 查看运行日志

1. 在 Actions 页面，点击对应的运行记录
2. 点击 **"运行签到脚本"** 步骤可以查看详细日志
3. 点击 **"上传日志"** 步骤可以下载日志文件

#### 定时说明

- 默认运行时间：**每天北京时间 0:00**（UTC 16:00）
- 如需修改运行时间，编辑 `.github/workflows/auto-sign.yml` 文件中的 cron 表达式
- Cron 格式：`分 时 日 月 星期`（UTC 时间）
  - 北京时间 0:00 = UTC 16:00（前一天）= `0 16 * * *`
  - 北京时间 8:00 = UTC 0:00 = `0 0 * * *`
  - 北京时间 12:00 = UTC 4:00 = `0 4 * * *`

### 方式二：本地定时任务

可以通过Linux的crontab设置定时任务，实现每天自动签到：

```bash
# 编辑crontab
crontab -e

# 添加以下内容，设置每天上午8:30执行签到脚本
30 8 * * * cd /path/to/script && python fnclub_signer.py
```

对于Windows系统，可以使用计划任务：

1. 打开任务计划程序
2. 创建基本任务
3. 设置每天运行，并指定时间
4. 选择启动程序，并设置为python脚本路径

## 注意事项

1. 请勿频繁运行脚本，以免对网站造成不必要的压力
2. 首次运行时会创建Cookie文件，之后会优先使用Cookie登录
3. 如Cookie失效，脚本会自动尝试使用账号密码重新登录
4. 验证码识别功能需要配置有效的百度OCR API密钥才能使用
5. 脚本内置了重试机制，可以自动处理临时性错误

## 免责声明

本脚本仅供学习交流使用，请勿用于任何商业用途。使用本脚本产生的任何后果由使用者自行承担。

## 更新日志

### 2023.03.15 - 重试机制与验证码识别优化
- 添加了完善的重试机制，提高脚本稳定性
- 优化了百度OCR API的集成，实现验证码自动识别
- 添加了access_token缓存功能，减少API调用次数
- 改进了错误处理和日志记录
- 添加了调试模式支持

### 登录功能优化
- 优化了登录表单的查找逻辑，支持多种表单ID格式
- 调整了登录请求参数，确保正确提交用户名和密码
- 添加了登录状态的准确检测

### 签到信息获取改进
- 优化了签到信息区域的查找方式
- 正确解析并记录签到相关信息
- 添加了详细的日志记录

### Cookie管理完善
- 实现了Cookie的保存和加载功能
- 添加了Cookie有效性检查
- 在Cookie失效时自动使用账号密码重新登录

### 错误处理增强
- 添加了验证码检测功能
- 完善了错误日志记录
- 优化了异常情况的处理流程

### 验证码识别功能
- 在Config类中添加了百度OCR API相关配置
- 实现了recognize_captcha方法，用于下载验证码图片、转换为Base64编码并调用API识别
- 添加了验证码文本清理功能，提高识别准确率
- 该方法会返回识别出的验证码文本或在失败时返回None

### 登录流程优化
- 修改了login方法，增加了验证码检测和处理逻辑
- 当检测到需要验证码时，会自动获取验证码图片URL
- 调用recognize_captcha方法识别验证码
- 将识别结果添加到登录表单数据中
- 增加了验证码错误的检测和处理

### 登录状态检测改进
- 优化了check_login_status函数，使用多种方法综合判断登录状态
- 检查登录链接是否存在
- 检查页面中是否包含用户名
- 检查是否有个人中心链接

### Cookie管理优化
- 更新了save_cookies函数，保存完整的Cookie信息，包括域名、路径、过期时间等
- 优化了load_cookies函数，使其能够处理新旧两种格式的Cookie文件，确保向后兼容性

### GitHub Actions 支持
- 添加了 `.github/workflows/auto-sign.yml` 工作流文件
- 支持定时自动运行（每天 UTC 0:00，北京时间 8:00）
- 支持手动触发运行
- 自动上传日志文件作为 Artifact

### IYUU 通知功能
- 集成了 IYUU 通知服务
- 在签到成功、失败、异常等情况下自动发送通知
- 支持通过环境变量 `IYUU_TOKEN` 配置通知令牌
- 通知内容包含详细的签到信息和状态

[![Star History Chart](https://api.star-history.com/svg?repos=kggzs/FN_AQ&type=Date)](https://www.star-history.com/#kggzs/FN_AQ&Date)
