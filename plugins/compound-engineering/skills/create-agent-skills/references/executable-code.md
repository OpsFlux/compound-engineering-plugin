<when_to_use_scripts>
即使克劳德可以编写脚本，预制脚本也具有以下优点：
- 比生成的代码更可靠
- 保存令牌（无需在上下文中包含代码）
- 节省时间（无需生成代码）
- 确保不同用途的一致性

<execution_vs_reference>
明确克劳德是否应该：
- **执行脚本**（最常见）：“运行`analyze_form.py`以提取字段”
- **将其作为参考**（对于复杂逻辑）：“有关提取算法，请参阅`analyze_form.py`”

对于大多数实用程序脚本，首选执行。
</execution_vs_reference>

<how_scripts_work>
当 Claude 通过 bash 执行脚本时：
1. 脚本代码永远不会进入上下文窗口
2、只有脚本输出消耗token
3. 比让 Claude 生成等效代码要高效得多
</how_scripts_work>
</when_to_use_scripts>

<file_organization>
<scripts_directory>
**最佳实践**：将所有可执行脚本放置在技能文件夹内的 `scripts/` 子目录中。

```
skill-name/
├── SKILL.md
├── scripts/
│   ├── main_utility.py
│   ├── helper_script.py
│   └── validator.py
└── references/
    └── api-docs.md
```


**好处**：
- 保持技能根干净有序
- 文档和可执行代码之间的明确分离
- 所有技能的模式一致
- 易于参考：`python scripts/script_name.py`

**参考模式**：在 SKILL.md 中，使用 `scripts/` 路径引用脚本：

```bash
python ~/.claude/skills/skill-name/scripts/analyze.py input.har
```

</scripts_directory>
</file_organization>

<utility_scripts_pattern>
<example>
## 实用脚本

**analyze_form.py**：从 PDF 中提取所有表单字段

```bash
python scripts/analyze_form.py input.pdf > fields.json
```


输出格式：
```json
{
  "field_name": { "type": "text", "x": 100, "y": 200 },
  "signature": { "type": "sig", "x": 150, "y": 500 }
}
```


**validate_boxes.py**：检查重叠的边界框

```bash
python scripts/validate_boxes.py fields.json
# Returns: "OK" or lists conflicts
```


**fill_form.py**：将字段值应用到 PDF

```bash
python scripts/fill_form.py input.pdf fields.json output.pdf
```

</example>
</utility_scripts_pattern>

<solve_dont_punt>
处理错误情况而不是向克劳德下注。

<example type="good">
```python
def process_file(path):
    """Process a file, creating it if it doesn't exist."""
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        print(f"File {path} not found, creating default")
        with open(path, 'w') as f:
            f.write('')
        return ''
    except PermissionError:
        print(f"Cannot access {path}, using default")
        return ''
```

</example>

<example type="bad">
```python
def process_file(path):
    # Just fail and let Claude figure it out
    return open(path).read()
```

</example>

<configuration_values>
记录配置参数以避免“巫术常数”：

<example type="good">
```python
# HTTP requests typically complete within 30 seconds
REQUEST_TIMEOUT = 30

# Three retries balances reliability vs speed
MAX_RETRIES = 3
```

</example>

<example type="bad">
```python
TIMEOUT = 47  # Why 47?
RETRIES = 5   # Why 5?
```

</example>
</configuration_values>
</solve_dont_punt>

<package_dependencies>
<runtime_constraints>
技能在代码执行环境中运行，具有特定于平台的限制：
- **claude.ai**：可以从 npm 和 PyPI 安装软件包
- **Anthropic API**：无网络访问且无运行时包安装
</runtime_constraints>

<guidance>
在 SKILL.md 中列出所需的软件包并验证它们是否可用。

<example type="good">
安装所需的软件包：`pip install pypdf`

然后使用它：

```python
from pypdf import PdfReader
reader = PdfReader("file.pdf")
```

</example>

<example type="bad">
“使用pdf库来处理文件。”
</example>
</guidance>
</package_dependencies>

<mcp_tool_references>
如果您的技能使用 MCP（模型上下文协议）工具，请始终使用完全限定的工具名称。

<format>服务器名称：tool_name</format>

<examples>
- 使用 BigQuery:bigquery_schema 工具检索表架构。
- 使用 GitHub:create_issue 工具创建问题。
</examples>

如果没有服务器前缀，Claude 可能无法找到该工具，特别是当有多个 MCP 服务器可用时。
</mcp_tool_references>