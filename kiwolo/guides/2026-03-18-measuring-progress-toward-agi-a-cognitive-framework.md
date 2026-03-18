# Measuring progress toward AGI: A cognitive framework

*Published: March 18, 2026 | Source: Google DeepMind*

## What It Is

Google DeepMind has introduced a formal cognitive framework designed to systematically measure and track progress toward Artificial General Intelligence (AGI). Rather than relying on vague milestones or single-benchmark performance metrics, this framework proposes a structured, multi-dimensional approach to evaluating how close AI systems are to achieving human-level — and eventually superhuman-level — general cognitive capabilities.

At its core, the framework draws from cognitive science, developmental psychology, and AI research to define AGI not as a binary switch but as a **spectrum of capability levels** across a broad range of cognitive domains. These domains include reasoning, memory, learning, planning, language, perception, social intelligence, and more. The framework also introduces a companion initiative: a **Kaggle hackathon** inviting the global research and developer community to build the evaluations needed to operationalize this measurement system.

The framework distinguishes between *narrow AI* (systems that excel at specific tasks), *general AI* (systems that can perform any cognitive task a human can), and levels in between, using a tiered model to describe capability breadth and depth simultaneously. It also explicitly separates **performance** from **generalization** — a system that scores highly on a single benchmark may not generalize across domains the way a truly general intelligence would.

This effort represents one of the most rigorous, publicly shared attempts by a major AI lab to define what AGI actually means in measurable, scientific terms — and to provide tools for tracking progress toward it transparently.

## Why It Matters

The question of "how close are we to AGI?" has historically been answered with speculation, hype, or dismissal. Google DeepMind's framework changes that dynamic by introducing scientific rigor into one of AI's most consequential questions. Here's why this matters profoundly:

### 1. Ending the Benchmark Trap
Current AI evaluation culture is dominated by narrow benchmarks — MMLU, HumanEval, GSM8K — that measure performance on specific tasks. Models can "pass" these without demonstrating anything close to general intelligence. This framework pushes the field toward holistic, multi-domain evaluations that resist gaming and overfitting.

### 2. A Common Language for AGI Discussion
Researchers, policymakers, journalists, and the public often use "AGI" to mean wildly different things. DeepMind's tiered, domain-based definition gives the field a shared vocabulary and a shared yardstick — critical for both scientific progress and responsible governance.

### 3. Safety and Alignment Implications
Knowing where we are on the AGI spectrum has direct implications for AI safety research. If we can measure capability levels across domains with precision, we can better anticipate risks, deploy appropriate safeguards, and pace capability development responsibly.

### 4. Open Community Participation
By launching a Kaggle hackathon, DeepMind is democratizing the evaluation-building process. This is a significant step toward community-driven, independent accountability in AI progress measurement — rather than labs self-reporting progress on their own terms.

### 5. Foundational for Future Policy
Governments and regulatory bodies worldwide are grappling with how to regulate AI. A scientifically grounded, widely adopted framework for measuring AI capability levels could directly inform policy thresholds, licensing requirements, and safety standards.

## How to Get Started

Engaging with this framework can take several forms: reading and applying the conceptual model, participating in the Kaggle hackathon, or building your own evaluation suites inspired by the framework. Here's how to get started across all three paths.

### Path 1: Study the Framework

  - Read the official DeepMind blog post: https://deepmind.google/blog/measuring-progress-toward-agi-a-cognitive-framework/
  - Access the accompanying technical paper through the DeepMind research portal or arXiv.
  - Familiarize yourself with the cognitive domain taxonomy used in the framework.

### Path 2: Join the Kaggle Hackathon

  - Create or log in to your Kaggle account at kaggle.com
  - Search for the DeepMind AGI Evaluation Hackathon competition page.
  - Review the competition rules, dataset access instructions, and submission format.
  - Install the Kaggle CLI for streamlined dataset access:

```
# Install the Kaggle CLI
pip install kaggle

# Configure your API credentials
# Download kaggle.json from your Kaggle account settings and place it at:
# ~/.kaggle/kaggle.json (Linux/macOS)
# C:\Users\\.kaggle\kaggle.json (Windows)

# Verify installation
kaggle --version

# List available competitions related to the hackathon
kaggle competitions list --search "deepmind agi"

```

### Path 3: Build Evaluation Tools Locally

  - Set up a Python environment for cognitive evaluation experiments:

```
# Create and activate a virtual environment
python -m venv agi-eval-env
source agi-eval-env/bin/activate  # On Windows: agi-eval-env\Scripts\activate

# Install core dependencies for evaluation work
pip install openai anthropic transformers datasets
pip install numpy pandas scikit-learn matplotlib seaborn
pip install torch torchvision  # For model-level experiments
pip install jupyter notebook    # For interactive exploration

# Verify environment
python -c "import transformers; print(transformers.__version__)"

```

## Step-by-Step Usage Guide

The following walkthrough demonstrates how to apply the DeepMind cognitive framework practically — mapping AI system capabilities to framework domains, designing evaluations, and analyzing results.

### Step 1: Map the Cognitive Domain Taxonomy

The framework organizes cognition into domains. Your first step is to explicitly define which domains you want to evaluate. Based on the framework's structure, create a domain map:

```
# cognitive_domains.py
# Define the cognitive domain taxonomy from the DeepMind framework

COGNITIVE_DOMAINS = {
    "reasoning": {
        "description": "Logical deduction, inductive reasoning, causal inference",
        "subtypes": ["deductive", "inductive", "abductive", "causal", "analogical"]
    },
    "language": {
        "description": "Comprehension, generation, translation, summarization",
        "subtypes": ["reading_comprehension", "generation", "translation", "summarization"]
    },
    "memory": {
        "description": "Working memory, long-term recall, episodic and semantic memory",
        "subtypes": ["working_memory", "long_term_recall", "episodic", "semantic"]
    },
    "planning": {
        "description": "Multi-step planning, goal setting, strategy formulation",
        "subtypes": ["sequential_planning", "hierarchical_planning", "resource_allocation"]
    },
    "learning": {
        "description": "Few-shot learning, transfer learning, adaptation",
        "subtypes": ["few_shot", "zero_shot", "transfer", "continual"]
    },
    "perception": {
        "description": "Visual, auditory, and multimodal perception and understanding",
        "subtypes": ["visual", "auditory", "multimodal", "spatial"]
    },
    "social_intelligence": {
        "description": "Theory of mind, emotion recognition, collaborative reasoning",
        "subtypes": ["theory_of_mind", "emotion_recognition", "collaboration"]
    },
    "creativity": {
        "description": "Novel generation, problem reformulation, artistic creation",
        "subtypes": ["divergent_thinking", "combinatorial_creativity", "aesthetic_judgment"]
    }
}

def list_domains():
    for domain, info in COGNITIVE_DOMAINS.items():
        print(f"\n📌 {domain.upper()}")
        print(f"   Description: {info['description']}")
        print(f"   Subtypes: {', '.join(info['subtypes'])}")

if __name__ == "__main__":
    list_domains()

```

### Step 2: Design Domain-Specific Evaluation Tasks

For each cognitive domain, design evaluation tasks that probe the relevant capabilities. Here's an example of building a small reasoning evaluation battery:

```
# reasoning_eval.py
# Build a reasoning evaluation suite aligned with the framework

REASONING_TASKS = [
    {
        "id": "deductive_001",
        "domain": "reasoning",
        "subtype": "deductive",
        "prompt": (
            "All mammals are warm-blooded. Whales are mammals. "
            "Are whales warm-blooded? Explain your reasoning step by step."
        ),
        "expected_answer": "Yes",
        "evaluation_criteria": ["correct_conclusion", "valid_chain_of_reasoning"]
    },
    {
        "id": "causal_001",
        "domain": "reasoning",
        "subtype": "causal",
        "prompt": (
            "A city introduces a congestion charge for driving into the city center. "
            "Over the next year, air pollution in the city center decreases by 18%. "
            "Identify two plausible causal pathways that could explain this outcome."
        ),
        "expected_answer": None,  # Open-ended, requires rubric evaluation
        "evaluation_criteria": [
            "identifies_direct_pathway",
            "identifies_indirect_pathway",
            "demonstrates_causal_language"
        ]
    },
    {
        "id": "analogical_001",
        "domain": "reasoning",
        "subtype": "analogical",
        "prompt": (
            "Doctor is to patient as teacher is to ___? "
            "Now create a novel analogy that follows the same structural relationship "
            "but uses concepts from technology."
        ),
        "expected_answer": "student",
        "evaluation_criteria": ["correct_completion", "novel_analogy_quality"]
    }
]

def display_tasks(tasks):
    for task in tasks:
        print(f"\n{'='*60}")
        print(f"Task ID: {task['id']}")
        print(f"Domain: {task['domain']} > {task['subtype']}")
        print(f"Prompt: {task['prompt']}")
        print(f"Evaluation Criteria: {', '.join(task['evaluation_criteria'])}")

if __name__ == "__main__":
    display_tasks(REASONING_TASKS)

```

### Step 3: Run Evaluations Against an AI Model

Using the OpenAI API (or any compatible LLM endpoint), run your evaluation battery and collect structured results:

```
# run_evaluation.py
import openai
import json
import time
from reasoning_eval import REASONING_TASKS

client = openai.OpenAI(api_key="YOUR_API_KEY_HERE")

def evaluate_model_on_task(task, model="gpt-4o"):
    """Run a single evaluation task and return structured results."""
    response = client.chat.completions.create(
        model=model,
        messages=[
            {
                "role": "system",
                "content": (
                    "You are being evaluated on cognitive tasks. "
                    "Respond clearly and show your reasoning where appropriate."
                )
            },
            {"role": "user", "content": task["prompt"]}
        ],
        temperature=0.0,  # Deterministic for evaluation
        max_tokens=500
    )
    
    return {
        "task_id": task["id"],
        "domain": task["domain"],
        "subtype": task["subtype"],
        "model": model,
        "response": response.choices[0].message.content,
        "tokens_used": response.usage.total_tokens,
        "prompt_tokens": response.usage.prompt_tokens,
        "completion_tokens": response.usage.completion_tokens
    }

def run_full_evaluation(tasks, model="gpt-4o", output_file="eval_results.json"):
    """Run all tasks and save results to JSON."""
    results = []
    
    for i, task in enumerate(tasks):
        print(f"Running task {i+1}/{len(tasks)}: {task['id']}...")
        try:
            result = evaluate_model_on_task(task, model)
            results.append(result)
            time.sleep(1)  # Respect rate limits
        except Exception as e:
            print(f"Error on task {task['id']}: {e}")
            results.append({
                "task_id": task["id"],
                "error": str(e)
            })
    
    with open(output_file, "w") as f:
        json.dump(results, f, indent=2)
    
    print(f"\n✅ Evaluation complete. Results saved to {output_file}")
    return results

if __name__ == "__main__":
    results = run_full_evaluation(REASONING_TASKS)
    for r in results:
        print(f"\nTask: {r['task_id']}")
        print(f"Response: {r.get('response', 'ERROR')[:200]}...")

```

### Step 4: Score and Analyze Results Using the Framework's Tier System

The DeepMind framework proposes capability tiers (from narrow/task-specific to broadly general). Apply a scoring system that maps evaluation performance to framework tiers:

# score_and_tier.py
import json

# Framework-inspired capability tiers
CAPABILITY_TIERS = {
    1: "Narrow Task Performance — excels at specific prompted tasks",
    2: "Emergent Generalization — shows transfer within a domain",
    3: "Cross-Domain Reasoning — applies concepts across cognitive domains",
    4: "Autonomous Learning — adapts with minimal supervision",
    5: "Human-Level General Cognition — matches human performance broadly",
    6: "Superhuman General Cognition — exceeds human performance broadly"
}

def compute_domain_score(results, domain):
    """Compute a raw pass rate for a given domain (placeholder scoring)."""
    domain_results = [r for r in results if r.get("domain") == domain]
    if not domain_results:
        return None
    
    # In a real system, this would use rubric-based human or LLM judging
    # Here we use response length as a naive proxy for completeness
    scores = []
    for r in domain_results:
        response = r.get("response", "")
        # Naive proxy: longer, more structured responses score higher
        score = min(len(response) / 300, 1.0)
        scores.append(score)
    
    return sum(scores) / len(scores)

def assign_tier(domain_scores):
    """Map average domain score to a capability tier."""
    avg_score = sum(domain_scores.values()) / len(domain_scores)
    if avg_score >= 0.9:
        return 5
    elif avg_score >= 0.75:
        return 4
    elif avg_score >= 0.55:
        return 3
    elif avg_score >= 0.35:
        return 2
    else:
        return 1

def generate_report(results_file="eval_results.json"):
    with open(results_file) as f:
        results = json.load(f)
    
    domains = list(set(r["domain"] for r in results if "domain" in r))
    domain_scores = {}
    
    print("\n📊 COGNITIVE FRAMEWORK EVALUATION REPORT")
    print("=" * 50)
    
    for domain in domains:
        score = compute_domain_score(results, domain)
        if score is not None:
            domain_scores[domain] = score
            print(f"  {domain.upper()}: {score:.2f} / 1.00")