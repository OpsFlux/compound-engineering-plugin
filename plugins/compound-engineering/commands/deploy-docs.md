---
name: deploy-docs
description: 验证并准备 GitHub Pages 部署的文档

---
# 部署文档命令

验证文档站点并为 GitHub Pages 部署做好准备。

## 第 1 步：验证文档

运行这些检查：

```bash
# Count components
echo "Agents: $(ls plugins/compound-engineering/agents/*.md | wc -l)"
echo "Commands: $(ls plugins/compound-engineering/commands/*.md | wc -l)"
echo "Skills: $(ls -d plugins/compound-engineering/skills/*/ 2>/dev/null | wc -l)"

# Validate JSON
cat .claude-plugin/marketplace.json | jq . > /dev/null && echo "✓ marketplace.json valid"
cat plugins/compound-engineering/.claude-plugin/plugin.json | jq . > /dev/null && echo "✓ plugin.json valid"

# Check all HTML files exist
for page in index agents commands skills mcp-servers changelog getting-started; do
  if [ -f "plugins/compound-engineering/docs/pages/${page}.html" ] || [ -f "plugins/compound-engineering/docs/${page}.html" ]; then
    echo "✓ ${page}.html exists"
  else
    echo "✗ ${page}.html MISSING"
  fi
done
```


## 步骤 2：检查未提交的更改

```bash
git status --porcelain plugins/compound-engineering/docs/
```


如果有未提交的更改，则警告用户先提交。

## 步骤 3：部署说明

由于 GitHub Pages 部署需要具有特殊权限的工作流程文件，因此请提供以下说明：

### 首次设置

1. 使用 GitHub Pages 工作流程创建`.github/workflows/deploy-docs.yml`
2. 转到存储库设置 > 页面
3. 将源设置为“GitHub Actions”

### 部署

合并到`main`后，文档将自动部署。或者：

1. 转到“操作”选项卡
2. 选择“将文档部署到 GitHub Pages”
3. 单击“运行工作流程”

### 工作流程文件内容

```yaml
name: Deploy Documentation to GitHub Pages

on:
  push:
    branches: [main]
    paths:
      - 'plugins/compound-engineering/docs/**'
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with:
          path: 'plugins/compound-engineering/docs'
      - uses: actions/deploy-pages@v4
```


## 步骤 4：报告状态

提供一个总结：

```
## Deployment Readiness

✓ All HTML pages present
✓ JSON files valid
✓ Component counts match

### Next Steps
- [ ] Commit any pending changes
- [ ] Push to main branch
- [ ] Verify GitHub Pages workflow exists
- [ ] Check deployment at https://everyinc.github.io/every-marketplace/
```

