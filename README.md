# 🎭 平行世界的你 (Parallel You Agent)

一个独特的 AI Agent，能够构建和维持一个持续演进的平行宇宙人格。与普通的聊天助手不同，这个 Agent 会模拟另一个平行世界里的"你"，并在每次对话中不断丰富这个世界的设定。

## ✨ 核心特性

- **持续的平行身份**：每个用户都有一个独特的平行世界身份（职业、地点、背景等）
- **动态世界构建**：基于对话内容自动生成新的世界设定和背景故事
- **深度记忆系统**：使用 LibSQL 数据库持久化存储世界设定和对话历史
- **沉浸式体验**：丰富的感官细节描述，让你真的感受到平行世界的存在
- **交互式命令行**：美观的终端界面，支持实时对话和世界管理

## 🛠️ 技术栈

- **AI 模型**: DeepSeek Chat (高性价比，支持中文)
- **框架**: Mastra AI Agent 框架
- **数据库**: LibSQL (兼容 SQLite)
- **语言**: TypeScript
- **包管理**: npm

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/Rexingleung/parallel-you-agent1.git
cd parallel-you-agent1
```

### 2. 安装依赖

```bash
npm install
```

### 3. 初始化设置

```bash
npm run setup
```

这个命令会引导你完成：
- DeepSeek API Key 配置
- 数据库选择（本地 SQLite 或 Turso 云端）
- Agent 名称自定义
- 平行世界种子设定

### 4. 开始对话

```bash
npm run chat
```

## 🎮 使用指南

### 对话命令

在聊天界面中，你可以使用以下命令：

- `/help` - 显示帮助信息
- `/profile` - 查看你的平行身份详情
- `/world` - 浏览当前的世界设定
- `/reset` - 重置平行身份
- `/stats` - 查看统计信息
- `/quit` - 退出程序

### 示例对话

```
你: 今天工作怎么样？

平行世界的你: 今天在量子实验室调试新的时间货币转换器时，
遇到了一个有趣的现象。当我将第47号量子水晶的振频调整到
3.14赫兹时，整个设备开始发出淡蓝色的光芒，空气中弥漫着
类似薄荷的清香。同事艾莉娜说这是时间粒子稳定的标志...

🎆 新发现的世界设定:1个
  📖 量子时间转换器 (technology)
     用于将时间货币在不同时间单位间转换的精密设备
```

## 🏗️ 项目结构

```
src/
├── types.ts           # TypeScript 类型定义
├── world-generator.ts # 世界设定生成器
├── memory-manager.ts  # 记忆管理系统
├── parallel-agent.ts  # 主要的 Agent 类
├── index.ts          # 程序入口
├── chat.ts           # 交互式聊天界面
└── setup.ts          # 初始化设置工具
```

## 🔧 配置选项

### 环境变量

创建 `.env` 文件：

```env
# DeepSeek API Key (获取地址: https://platform.deepseek.com/)
DEEPSEEK_API_KEY=your_api_key_here

# 数据库配置
DATABASE_URL=file:./parallel_world.db  # 本地 SQLite
# 或者使用 Turso:
# DATABASE_URL=libsql://your-db.turso.io
# DATABASE_AUTH_TOKEN=your_token

# Agent 配置
AGENT_NAME=平行世界的你
DEFAULT_WORLD_SEED=火星殖民地新雅典，2125年
```

### 数据库选择

1. **本地 SQLite** (推荐新手)
   - 简单易用，数据存储在本地文件
   - 适合个人使用和开发

2. **Turso 云端** (推荐生产)
   - 基于 LibSQL 的云端数据库
   - 支持多设备同步和协作
   - 免费额度：10万次读取/月

## 🎨 自定义和扩展

### 添加新的世界设定模板

编辑 `src/world-generator.ts` 中的 `WORLD_TEMPLATES`：

```typescript
technology: [
  {
    title: '你的新技术',
    description: '描述这个技术的功能',
    baseDetails: { key: 'value' }
  }
]
```

### 自定义身份生成

修改 `src/memory-manager.ts` 中的 `generateRandomIdentity()` 方法来添加新的职业、地点等选项。

## 🔬 高级用法

### 编程接口

```typescript
import { createParallelYouAgent } from './src/index.js';

const agent = await createParallelYouAgent();

// 与Agent对话
const result = await agent.chat('user_id', '你好，平行世界的我！');
console.log(result.response);

// 获取世界概览
const overview = await agent.getWorldOverview('user_id');
console.log(overview.worldSettings);
```

### 批量导入世界设定

```typescript
import { WorldGenerator } from './src/world-generator.js';

// 从JSON文件导入设定
const customSettings = WorldGenerator.generateFromConversation(
  "描述你的世界", 
  "AI回复", 
  existingSettings
);
```

## 🚨 注意事项

- 确保 DeepSeek API Key 有效且有足够余额
- 本地 SQLite 文件会随时间增长，定期备份
- Turso 有免费额度限制，注意使用量
- 长时间对话可能生成大量世界设定，影响性能

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 这个仓库
2. 创建你的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交你的改动 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 开启一个 Pull Request

## 📋 待办事项

- [ ] Web 界面支持
- [ ] 多用户会话管理
- [ ] 世界设定导出/导入
- [ ] 语音对话支持
- [ ] 更多 AI 模型支持
- [ ] 插件系统
- [ ] 移动端适配

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- [Mastra](https://mastra.ai/) - 优秀的 AI Agent 框架
- [DeepSeek](https://deepseek.com/) - 高质量的中文 AI 模型
- [LibSQL](https://libsql.org/) - 现代化的 SQLite 替代品

---

**享受与平行世界的自己对话的奇妙体验吧！** 🌟

如果这个项目对你有帮助，请给个 ⭐️ star！