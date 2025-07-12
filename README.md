# PDF Generator Host

一个高性能的 PDF 生成服务，支持多种模板和格式，提供 RESTful API 接口。

## 🚀 特性

- **多格式支持**: 支持从 HTML、Markdown、JSON 等格式生成 PDF
- **模板引擎**: 内置模板引擎，支持自定义模板
- **批量处理**: 支持批量生成 PDF 文档
- **异步处理**: 支持异步任务队列，处理大量请求
- **水印功能**: 支持添加文字和图片水印
- **加密保护**: 支持 PDF 加密和权限设置
- **API 接口**: RESTful API 设计，易于集成
- **高性能**: 基于 Node.js，支持并发处理

## 📋 系统要求

- Node.js >= 16.0.0
- npm >= 8.0.0
- 内存 >= 2GB (推荐 4GB)

## 🔧 安装

### 1. 克隆仓库

```bash
git clone https://github.com/Joseph19820124/pdf-generator-host.git
cd pdf-generator-host
```

### 2. 安装依赖

```bash
npm install
```

### 3. 配置环境变量

复制 `.env.example` 到 `.env` 并配置：

```bash
cp .env.example .env
```

编辑 `.env` 文件：

```env
# 服务器配置
PORT=3000
HOST=localhost

# 数据库配置
DB_HOST=localhost
DB_PORT=5432
DB_NAME=pdf_generator
DB_USER=your_user
DB_PASS=your_password

# Redis 配置 (用于任务队列)
REDIS_HOST=localhost
REDIS_PORT=6379

# 存储配置
STORAGE_TYPE=local
STORAGE_PATH=./storage

# API 密钥
API_KEY=your-secret-api-key
```

### 4. 启动服务

```bash
# 开发环境
npm run dev

# 生产环境
npm start
```

## 📖 使用说明

### API 端点

#### 1. 生成 PDF

**POST** `/api/v1/generate`

请求体：
```json
{
  "type": "html",
  "content": "<h1>Hello World</h1>",
  "options": {
    "format": "A4",
    "margin": {
      "top": "1cm",
      "bottom": "1cm",
      "left": "1cm",
      "right": "1cm"
    }
  }
}
```

响应：
```json
{
  "success": true,
  "data": {
    "id": "pdf_123456",
    "url": "https://example.com/download/pdf_123456.pdf",
    "size": 102400,
    "pages": 1
  }
}
```

#### 2. 使用模板生成

**POST** `/api/v1/generate/template`

请求体：
```json
{
  "templateId": "invoice",
  "data": {
    "invoiceNumber": "INV-2024-001",
    "customerName": "张三",
    "items": [
      {
        "name": "产品 A",
        "quantity": 2,
        "price": 100
      }
    ]
  }
}
```

#### 3. 批量生成

**POST** `/api/v1/generate/batch`

请求体：
```json
{
  "tasks": [
    {
      "type": "html",
      "content": "<h1>Document 1</h1>"
    },
    {
      "type": "markdown",
      "content": "# Document 2"
    }
  ]
}
```

#### 4. 获取任务状态

**GET** `/api/v1/task/{taskId}`

响应：
```json
{
  "success": true,
  "data": {
    "id": "task_123456",
    "status": "completed",
    "progress": 100,
    "result": {
      "url": "https://example.com/download/task_123456.zip"
    }
  }
}
```

### 支持的格式

- **HTML**: 完整的 HTML 文档或片段
- **Markdown**: Markdown 格式文本
- **JSON**: 结构化数据配合模板
- **URL**: 从网页生成 PDF

### PDF 选项

```javascript
{
  format: 'A4',              // A3, A4, A5, Letter, Legal, Tabloid
  landscape: false,          // 横向/纵向
  margin: {                  // 页边距
    top: '1cm',
    bottom: '1cm',
    left: '1cm',
    right: '1cm'
  },
  printBackground: true,     // 打印背景
  displayHeaderFooter: true, // 显示页眉页脚
  headerTemplate: '<div>页眉</div>',
  footerTemplate: '<div>第 <span class="pageNumber"></span> 页</div>',
  scale: 1,                  // 缩放比例
  pageRanges: '1-5',        // 页面范围
  preferCSSPageSize: false,  // 优先使用 CSS 定义的页面大小
  waitUntil: 'networkidle0'  // 等待条件
}
```

## 🛠️ 开发

### 项目结构

```
pdf-generator-host/
├── src/
│   ├── controllers/    # 控制器
│   ├── services/       # 业务逻辑
│   ├── models/         # 数据模型
│   ├── routes/         # 路由定义
│   ├── middlewares/    # 中间件
│   ├── utils/          # 工具函数
│   └── config/         # 配置文件
├── templates/          # PDF 模板
├── storage/           # 文件存储
├── tests/             # 测试文件
├── docs/              # 文档
└── package.json
```

### 运行测试

```bash
# 运行所有测试
npm test

# 运行单元测试
npm run test:unit

# 运行集成测试
npm run test:integration

# 测试覆盖率
npm run test:coverage
```

### 代码规范

```bash
# 检查代码规范
npm run lint

# 自动修复
npm run lint:fix
```

## 🔌 集成示例

### Node.js

```javascript
const axios = require('axios');

async function generatePDF() {
  try {
    const response = await axios.post('http://localhost:3000/api/v1/generate', {
      type: 'html',
      content: '<h1>Hello World</h1>',
      options: {
        format: 'A4'
      }
    }, {
      headers: {
        'Authorization': 'Bearer your-api-key',
        'Content-Type': 'application/json'
      }
    });
    
    console.log('PDF URL:', response.data.data.url);
  } catch (error) {
    console.error('Error:', error.message);
  }
}
```

### Python

```python
import requests

def generate_pdf():
    url = "http://localhost:3000/api/v1/generate"
    headers = {
        "Authorization": "Bearer your-api-key",
        "Content-Type": "application/json"
    }
    data = {
        "type": "html",
        "content": "<h1>Hello World</h1>",
        "options": {
            "format": "A4"
        }
    }
    
    response = requests.post(url, json=data, headers=headers)
    if response.status_code == 200:
        result = response.json()
        print(f"PDF URL: {result['data']['url']}")
    else:
        print(f"Error: {response.text}")
```

## 🚀 部署

### Docker

```bash
# 构建镜像
docker build -t pdf-generator-host .

# 运行容器
docker run -d \
  -p 3000:3000 \
  -e API_KEY=your-api-key \
  --name pdf-generator \
  pdf-generator-host
```

### Docker Compose

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - API_KEY=${API_KEY}
    volumes:
      - ./storage:/app/storage
    depends_on:
      - redis
      - postgres
  
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
  
  postgres:
    image: postgres:14
    environment:
      - POSTGRES_DB=pdf_generator
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## 📊 性能优化

- 使用 Redis 缓存频繁访问的模板
- 启用 Puppeteer 集群模式处理并发请求
- 实现任务队列避免服务器过载
- 定期清理过期的 PDF 文件

## 🤝 贡献

欢迎贡献代码！请查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详情。

1. Fork 项目
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📝 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 📞 联系方式

- 作者: Joseph
- 项目链接: [https://github.com/Joseph19820124/pdf-generator-host](https://github.com/Joseph19820124/pdf-generator-host)
- 问题反馈: [Issues](https://github.com/Joseph19820124/pdf-generator-host/issues)

## 🙏 致谢

- [Puppeteer](https://github.com/puppeteer/puppeteer) - Headless Chrome Node.js API
- [PDFKit](https://github.com/foliojs/pdfkit) - JavaScript PDF 生成库
- [Handlebars](https://handlebarsjs.com/) - 模板引擎
