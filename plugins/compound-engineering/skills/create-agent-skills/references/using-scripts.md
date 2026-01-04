# 在技能中使用脚本

<purpose>
脚本是克劳德按原样运行而不是每次都重新生成的可执行代码。它们确保重复操作的可靠、无差错执行。
</purpose>

<when_to_use>
在以下情况下使用脚本：
- 相同的代码在多个技能调用中运行
- 从头重写时操作容易出错
- 涉及复杂的shell命令或API交互
- 一致性比灵活性更重要

常见的脚本类型：
- **部署** - 部署到 Vercel、发布包、推送版本
- **安装** - 初始化项目、安装依赖项、配置环境
- **API 调用** - 经过身份验证的请求、Webhook 处理程序、数据获取
- **数据处理** - 转换文件、批量操作、迁移
- **构建流程** - 编译、捆绑、测试运行程序
</when_to_use>

<script_structure>
脚本位于技能目录中的`scripts/`中：

```
skill-name/
├── SKILL.md
├── workflows/
├── references/
├── templates/
└── scripts/
    ├── deploy.sh
    ├── setup.py
    └── fetch-data.ts
```


结构良好的脚本包括：
1. 顶部有明确目的的评论
2. 输入验证
3. 错误处理
4. 尽可能进行幂等操作
5. 清晰的输出/反馈
</script_structure>

<script_example>
```bash
#!/bin/bash
# deploy.sh - Deploy project to Vercel
# Usage: ./deploy.sh [environment]
# Environments: preview (default), production

set -euo pipefail

ENVIRONMENT="${1:-preview}"

# Validate environment
if [[ "$ENVIRONMENT" != "preview" && "$ENVIRONMENT" != "production" ]]; then
    echo "Error: Environment must be 'preview' or 'production'"
    exit 1
fi

echo "Deploying to $ENVIRONMENT..."

if [[ "$ENVIRONMENT" == "production" ]]; then
    vercel --prod
else
    vercel
fi

echo "Deployment complete."
```

</script_example>

<workflow_integration>
工作流程参考脚本如下：

```xml
<process>
## Step 5: Deploy

1. Ensure all tests pass
2. Run `scripts/deploy.sh production`
3. Verify deployment succeeded
4. Update user with deployment URL
</process>
```


工作流程告诉 Claude 何时运行脚本。该脚本处理操作的执行方式。
</workflow_integration>

<best_practices>
**做：**
- 使脚本幂等（安全运行多次）
- 包括清晰的使用注释
- 执行前验证输入
- 提供有意义的错误消息
- 在 bash 脚本中使用`set -euo pipefail`

**不要：**
- 硬编码秘密或凭证（使用环境变量）
- 为一次性操作创建脚本
- 跳过错误处理
- 让脚本做太多不相关的事情
- 忘记使脚本可执行（`chmod +x`）
</best_practices>

<security_considerations>
- 切勿在脚本中嵌入 API 密钥、令牌或机密
- 使用环境变量进行敏感配置
- 验证和清理任何用户提供的输入
- 小心删除或修改数据的脚本
- 考虑为破坏性操作添加`--dry-run`选项
</security_considerations>