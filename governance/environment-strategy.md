# Environment Strategy

> Tier: 2+ (dev + staging) | Tier 3+ (dev + staging + prod)

## Environment Separation

| 环境 | 用途 | 谁部署 | 数据 |
|------|------|--------|------|
| **dev** | 本地开发 | 开发者自己 | Mock / 测试数据 |
| **staging** | 预发布验证 | CI 自动 | 脱敏生产数据 |
| **prod** | 生产环境 | 手动触发 | 真实数据 |

## Configuration Management

### 禁止事项
- ❌ 生产密钥写入代码仓库
- ❌ 环境变量 `.env` 提交到 Git
- ❌ 各环境使用同一套密钥

### 必须事项
- ✅ `.env.example` 模板文件供开发者参考
- ✅ 生产密钥通过环境变量注入（CI secrets / K8s secrets / vault）
- ✅ 不同环境使用不同的 API endpoint / database URL

## Environment Parity

- Staging 必须与 Prod 使用相同的：
  - 操作系统 / 运行时版本
  - 数据库版本
  - 第三方服务版本
- Dev 可使用轻量替代（SQLite 代替 PostgreSQL、mock server 代替真实 API）

## Secret Rotation (Tier 3)

- 生产密钥定期轮换（至少每季度一次）
- 轮换后更新所有环境变量
- 旧密钥在 7 天内保留过渡期后撤销
