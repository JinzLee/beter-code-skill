# 架构设计文档

## 目录结构
```
project_root/
├── src/
│   ├── module_a/    # <职责>
│   │   ├── __init__.py
│   │   └── ...
│   └── module_b/
├── tests/
└── ...
```

## 模块清单
| 模块名 | 职责 | 负责文件 | 依赖 | 并行标记 |
|--------|------|----------|------|---------|
| module_a | ... | `src/module_a/*` | 无 | ✅ 可并行 |
| module_b | ... | `src/module_b/*` | module_a | ❌ 串行 (依赖A) |

## 依赖关系图
```
module_a ──→ module_b ──→ module_c
module_a ──→ module_d (可并行于 B)
```

## 开发顺序
```
Phase 1: [可并行] module_a, module_d
Phase 2: [串行-依赖A] module_b
Phase 3: [串行-依赖B] module_c
```

## API 接口规范（如适用）
| Method | Path | Request | Response | 说明 |
|--------|------|---------|----------|------|

## 数据流
```
用户操作 → Component A → Store → API → Response → Store → Component B
```

## 技术栈清单
| 组件 | 版本 | 用途 |
|------|------|------|

## 关键设计决策
1. <决策> — 理由: <why>
