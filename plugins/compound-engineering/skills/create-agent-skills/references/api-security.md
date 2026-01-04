<overview>
在构建进行需要凭据（API 密钥、令牌、机密）的 API 调用的技能时，请遵循此协议以防止凭据出现在聊天中。
</overview>

<the_problem>
带有环境变量的原始curl命令公开凭据：

```bash
# ❌ BAD - API key visible in chat
curl -H "Authorization: Bearer $API_KEY" https://api.example.com/data
```


当 Claude 执行此操作时，带有扩展的 `$API_KEY` 的完整命令会出现在对话中。
</the_problem>

<the_solution>
使用 `~/.claude/scripts/secure-api.sh` - 在内部加载凭据的包装器。

<for_supported_services>
```bash
# ✅ GOOD - No credentials visible
~/.claude/scripts/secure-api.sh <service> <operation> [args]

# Examples:
~/.claude/scripts/secure-api.sh facebook list-campaigns
~/.claude/scripts/secure-api.sh ghl search-contact "email@example.com"
```

</for_supported_services>

<adding_new_services>
当构建需要 API 调用的新技能时：

1. **向包装器添加操作** (`~/.claude/scripts/secure-api.sh`)：

```bash
case "$SERVICE" in
    yourservice)
        case "$OPERATION" in
            list-items)
                curl -s -G \
                    -H "Authorization: Bearer $YOUR_API_KEY" \
                    "https://api.yourservice.com/items"
                ;;
            get-item)
                ITEM_ID=$1
                curl -s -G \
                    -H "Authorization: Bearer $YOUR_API_KEY" \
                    "https://api.yourservice.com/items/$ITEM_ID"
                ;;
            *)
                echo "Unknown operation: $OPERATION" >&2
                exit 1
                ;;
        esac
        ;;
esac
```


2. **向包装器添加配置文件支持**（如果服务需要多个帐户）：

```bash
# In secure-api.sh, add to profile remapping section:
yourservice)
    SERVICE_UPPER="YOURSERVICE"
    YOURSERVICE_API_KEY=$(eval echo \$${SERVICE_UPPER}_${PROFILE_UPPER}_API_KEY)
    YOURSERVICE_ACCOUNT_ID=$(eval echo \$${SERVICE_UPPER}_${PROFILE_UPPER}_ACCOUNT_ID)
    ;;
```


3. **使用配置文件命名将凭证占位符添加到 `~/.claude/.env`**：

```bash
# Check if entries already exist
grep -q "YOURSERVICE_MAIN_API_KEY=" ~/.claude/.env 2>/dev/null || \
  echo -e "\n# Your Service - Main profile\nYOURSERVICE_MAIN_API_KEY=\nYOURSERVICE_MAIN_ACCOUNT_ID=" >> ~/.claude/.env

echo "Added credential placeholders to ~/.claude/.env - user needs to fill them in"
```


4. **在 SKILL.md 中记录配置文件工作流程**：

```markdown
## Profile Selection Workflow

**CRITICAL:** Always use profile selection to prevent using wrong account credentials.

### When user requests YourService operation:

1. **Check for saved profile:**
   ```bash

   ~/.claude/scripts/profile-state 获取你的服务
   ```

2. **If no profile saved, discover available profiles:**
   ```bash

   〜/.claude/scripts/list-profiles yourservice
   ```

3. **If only ONE profile:** Use it automatically and announce:
   ```

   “使用 YourService 配置文件‘main’列出项目...”
   ```

4. **If MULTIPLE profiles:** Ask user which one:
   ```

   “哪个 YourService 配置文件：main、clienta 还是 clientb？”
   ```

5. **Save user's selection:**
   ```bash

   ~/.claude/scripts/profile-state 设置您的服务<selected_profile>
   ```

6. **Always announce which profile before calling API:**
   ```

   “使用 YourService 配置文件‘main’列出项目...”
   ```

7. **Make API call with profile:**
   ```bash

   ~/.claude/scripts/secure-api.sh 你的服务：<profile> 列表项
   ```

## Secure API Calls

All API calls use profile syntax:

```bash

~/.claude/scripts/secure-api.sh 你的服务：<profile> <operation> [args]

# 示例：
~/.claude/scripts/secure-api.sh 你的服务：主要列表项
~/.claude/scripts/secure-api.sh 你的服务：主要获取项目<ITEM_ID>
```

**Profile persists for session:** Once selected, use same profile for subsequent operations unless user explicitly changes it.
```

</adding_new_services>
</the_solution>

<pattern_guidelines>
<simple_get_requests>
```bash
curl -s -G \
    -H "Authorization: Bearer $API_KEY" \
    "https://api.example.com/endpoint"
```

</simple_get_requests>

<post_with_json_body>
```bash
ITEM_ID=$1
curl -s -X POST \
    -H "Authorization: Bearer $API_KEY" \
    -H "Content-Type: application/json" \
    -d @- \
    "https://api.example.com/items/$ITEM_ID"
```


用途：
```bash
echo '{"name":"value"}' | ~/.claude/scripts/secure-api.sh service create-item
```

</post_with_json_body>

<post_with_form_data>
```bash
curl -s -X POST \
    -F "field1=value1" \
    -F "field2=value2" \
    -F "access_token=$API_TOKEN" \
    "https://api.example.com/endpoint"
```

</post_with_form_data>
</pattern_guidelines>

<credential_storage>
**位置：** `~/.claude/.env`（所有技能全局，可从任何目录访问）

**格式：**
```bash
# Service credentials
SERVICE_API_KEY=your-key-here
SERVICE_ACCOUNT_ID=account-id-here

# Another service
OTHER_API_TOKEN=token-here
OTHER_BASE_URL=https://api.other.com
```


**加载到脚本中：**
```bash
set -a
source ~/.claude/.env 2>/dev/null || { echo "Error: ~/.claude/.env not found" >&2; exit 1; }
set +a
```

</credential_storage>

<best_practices>
1. **切勿在技能示例中将原始卷曲与`$VARIABLE`一起使用** - 始终使用包装器
2. **将所有操作添加到包装器** - 不要让用户弄清楚curl语法
3. **自动创建凭证占位符** - 创建技能时立即将空字段添加到`~/.claude/.env`
4. **将凭证保存在 `~/.claude/.env`** - 一个中心位置，随处可用
5. **记录每个操作** - 在 SKILL.md 中显示示例
6. **优雅地处理错误** - 检查是否缺少环境变量，显示有用的错误消息
</best_practices>

<testing>
在不暴露凭据的情况下测试包装器：

```bash
# This command appears in chat
~/.claude/scripts/secure-api.sh facebook list-campaigns

# But API keys never appear - they're loaded inside the script
```


验证凭据是否已加载：
```bash
# Check .env exists
ls -la ~/.claude/.env

# Check specific variables (without showing values)
grep -q "YOUR_API_KEY=" ~/.claude/.env && echo "API key configured" || echo "API key missing"
```

</testing>