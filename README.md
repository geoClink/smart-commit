# Smart Commit

AI-powered Git commit message generator. Analyzes your staged changes and generates a structured, accurate commit message — locally, privately, and for free.

Originally created by [brazill7](https://github.com/brazill7/smart-commit). This fork adds Ollama, Groq, and Gemini support, safety checks, and UX improvements.

---

## What it does

- Reads your staged `git diff`
- Generates a commit message using AI with this format:

```
[Feature] Short summary title
- Added X
- Removed Y
- Fixed Z
```

- You can accept, give feedback to regenerate, or abort

---

## Providers

| Provider | Requires | Internet |
|----------|----------|----------|
| MLX (default) | Apple Silicon | No |
| Ollama | Ollama installed locally | No |
| Apple Intelligence | Apple Silicon + macOS 15+ | No |
| Groq | Free API key | Yes |
| Gemini | Google API key | Yes |

---

## Requirements

- Python 3.11+
- Git
- One of the supported AI providers (see above)

---

## Installation

**1. Clone the repo**
```bash
git clone https://github.com/geoClink/smart-commit.git
cd smart-commit
```

**2. Install dependencies**
```bash
pip3.11 install apple-fm-sdk mlx-lm ollama groq google-genai
```

**3. Set up the git alias**
```bash
git config --global alias.sc '!python3.11 ~/smart-commit/smartcommit.py'
```

---

## Usage

Stage your files, then run from any project:

```bash
git add .
git sc
```

### Use a specific provider

```bash
git sc --provider mlx          # default, Apple Silicon only
git sc --provider ollama       # no API key needed
git sc --provider apple        # Apple Intelligence (on-device)
git sc --provider groq         # requires GROQ_API_KEY
git sc --provider gemini       # requires GEMINI_API_KEY
```

### Add context to guide the AI

```bash
git sc -c "fixes login race condition"
```

### Preview without committing

```bash
git sc --dry-run
```

---

## MLX Setup (default)

Requires Apple Silicon. Install the dependency and the model downloads automatically on first run:

```bash
pip3.11 install mlx-lm
```

The default model is `mlx-community/Qwen2.5-Coder-7B-Instruct-4bit`. You can override it:

```bash
git sc --mlx-model mlx-community/some-other-model
```

---

## Ollama Setup

```bash
brew install ollama
brew services start ollama
ollama pull qwen2.5-coder
pip3.11 install ollama
```

Then just use `git sc` — Ollama runs in the background automatically.

---

## Groq Setup (optional)

Get a free API key at [console.groq.com](https://console.groq.com) and add it to your shell:

```bash
echo 'export GROQ_API_KEY=your_key_here' >> ~/.zshrc
source ~/.zshrc
```

---

## Safety Features

- Warns if committing directly to `main` or `master`
- Warns if sensitive files (`.env`, `.pem`, `id_rsa`, etc.) are staged
- Detects possible secrets or API keys in your diff
- Warns on unusually large commits
- Warns about unstaged or untracked files

---

## Credits

Original project by [brazill7](https://github.com/brazill7/smart-commit).
