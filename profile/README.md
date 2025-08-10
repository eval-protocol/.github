# Eval Protocol (EP)

<div align="center">

![Eval Protocol Logo](https://raw.githubusercontent.com/eval-protocol/eval-protocol/main/assets/logo-light.png)

**Build high quality AI applications.**

**An open specification, pytest wrapper, and suite of tools that standardizes how developers author evaluations for large language model (LLM) applications.**

[![PyPI](https://img.shields.io/pypi/v/eval-protocol)](https://pypi.org/project/eval-protocol/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/eval-protocol/eval-protocol/blob/main/LICENSE)
[![Discord](https://img.shields.io/badge/discord-join-7289da.svg)](https://discord.com/channels/1137072072808472616/1400975572405850155)

[Documentation](https://evalprotocol.io) • [Quick Start](#user-content--quick-start) • [Why EP?](#user-content--why-eval-protocol) • [Community](#user-content--community)

</div>

## 🚀 What is Eval Protocol?

Eval Protocol (EP) transforms evaluations from one-off tests into the foundation of your AI development loop. Whether you're benchmarking single-turn tasks or training complex multi-turn agents, EP gives you a consistent way to define, run, and monitor evaluations—at every stage of system development.

### ✨ Key Features

- **🧪 Evaluations as Code**: Write evals as pytest functions with familiar decorators
- **🔄 One Format, Two Modes**: Support for both static and dynamic evaluation
- **🎯 Reinforcement Learning Ready**: Bridge the gap between evaluation and training
- **🔧 Standards-Based**: Built on OpenAI Chat Completions API, MCP, and pytest
- **📊 Rich Instrumentation**: Real-time log viewing and comprehensive result storage
- **🚀 Production Ready**: Parallel execution and optimized for multi-turn scenarios

## 🏗️ Organization Repositories

### Core Projects

| Repository | Description | Status |
|------------|-------------|---------|
| **[eval-protocol](https://github.com/eval-protocol/eval-protocol)** | Main specification, documentation, and examples | ![Active](https://img.shields.io/badge/status-active-brightgreen) |
| **[python-sdk](https://github.com/eval-protocol/python-sdk)** | Python implementation with pytest integration | ![Active](https://img.shields.io/badge/status-active-brightgreen) |

## 🚀 Quick Start

Install EP with a single command and start evaluating your models:

```bash
pip install eval-protocol
```

### Simple Example

Here's a quick test that checks if a model's response contains **bold** text formatting:

```python
from eval_protocol.models import EvaluateResult, EvaluationRow, Message
from eval_protocol.pytest import default_single_turn_rollout_processor, evaluation_test

@evaluation_test(
    input_messages=[
        [
            Message(role="system", content="You are a helpful assistant. Use bold text to highlight important information."),
            Message(role="user", content="Explain why **evaluations** matter for building AI agents. Make it dramatic!"),
        ],
    ],
    model=["fireworks_ai/accounts/fireworks/models/llama-v3p1-8b-instruct"],
    rollout_processor=default_single_turn_rollout_processor,
    mode="pointwise",
)
def test_bold_format(row: EvaluationRow) -> EvaluationRow:
    """Simple evaluation that checks if the model's response contains bold text."""
    assistant_response = row.messages[-1].content
    
    if assistant_response is None:
        result = EvaluateResult(score=0.0, reason="❌ No response found")
        row.evaluation_result = result
        return row
    
    # Check if response contains **bold** text
    has_bold = "**" in str(assistant_response)
    
    if has_bold:
        result = EvaluateResult(score=1.0, reason="✅ Response contains bold text")
    else:
        result = EvaluateResult(score=0.0, reason="❌ No bold text found")
    
    row.evaluation_result = result
    return row
```

## 🎯 Why Eval Protocol?

AI quality is a hard problem. Building a great AI system isn't just about using a better model—it's about making better decisions across prompts, data, model versions, tool usage, and user workflows. To do that well, you need great *evals*.

EP was created to help developers treat evaluation as a core part of the development loop, not an afterthought. With EP, you get one evaluation framework that supports:

- ✅ **Offline benchmarking** (for model selection and quality tracking)
- ✅ **Dataset generation** (for SFT and RL)
- ✅ **Reward shaping and debugging** (for custom training workflows)
- ✅ **CI integration** (so regressions are caught early)

## 🏛️ Core Principles

1. **Evaluations as Code**: Treat evaluations as first-class code—not configuration files
2. **Developer Experience First**: Prioritize developer productivity through simple integration
3. **Standards-Based Interoperability**: Build on existing, proven standards
4. **Non-Prescriptive Architecture**: Work with any AI architecture
5. **Open Source Foundation**: Unify AI developers on a standard
6. **Performance at Scale**: Designed for production workloads
7. **Evolutionary Architecture**: Grow from single-turn to multi-turn evaluations
8. **Reinforcement Learning Ready**: Bridge evaluation and training

## 📚 Documentation & Resources

- **[📖 Full Documentation](https://evalprotocol.io)** - Comprehensive guides and API reference
- **[📋 Specification](https://evalprotocol.io/specification)** - Technical specification details
- **[🎓 Tutorials](https://evalprotocol.io/tutorial)** - Step-by-step guides for different use cases
- **[🔧 Examples](https://evalprotocol.io/example)** - Ready-to-run evaluation examples

## 🤝 Community

- **💬 [Discord](https://discord.com/channels/1137072072808472616/1400975572405850155)** - Join our community discussions

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/eval-protocol/eval-protocol/blob/main/LICENSE) file for details.