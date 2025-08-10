# 🛠️ API 文档

## 核心类和接口

### ParallelYouAgent

主要的 AI Agent 类，提供所有核心功能。

#### 构造函数

```typescript
const agent = new ParallelYouAgent({
  databaseUrl: string,        // 数据库连接URL
  authToken?: string,         // 数据库认证令牌（可选）
  deepseekApiKey: string,     // DeepSeek API密钥
  agentName?: string          // Agent名称（可选）
});
```

#### 主要方法

##### initialize()
初始化 Agent 和数据库。

```typescript
await agent.initialize();
```

##### chat(userId, message)
处理用户消息并生成回复。

```typescript
const result = await agent.chat('user_123', '你好！');

// 返回值结构
{
  response: string,           // AI回复内容
  worldUpdates: WorldSetting[], // 新生成的世界设定
  userProfile: UserProfile    // 更新后的用户档案
}
```

##### getWorldOverview(userId)
获取用户的平行世界完整概览。

```typescript
const overview = await agent.getWorldOverview('user_123');

// 返回值结构
{
  userProfile: UserProfile,
  worldSettings: { [category: string]: WorldSetting[] },
  conversationStats: any
}
```

##### resetParallelIdentity(userId, newSeed?)
重置用户的平行身份。

```typescript
const newProfile = await agent.resetParallelIdentity('user_123', '新世界种子');
```

### MemoryManager

记忆管理系统，负责数据存储和检索。

#### 主要方法

```typescript
// 获取用户档案
const profile = await memoryManager.getUserProfile(userId);

// 保存世界设定
await memoryManager.saveWorldSettings(settings);

// 搜索世界设定
const results = await memoryManager.searchWorldSettings('量子', 10);

// 获取对话历史
const conversations = await memoryManager.getRecentConversations(userId, 5);
```

### WorldGenerator

世界设定生成器，创建和扩展平行世界内容。

#### 静态方法

```typescript
// 生成初始世界设定
const initialSettings = WorldGenerator.generateInitialWorld('火星殖民地');

// 基于对话生成新设定
const newSettings = WorldGenerator.generateFromConversation(
  userMessage,
  aiResponse,
  existingSettings
);
```

## 数据类型

### UserProfile

```typescript
interface UserProfile {
  id: string;
  name?: string;
  parallelIdentity: {
    profession: string;      // 职业
    location: string;        // 地点
    age?: number;           // 年龄
    background: string;      // 背景故事
    personality: string[];   // 性格特点
    relationships: Record<string, string>; // 人际关系
    goals: string[];        // 人生目标
  };
  createdAt: number;
  lastActiveAt: number;
  conversationCount: number;
}
```

### WorldSetting

```typescript
interface WorldSetting {
  id: string;
  category: 'technology' | 'culture' | 'history' | 'geography' | 'economy' | 'society' | 'personal';
  title: string;           // 设定标题
  description: string;     // 详细描述
  details: Record<string, any>; // 具体细节
  timestamp: number;       // 创建时间
  importance: number;      // 重要性(1-10)
  tags: string[];         // 标签
}
```

### ConversationMemory

```typescript
interface ConversationMemory {
  id: string;
  userId: string;
  message: string;         // 用户消息
  response: string;        // AI回复
  worldUpdates: string[];  // 相关的世界设定ID
  timestamp: number;
  mood?: string;          // 对话情绪
  context?: Record<string, any>; // 上下文信息
}
```

## 使用示例

### 基础使用

```typescript
import { createParallelYouAgent } from './src/index.js';

async function main() {
  // 创建Agent实例
  const agent = await createParallelYouAgent();
  
  // 开始对话
  const result = await agent.chat('user_123', '今天天气怎么样？');
  console.log('AI回复:', result.response);
  
  // 查看新生成的世界设定
  if (result.worldUpdates.length > 0) {
    console.log('新世界设定:', result.worldUpdates);
  }
}
```

### 高级用法

```typescript
import { ParallelYouAgent, MemoryManager, WorldGenerator } from './src/index.js';

async function advancedUsage() {
  // 手动创建组件
  const memoryManager = new MemoryManager('file:./my_world.db');
  await memoryManager.initialize();
  
  const agent = new ParallelYouAgent({
    databaseUrl: 'file:./my_world.db',
    deepseekApiKey: 'your_api_key',
    agentName: '我的专属平行世界'
  });
  
  await agent.initialize();
  
  // 批量导入自定义世界设定
  const customSettings = [
    {
      id: 'custom_1',
      category: 'technology' as const,
      title: '反重力飞板',
      description: '个人交通工具',
      details: { speed: '100km/h', energy: '太阳能' },
      timestamp: Date.now(),
      importance: 8,
      tags: ['交通', '科技', '环保']
    }
  ];
  
  await memoryManager.saveWorldSettings(customSettings);
  
  // 多轮对话
  const userId = 'advanced_user';
  
  let result1 = await agent.chat(userId, '介绍一下你的工作');
  console.log(result1.response);
  
  let result2 = await agent.chat(userId, '你使用什么交通工具上班？');
  console.log(result2.response);
  
  // 分析对话统计
  const overview = await agent.getWorldOverview(userId);
  console.log(`已创建 ${Object.values(overview.worldSettings).flat().length} 个世界设定`);
}
```

### 批处理操作

```typescript
async function batchOperations() {
  const agent = await createParallelYouAgent();
  
  // 批量处理多个用户的对话
  const users = ['user_1', 'user_2', 'user_3'];
  const messages = ['你好', '今天过得怎么样？', '明天有什么计划？'];
  
  const results = await Promise.all(
    users.map((userId, index) => 
      agent.chat(userId, messages[index])
    )
  );
  
  // 分析所有用户的世界设定
  const overviews = await Promise.all(
    users.map(userId => agent.getWorldOverview(userId))
  );
  
  overviews.forEach((overview, index) => {
    console.log(`用户 ${users[index]} 的平行身份:`, overview.userProfile.parallelIdentity);
  });
}
```

## 环境变量配置

```env
# 必需配置
DEEPSEEK_API_KEY=your_deepseek_api_key

# 数据库配置
DATABASE_URL=file:./parallel_world.db
DATABASE_AUTH_TOKEN=  # Turso专用，本地SQLite留空

# 可选配置
AGENT_NAME=平行世界的你
DEFAULT_WORLD_SEED=火星殖民地新雅典，2125年
```

## 错误处理

```typescript
try {
  const result = await agent.chat(userId, message);
  console.log(result.response);
} catch (error) {
  if (error.message.includes('API key')) {
    console.error('API密钥错误');
  } else if (error.message.includes('database')) {
    console.error('数据库连接失败');
  } else {
    console.error('未知错误:', error);
  }
}
```

## 性能优化建议

1. **数据库索引**: 已自动创建必要索引，大量数据时考虑添加自定义索引
2. **对话长度**: 建议每次对话控制在800字符以内以获得最佳响应速度
3. **内存管理**: 长期运行时定期清理过期的对话记录
4. **批处理**: 对于多用户场景，使用批处理API提高效率

## 扩展开发

### 自定义世界生成器

```typescript
class CustomWorldGenerator extends WorldGenerator {
  static generateMagicWorldSettings(theme: string): WorldSetting[] {
    // 自定义的魔法世界生成逻辑
    return [];
  }
}
```

### 插件系统

```typescript
interface ParallelWorldPlugin {
  name: string;
  beforeChat?(message: string): string;
  afterChat?(response: string): string;
  onWorldUpdate?(settings: WorldSetting[]): void;
}

// 使用插件
agent.use(new MyCustomPlugin());
```