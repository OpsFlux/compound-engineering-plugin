# DSPy.rb LLM 提供商

## 支持的提供商

DSPy.rb 通过安装时自动加载的适配器 gem 提供跨多个 LLM 提供商的统一支持。

### 提供商概述

- **OpenAI**：GPT-4、GPT-4o、GPT-4o-mini、GPT-3.5-turbo
- **人择**：克劳德 3 系列（十四行诗、作品、俳句）、克劳德 3.5 十四行诗
- **Google Gemini**：Gemini 1.5 Pro、Gemini 1.5 Flash、其他版本
- **Ollama**：通过 OpenAI 兼容层支持本地模型
- **OpenRouter**：适用于 200 多种型号的统一多提供商 API

## 配置

### 基本设置

```ruby
require 'dspy'

DSPy.configure do |c|
  c.lm = DSPy::LM.new('provider/model-name', api_key: ENV['API_KEY'])
end
```


### OpenAI 配置

**所需宝石**：`dspy-openai`

```ruby
DSPy.configure do |c|
  # GPT-4o Mini (recommended for development)
  c.lm = DSPy::LM.new('openai/gpt-4o-mini', api_key: ENV['OPENAI_API_KEY'])

  # GPT-4o (more capable)
  c.lm = DSPy::LM.new('openai/gpt-4o', api_key: ENV['OPENAI_API_KEY'])

  # GPT-4 Turbo
  c.lm = DSPy::LM.new('openai/gpt-4-turbo', api_key: ENV['OPENAI_API_KEY'])
end
```


**环境变量**：`OPENAI_API_KEY`

### 人择配置

**所需宝石**：`dspy-anthropic`

```ruby
DSPy.configure do |c|
  # Claude 3.5 Sonnet (latest, most capable)
  c.lm = DSPy::LM.new('anthropic/claude-3-5-sonnet-20241022',
    api_key: ENV['ANTHROPIC_API_KEY'])

  # Claude 3 Opus (most capable in Claude 3 family)
  c.lm = DSPy::LM.new('anthropic/claude-3-opus-20240229',
    api_key: ENV['ANTHROPIC_API_KEY'])

  # Claude 3 Sonnet (balanced)
  c.lm = DSPy::LM.new('anthropic/claude-3-sonnet-20240229',
    api_key: ENV['ANTHROPIC_API_KEY'])

  # Claude 3 Haiku (fast, cost-effective)
  c.lm = DSPy::LM.new('anthropic/claude-3-haiku-20240307',
    api_key: ENV['ANTHROPIC_API_KEY'])
end
```


**环境变量**：`ANTHROPIC_API_KEY`

### 谷歌双子座配置

**所需宝石**：`dspy-gemini`

```ruby
DSPy.configure do |c|
  # Gemini 1.5 Pro (most capable)
  c.lm = DSPy::LM.new('gemini/gemini-1.5-pro',
    api_key: ENV['GOOGLE_API_KEY'])

  # Gemini 1.5 Flash (faster, cost-effective)
  c.lm = DSPy::LM.new('gemini/gemini-1.5-flash',
    api_key: ENV['GOOGLE_API_KEY'])
end
```


**环境变量**：`GOOGLE_API_KEY` 或 `GEMINI_API_KEY`

### 奥拉玛配置

**必需的 gem**：无（使用 OpenAI 兼容层）

```ruby
DSPy.configure do |c|
  # Local Ollama instance
  c.lm = DSPy::LM.new('ollama/llama3.1',
    base_url: 'http://localhost:11434')

  # Other Ollama models
  c.lm = DSPy::LM.new('ollama/mistral')
  c.lm = DSPy::LM.new('ollama/codellama')
end
```


**注意**：确保 Ollama 在本地运行：`ollama serve`

### OpenRouter配置

**所需的 gem**：`dspy-openai`（使用 OpenAI 适配器）

```ruby
DSPy.configure do |c|
  # Access 200+ models through OpenRouter
  c.lm = DSPy::LM.new('openrouter/anthropic/claude-3.5-sonnet',
    api_key: ENV['OPENROUTER_API_KEY'],
    base_url: 'https://openrouter.ai/api/v1')

  # Other examples
  c.lm = DSPy::LM.new('openrouter/google/gemini-pro')
  c.lm = DSPy::LM.new('openrouter/meta-llama/llama-3.1-70b-instruct')
end
```


**环境变量**：`OPENROUTER_API_KEY`

## 提供商兼容性矩阵

### 功能支持

|特色 |开放人工智能 |人择 |双子座|奥拉玛 |
|--------|--------|------------|--------|--------|
|结构化输出| ✅ | ✅ | ✅ | ✅ |
|愿景（图像）| ✅ | ✅ | ✅ | ⚠️ 有限公司 |
|图像 URL | ✅ | ❌ | ❌ | ❌ |
|工具调用 | ✅ | ✅ | ✅ |变化 |
|流媒体 | ❌ | ❌ | ❌ | ❌ |
|函数调用 | ✅ | ✅ | ✅ |变化 |

**图例**： ✅ 全力支持| ⚠️部分支持| ❌ 不支持

### 视觉能力

**图像 URL**：仅 OpenAI 支持直接 URL 引用。对于其他提供程序，将图像加载为 base64 或从文件加载。

```ruby
# OpenAI - supports URLs
DSPy::Image.from_url("https://example.com/image.jpg")

# Anthropic, Gemini - use file or base64
DSPy::Image.from_file("path/to/image.jpg")
DSPy::Image.from_base64(base64_data, mime_type: "image/jpeg")
```


**Ollama**：有限的多模式功能。检查特定型号的功能。

## 高级配置

### 自定义参数

在配置期间传递特定于提供者的参数：

```ruby
DSPy.configure do |c|
  c.lm = DSPy::LM.new('openai/gpt-4o',
    api_key: ENV['OPENAI_API_KEY'],
    temperature: 0.7,
    max_tokens: 2000,
    top_p: 0.9
  )
end
```


### 多个提供商

针对不同的任务使用不同的模型：

```ruby
# Fast model for simple tasks
fast_lm = DSPy::LM.new('openai/gpt-4o-mini', api_key: ENV['OPENAI_API_KEY'])

# Powerful model for complex tasks
powerful_lm = DSPy::LM.new('anthropic/claude-3-5-sonnet-20241022',
  api_key: ENV['ANTHROPIC_API_KEY'])

# Use different models in different modules
class SimpleClassifier < DSPy::Module
  def initialize
    super
    DSPy.configure { |c| c.lm = fast_lm }
    @predictor = DSPy::Predict.new(SimpleSignature)
  end
end

class ComplexAnalyzer < DSPy::Module
  def initialize
    super
    DSPy.configure { |c| c.lm = powerful_lm }
    @predictor = DSPy::ChainOfThought.new(ComplexSignature)
  end
end
```


### 每个请求配置

覆盖特定预测的配置：

```ruby
predictor = DSPy::Predict.new(MySignature)

# Use default configuration
result1 = predictor.forward(input: "data")

# Override temperature for this request
result2 = predictor.forward(
  input: "data",
  config: { temperature: 0.2 }  # More deterministic
)
```


## 成本优化

### 模型选择策略

1. **开发**：使用更便宜、更快的模型（gpt-4o-mini、claude-3-haiku、gemini-1.5-flash）
2. **生产简单任务**：如果质量足够，继续使用更便宜的型号
3. **生产复杂任务**：升级到功能更强大的模型（gpt-4o、claude-3.5-sonnet、gemini-1.5-pro）
4. **本地开发**：使用 Ollama 保护隐私和零 API 成本

### 成本意识设置示例

```ruby
# Development environment
if Rails.env.development?
  DSPy.configure do |c|
    c.lm = DSPy::LM.new('ollama/llama3.1')  # Free, local
  end
elsif Rails.env.test?
  DSPy.configure do |c|
    c.lm = DSPy::LM.new('openai/gpt-4o-mini',  # Cheap for testing
      api_key: ENV['OPENAI_API_KEY'])
  end
else  # production
  DSPy.configure do |c|
    c.lm = DSPy::LM.new('anthropic/claude-3-5-sonnet-20241022',
      api_key: ENV['ANTHROPIC_API_KEY'])
  end
end
```


## 特定于提供商的最佳实践

### 开放人工智能

- 使用`gpt-4o-mini`进行开发和简单任务
- 使用`gpt-4o`执行复杂的生产任务
- 最佳视觉支持，包括 URL 加载
- 优秀的函数调用能力

### 人择

- Claude 3.5 Sonnet是目前功能最强大的型号
- 非常适合复杂的推理和分析
- 强大的安全功能和有用的输出
- 图像需要 base64（不支持 URL）

### 谷歌双子座

- Gemini 1.5 Pro 用于复杂任务，Flash 用于速度
- 强大的多式联运能力
- 成本和性能的良好平衡
- 图像需要 base64

### 成为

- 最适合隐私敏感的应用程序
- 零 API 成本
- 需要本地硬件资源
- 有限的多式联运支持取决于型号
- 适合开发和测试

## 故障排除

### API 密钥问题

```ruby
# Verify API key is set
if ENV['OPENAI_API_KEY'].nil?
  raise "OPENAI_API_KEY environment variable not set"
end

# Test connection
begin
  DSPy.configure { |c| c.lm = DSPy::LM.new('openai/gpt-4o-mini',
    api_key: ENV['OPENAI_API_KEY']) }
  predictor = DSPy::Predict.new(TestSignature)
  predictor.forward(test: "data")
  puts "✅ Connection successful"
rescue => e
  puts "❌ Connection failed: #{e.message}"
end
```


### 速率限制

优雅地处理速率限制：

```ruby
def call_with_retry(predictor, input, max_retries: 3)
  retries = 0
  begin
    predictor.forward(input)
  rescue RateLimitError => e
    retries += 1
    if retries < max_retries
      sleep(2 ** retries)  # Exponential backoff
      retry
    else
      raise
    end
  end
end
```


### 型号未找到

确保安装了正确的 gem：

```bash
# For OpenAI
gem install dspy-openai

# For Anthropic
gem install dspy-anthropic

# For Gemini
gem install dspy-gemini
```

