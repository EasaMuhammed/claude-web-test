# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a test repository for Claude Code on the web, running in a secure sandboxed environment. It contains the **nanochat** project by Andrej Karpathy - a minimal, full-stack implementation of a ChatGPT-like LLM trained from scratch.

## nanochat Overview

nanochat is a complete LLM training pipeline (~8K LOC) that trains ChatGPT-style models for ~$100-1000. The project includes:
- Tokenization (custom Rust BPE tokenizer)
- Base pretraining on web data
- Midtraining on chat/multiple-choice/tool-use data
- Supervised Fine-Tuning (SFT)
- Optional Reinforcement Learning (RL) on GSM8K
- Evaluation and web serving with ChatGPT-like UI

**Model tiers:**
- `speedrun.sh`: d20 model (~$100, 4 hours on 8xH100)
- `run1000.sh`: d32 model (~$800, 33 hours on 8xH100, 1.9B params)

## Key Commands

All commands should be run from the `nanochat/` directory:

**Training:**
```bash
# Full pipeline training (requires 8xH100 GPUs)
bash speedrun.sh

# Run in screen session with logging
screen -L -Logfile speedrun.log -S speedrun bash speedrun.sh

# CPU/MPS development (smaller models)
bash dev/runcpu.sh
```

**Testing:**
```bash
# Run tokenizer tests
python -m pytest tests/test_rustbpe.py -v -s

# Run all tests
python -m pytest tests/ -v -s
```

**Inference:**
```bash
# Activate virtual environment first
source .venv/bin/activate

# Web UI (ChatGPT-like interface)
python -m scripts.chat_web

# CLI chat interface
python -m scripts.chat_cli
```

**Environment setup:**
```bash
# Install with uv (project uses uv for dependency management)
uv sync

# For GPU training
uv sync --extra gpu

# For CPU/MPS development
uv sync --extra cpu
```

## Architecture Overview

**Training Pipeline:**
1. **Tokenizer Training** (`scripts/tok_train.py`) - Trains custom BPE tokenizer using Rust backend
2. **Base Training** (`scripts/base_train.py`) - Pretrains GPT model on web text (FineWeb dataset)
3. **Midtraining** (`scripts/mid_train.py`) - Continues training on chat/tool-use data
4. **SFT** (`scripts/chat_sft.py`) - Supervised fine-tuning on conversational data
5. **RL** (`scripts/chat_rl.py`) - Optional reinforcement learning on math problems

**Core Components:**
- `nanochat/gpt.py` - GPT Transformer implementation
- `nanochat/engine.py` - Efficient inference with KV caching
- `nanochat/dataloader.py` - Distributed data loading with tokenization
- `nanochat/adamw.py` & `nanochat/muon.py` - Distributed optimizers
- `nanochat/tokenizer.py` - BPE tokenizer wrapper (GPT-4 style)
- `rustbpe/` - Rust BPE tokenizer implementation

**Tasks/Evaluations:**
- `tasks/arc.py` - ARC science questions
- `tasks/gsm8k.py` - Grade school math problems
- `tasks/humaneval.py` - Python coding tasks
- `tasks/mmlu.py` - Multiple choice knowledge questions
- `tasks/smoltalk.py` - Conversational dataset

**GPU Requirements:**
- Designed for 8xH100 (80GB each) but works on single GPU (8x slower)
- For <80GB VRAM: reduce `--device_batch_size` (32→16→8→4→2→1)
- Ampere 8xA100 works but slower than H100

## Git Branch Convention

Development branches follow the pattern: `claude/<session-id>`
- All development work should occur on session-specific branches
- Branches must start with `claude/` prefix
- Push operations require the correct session ID suffix

## Working with External Repositories

The nanochat repository is excluded from version control via `.gitignore`. This allows exploration without committing the external codebase to this repository.
