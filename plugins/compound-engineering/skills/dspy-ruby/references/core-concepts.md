# DSPy.rb 核心概念

## 哲学

DSPy.rb 使开发人员能够**对 LLM 进行编程，而不是提示它们**。无需手动制作提示，而是使用类型安全的可组合模块通过代码定义应用程序需求。

## 签名

签名定义了 LLM 操作的类型安全输入/输出合约。它们通过运行时类型检查来指定输入哪些数据和输出哪些数据。

### 基本签名结构

```ruby
class TaskSignature < DSPy::Signature
  description "Brief description of what this signature does"

  input do
    const :field_name, String, desc: "Description of this input field"
    const :another_field, Integer, desc: "Another input field"
  end

  output do
    const :result_field, String, desc: "Description of the output"
    const :confidence, Float, desc: "Confidence score (0.0-1.0)"
  end
end
```


### 类型安全

签名支持冰糕类型，包括：
- `String` - 文本数据
- `Integer`、`Float` - 数字数据
- `T::Boolean` - 布尔值
- `T::Array[Type]` - 特定类型的数组
- 自定义枚举和类

### 字段说明

始终使用 `desc:` 参数提供清晰的字段描述。这些描述：
- 指导法学硕士预期的输入/输出格式
- 作为开发者的文档
- 提高预测准确性

## 模块

模块是可组合的构建块，使用签名来执行 LLM 操作。它们可以链接在一起以创建复杂的工作流程。

### 基本模块结构

```ruby
class MyModule < DSPy::Module
  def initialize
    super
    @predictor = DSPy::Predict.new(MySignature)
  end

  def forward(input_hash)
    @predictor.forward(input_hash)
  end
end
```


### 模块组成

模块可以调用其他模块来创建管道：

```ruby
class ComplexWorkflow < DSPy::Module
  def initialize
    super
    @step1 = FirstModule.new
    @step2 = SecondModule.new
  end

  def forward(input)
    result1 = @step1.forward(input)
    result2 = @step2.forward(result1)
    result2
  end
end
```


## 预测器

预测器是获取签名并执行 LLM 推理的核心执行引擎。 DSPy.rb 提供了多种预测器类型。

### 预测

具有类型安全输入和输出的基本 LLM 推理。

```ruby
predictor = DSPy::Predict.new(TaskSignature)
result = predictor.forward(field_name: "value", another_field: 42)
# Returns: { result_field: "...", confidence: 0.85 }
```


### 思想链

自动向输出添加推理字段，提高复杂任务的准确性。

```ruby
class EmailClassificationSignature < DSPy::Signature
  description "Classify customer support emails"

  input do
    const :email_subject, String
    const :email_body, String
  end

  output do
    const :category, String  # "Technical", "Billing", or "General"
    const :priority, String  # "High", "Medium", or "Low"
  end
end

predictor = DSPy::ChainOfThought.new(EmailClassificationSignature)
result = predictor.forward(
  email_subject: "Can't log in to my account",
  email_body: "I've been trying to access my account for hours..."
)
# Returns: {
#   reasoning: "This appears to be a technical issue...",
#   category: "Technical",
#   priority: "High"
# }
```


### 反应

具有迭代推理的工具使用代理。通过允许法学硕士使用外部工具来实现自主解决问题。

```ruby
class SearchTool < DSPy::Tool
  def call(query:)
    # Perform search and return results
    { results: search_database(query) }
  end
end

predictor = DSPy::ReAct.new(
  TaskSignature,
  tools: [SearchTool.new],
  max_iterations: 5
)
```


### 代码法案

用于以编程方式解决问题的动态代码生成。需要可选的`dspy-code_act`宝石。

```ruby
predictor = DSPy::CodeAct.new(TaskSignature)
result = predictor.forward(task: "Calculate the factorial of 5")
# The LLM generates and executes Ruby code to solve the task
```


## 多式联运支持

DSPy.rb 使用统一的`DSPy::Image` 接口支持跨兼容模型的视觉功能。

```ruby
class VisionSignature < DSPy::Signature
  description "Describe what's in an image"

  input do
    const :image, DSPy::Image
    const :question, String
  end

  output do
    const :description, String
  end
end

predictor = DSPy::Predict.new(VisionSignature)
result = predictor.forward(
  image: DSPy::Image.from_file("path/to/image.jpg"),
  question: "What objects are visible in this image?"
)
```


### 图像输入方法

```ruby
# From file path
DSPy::Image.from_file("path/to/image.jpg")

# From URL (OpenAI only)
DSPy::Image.from_url("https://example.com/image.jpg")

# From base64-encoded data
DSPy::Image.from_base64(base64_string, mime_type: "image/jpeg")
```


## 最佳实践

### 1. 清晰的签名描述

始终为签名和字段提供清晰、具体的描述：

```ruby
# Good
description "Classify customer support emails into Technical, Billing, or General categories"

# Avoid
description "Classify emails"
```


### 2.类型安全

尽可能使用特定类型而不是通用字符串：

```ruby
# Good - Use enums for constrained outputs
output do
  const :category, T.enum(["Technical", "Billing", "General"])
end

# Less ideal - Generic string
output do
  const :category, String, desc: "Must be Technical, Billing, or General"
end
```


### 3.可组合架构

从简单、可重用的模块构建复杂的工作流程：

```ruby
class EmailPipeline < DSPy::Module
  def initialize
    super
    @classifier = EmailClassifier.new
    @prioritizer = EmailPrioritizer.new
    @responder = EmailResponder.new
  end

  def forward(email)
    classification = @classifier.forward(email)
    priority = @prioritizer.forward(classification)
    @responder.forward(classification.merge(priority))
  end
end
```


### 4. 错误处理

始终处理潜在的类型验证错误：

```ruby
begin
  result = predictor.forward(input_data)
rescue DSPy::ValidationError => e
  # Handle validation error
  logger.error "Invalid output from LLM: #{e.message}"
end
```


## 限制

当前需要注意的限制：
- 没有流支持（仅限单个请求处理）
- 通过 Ollama 对本地部署提供有限的多式联运支持
- 视觉功能因提供商而异（有关兼容性矩阵，请参阅providers.md）