# Introducing GPT-5.4 mini and nano

*Published: March 18, 2026 | Source: OpenAI Blog*

## What It Is

  GPT-4.1 mini and GPT-4.1 nano are the latest entries in OpenAI's growing family of efficient, cost-optimized language models. Building on the architectural advances of the GPT-4.1 series, these two models are purpose-built for speed, affordability, and high-throughput deployment scenarios. They represent OpenAI's continued push toward making powerful AI accessible at scale — not just for large enterprises with deep pockets, but for developers, startups, and embedded applications running under tight latency and cost constraints.

  **GPT-4.1 mini** is designed as a balanced powerhouse — smaller and faster than the full GPT-4.1 model, yet capable enough to handle sophisticated coding tasks, tool use (function calling), and multimodal reasoning involving text and images. It sits in the "efficient but capable" tier of OpenAI's model lineup.

  **GPT-4.1 nano** goes even further in the efficiency direction. It is OpenAI's smallest and fastest model to date, optimized for ultra-low latency, high-volume API workloads, and use as a sub-agent within larger multi-agent systems. Think of nano as the workhorse you deploy when you need thousands of concurrent inferences at minimal cost per token.

  Both models are available via the OpenAI API and are positioned as successors and replacements for earlier small models like GPT-3.5 Turbo and GPT-4o mini, offering better performance per dollar across key benchmarks including coding, instruction following, and structured output generation.

## Why It Matters

  The release of GPT-4.1 mini and nano is significant for several interconnected reasons that reshape how developers and organizations think about deploying AI in production.

### 1. The Cost-Performance Revolution Continues

  One of the biggest barriers to AI adoption at scale has always been cost. Running large frontier models for every single API call in a high-traffic application is prohibitively expensive. GPT-4.1 mini and nano dramatically lower the cost per token while preserving strong performance on the tasks that matter most in real-world applications — particularly coding and tool use.

### 2. Multi-Agent Architectures Get a Dedicated Engine

  Multi-agent AI systems — where multiple AI models collaborate, delegate tasks, and call tools — are rapidly becoming the dominant paradigm for complex AI workflows. GPT-4.1 nano is explicitly designed to serve as a sub-agent: a fast, cheap, reliable component that handles routine sub-tasks within a larger orchestrated pipeline. This is a strategic architectural signal from OpenAI about where the industry is heading.

### 3. Multimodal Reasoning at Scale

  Both models support multimodal inputs, meaning they can process images alongside text. This opens the door to vision-enabled pipelines — document parsing, screenshot analysis, UI testing, and more — at a fraction of the cost of full-size multimodal models.

### 4. Coding and Tool Use as First-Class Citizens

  Unlike earlier small models that often struggled with reliable function calling and code generation, GPT-4.1 mini and nano have been explicitly optimized for these use cases. This makes them suitable replacements for much larger models in developer tooling, CI/CD pipelines, and automated code review workflows.

## How to Get Started

  Getting started with GPT-4.1 mini or nano is straightforward if you already have an OpenAI account. Follow the steps below to set up your environment and make your first API call.

### Prerequisites

  - An OpenAI account with API access (platform.openai.com)
  - Python 3.8+ installed on your machine
  - An active OpenAI API key

### Step 1: Install the OpenAI Python SDK
```
pip install openai --upgrade
```

### Step 2: Set Your API Key
It is best practice to set your API key as an environment variable rather than hardcoding it into your scripts.

```
# On macOS/Linux
export OPENAI_API_KEY="your-api-key-here"

# On Windows (Command Prompt)
set OPENAI_API_KEY=your-api-key-here

# On Windows (PowerShell)
$env:OPENAI_API_KEY="your-api-key-here"
```

### Step 3: Make Your First API Call
```
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4.1-mini",  # or "gpt-4.1-nano"
    messages=[
        {"role": "user", "content": "Hello! What can you do?"}
    ]
)

print(response.choices[0].message.content)
```

## Step-by-Step Usage Guide

  The following walkthrough covers three core use cases: basic text completion, tool/function calling, and multimodal (image + text) reasoning.

### Step 1: Basic Chat Completion with GPT-4.1 Mini

  Start with a simple conversation. This is the foundation of everything built on top of the chat completions API.

```
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=[
        {
            "role": "system",
            "content": "You are a senior Python developer. Be concise and precise."
        },
        {
            "role": "user",
            "content": "Write a Python function to validate an email address using regex."
        }
    ],
    temperature=0.2,
    max_tokens=500
)

print(response.choices[0].message.content)
```

  Notice the `temperature=0.2` setting — lower temperatures produce more deterministic, focused outputs, which is ideal for coding tasks.

### Step 2: Function Calling / Tool Use with GPT-4.1 Nano

  GPT-4.1 nano excels at tool use. Here is how to define a tool and have the model call it reliably.

```
from openai import OpenAI
import json

client = OpenAI()

tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get the current weather for a given city.",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "The name of the city, e.g. 'San Francisco'"
                    },
                    "unit": {
                        "type": "string",
                        "enum": ["celsius", "fahrenheit"],
                        "description": "Temperature unit"
                    }
                },
                "required": ["city"]
            }
        }
    }
]

response = client.chat.completions.create(
    model="gpt-4.1-nano",
    messages=[
        {"role": "user", "content": "What's the weather like in Tokyo right now?"}
    ],
    tools=tools,
    tool_choice="auto"
)

tool_call = response.choices[0].message.tool_calls[0]
print(f"Tool called: {tool_call.function.name}")
print(f"Arguments: {tool_call.function.arguments}")
```

### Step 3: Multimodal Reasoning — Analyzing an Image

  Both models support vision inputs. The following example sends an image URL along with a text prompt.

```
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "Describe what you see in this image and identify any UI elements."
                },
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/JPEG_example_flower.jpg/800px-JPEG_example_flower.jpg"
                    }
                }
            ]
        }
    ],
    max_tokens=300
)

print(response.choices[0].message.content)
```

### Step 4: Streaming Responses for Real-Time Applications

  For chat interfaces or real-time feedback loops, streaming dramatically improves perceived responsiveness.

```
from openai import OpenAI

client = OpenAI()

stream = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=[
        {"role": "user", "content": "Explain quantum entanglement in simple terms."}
    ],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="", flush=True)

print()  # newline at end
```

## 5+ Real-World Use Cases

### 1. Automated Code Review in CI/CD Pipelines

  Use GPT-4.1 mini to automatically review pull requests for bugs, style violations, and security issues as part of a GitHub Actions workflow.

```
import subprocess
from openai import OpenAI

client = OpenAI()

# Get the git diff for staged changes
diff = subprocess.check_output(["git", "diff", "HEAD~1"]).decode("utf-8")

response = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=[
        {
            "role": "system",
            "content": "You are a code reviewer. Identify bugs, security issues, and style problems."
        },
        {
            "role": "user",
            "content": f"Review this git diff:\n\n{diff}"
        }
    ]
)

print(response.choices[0].message.content)
```

### 2. High-Volume Customer Support Classification

  Deploy GPT-4.1 nano to classify thousands of incoming support tickets per minute into categories like billing, technical, and general — at minimal cost.

```
from openai import OpenAI

client = OpenAI()

def classify_ticket(ticket_text: str) -> str:
    response = client.chat.completions.create(
        model="gpt-4.1-nano",
        messages=[
            {
                "role": "system",
                "content": "Classify the support ticket into one of: billing, technical, general, urgent. Reply with only the category."
            },
            {"role": "user", "content": ticket_text}
        ],
        max_tokens=10,
        temperature=0
    )
    return response.choices[0].message.content.strip()

# Example batch processing
tickets = [
    "I can't log into my account since this morning.",
    "I was charged twice for my subscription this month.",
    "How do I export my data to CSV?"
]

for ticket in tickets:
    category = classify_ticket(ticket)
    print(f"Ticket: '{ticket[:40]}...' → Category: {category}")
```

### 3. Sub-Agent in a Multi-Agent Research Pipeline

  Use GPT-4.1 nano as a fast sub-agent that summarizes individual web pages, feeding results to a larger orchestrator model for synthesis.

```
from openai import OpenAI

client = OpenAI()

def summarize_page(page_content: str) -> str:
    """Sub-agent: summarize a single page quickly and cheaply."""
    response = client.chat.completions.create(
        model="gpt-4.1-nano",
        messages=[
            {
                "role": "system",
                "content": "Summarize the following web page content in 3 bullet points."
            },
            {"role": "user", "content": page_content[:4000]}  # token limit guard
        ],
        max_tokens=200
    )
    return response.choices[0].message.content

def synthesize_research(summaries: list[str], topic: str) -> str:
    """Orchestrator: synthesize all summaries into a research report."""
    combined = "\n\n".join(summaries)
    response = client.chat.completions.create(
        model="gpt-4.1-mini",  # slightly larger model for synthesis
        messages=[
            {
                "role": "system",
                "content": f"You are a research analyst. Synthesize these summaries about '{topic}' into a coherent report."
            },
            {"role": "user", "content": combined}
        ]
    )
    return response.choices[0].message.content

# Simulated pipeline
pages = ["Page 1 content about AI...", "Page 2 content about AI..."]
summaries = [summarize_page(p) for p in pages]
report = synthesize_research(summaries, "AI model efficiency")
print(report)
```

### 4. Document Intelligence and Data Extraction

  Extract structured data from scanned invoices or receipts using the multimodal capabilities of GPT-4.1 mini, then output clean JSON.

```
from openai import OpenAI
import json

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4.1-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "Extract the vendor name, total amount, date, and line items from this invoice. Return valid JSON only."
                },
                {
                    "type": "image_url",
                    "image_url": {"url": "https://example.com/invoice.jpg"}
                }
            ]
        }
    ],
    response_format={"type": "json_object"},
    max_tokens=500
)

invoice_data = json.loads(response.choices[0].message.content)
print(json.dumps(invoice_data, indent=2))
```

### 5. Real-Time Coding Assistant in a VS Code Extension

  Power an in-editor coding assistant that suggests completions, explains errors, and refactors code on demand — using GPT-4.1 mini for low latency.

from openai import OpenAI

client = OpenAI()

def explain_error(error_message: str, code_context: str) -> str:
    response = client.chat.completions.create(
        model="gpt-4.1-mini",
        messages=[
            {
                "role": "system",
                "content": "You are an expert debugger. Explain errors clearly and suggest fixes."
            },
            {
                "role": "user",
                "content": f"Error:\n{error_message}\n\nCode context:\n```python\n{code_context}\n```\n\nWhat's wrong and how do I fix it?"
            }
        ],
        stream=True
    )
    result = ""
    for chunk in response:
        if chunk.choices[0].delta.content:
            result += chunk.choices[0].delta.content
    return result

error = "TypeError: unsupported operand type(s) for +: 'int' and 'str