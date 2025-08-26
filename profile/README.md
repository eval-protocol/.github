# Eval Protocol (EP)

<div align="center">

<img src="https://raw.githubusercontent.com/eval-protocol/eval-protocol/main/assets/favicon-light.png" alt="Eval Protocol Logo" height="128"/>

**The open-source toolkit for building your internal model leaderboard.**

[![PyPI](https://img.shields.io/pypi/v/eval-protocol)](https://pypi.org/pypi/v/eval-protocol/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/eval-protocol/eval-protocol/blob/main/LICENSE)

[Documentation](https://evalprotocol.io)

</div>

## 🚀 What is Eval Protocol?

When you have multiple AI models to choose from—different versions, providers, or configurations—how do you know which one is best for your use case?

## 🏗️ Organization Repositories

### Core Projects

| Repository | Description |
|------------|-------------|
| **[eval-protocol](https://github.com/eval-protocol/eval-protocol)** | Main specification, documentation, and examples |
| **[python-sdk](https://github.com/eval-protocol/python-sdk)** | Python implementation with pytest integration |

## 🚀 Quick Start

```bash
pip install eval-protocol
```

### Example: Model Comparison

Compare models on a simple formatting task:

```python
from eval_protocol.models import EvaluateResult, EvaluationRow, Message
from eval_protocol.pytest import default_single_turn_rollout_processor, evaluation_test

@evaluation_test(
    input_messages=[
        [
            Message(role="system", content="Use bold text to highlight important information."),
            Message(role="user", content="Explain why evaluations matter for AI agents. Make it dramatic!"),
        ],
    ],
    model=[
        "fireworks_ai/accounts/fireworks/models/llama-v3p1-8b-instruct",
        "openai/gpt-4",
        "anthropic/claude-3-sonnet"
    ],
    rollout_processor=default_single_turn_rollout_processor,
    mode="pointwise",
)
def test_bold_format(row: EvaluationRow) -> EvaluationRow:
    """Check if the model's response contains bold text."""
    assistant_response = row.messages[-1].content
    
    if assistant_response is None:
        row.evaluation_result = EvaluateResult(score=0.0, reason="No response")
        return row
    
    has_bold = "**" in str(assistant_response)
    score = 1.0 if has_bold else 0.0
    reason = "Contains bold text" if has_bold else "No bold text found"
    
    row.evaluation_result = EvaluateResult(score=score, reason=reason)
    return row
```

## 🎯 Why Build Model Leaderboards?

- **Compare models** across quality, performance, and cost
- **Standardize evaluation** across your entire model portfolio  
- **Make data-driven decisions** about which model to deploy
- **Track improvements** as you iterate on models and prompts

## 📚 Resources

- **[Documentation](https://evalprotocol.io)** - Complete guides and API reference
- **[Discord](https://discord.com/channels/1137072072808472616/1400975572405850155)** - Community discussions

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/eval-protocol/eval-protocol/blob/main/LICENSE) file for details.
