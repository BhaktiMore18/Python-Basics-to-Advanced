# Code Generation CLI (GitHub Models)

A minimal command‑line tool that generates code from a short description and a target language using the GitHub Models (OpenAI‑compatible) API.

## What it does
- Takes two inputs: program name/description and programming language
- Calls the GitHub Models Chat Completions API
- Prints the generated code to stdout

## Requirements
- Python 3.9+
- Internet access to `https://models.github.ai/inference`
- GitHub Personal Access Token (fine‑grained) with Models/AI inference access

## Setup
1) Create and activate a virtual environment
```bash
python -m venv .venv
source .venv/bin/activate   # Windows PowerShell: .\.venv\Scripts\Activate.ps1
```

2) Install dependencies
```bash
pip install -r requirements.txt
```

3) Create `secret.txt`
```text
<YOUR_GITHUB_TOKEN_HERE>
```
- Single line, no quotes, no key name
- File must be in the same directory as `code_generator.py`

## Usage
```bash
python code_generator.py "Two Sum" python
```

Optional logging and diagnostics:
```bash
# Verbose logs
python code_generator.py "Two Sum" python --log-level DEBUG

# Deep connection diagnostics (DNS/TLS/proxy/CA info)
python code_generator.py "Two Sum" python --debug-connect --log-level DEBUG
```

## Defaults
- Endpoint: `https://models.github.ai/inference`
- Model: `openai/gpt-4o-mini`

## Docker

Build the image:
```bash
docker build -t code-gen-cli:latest .
```

Run with token via env var (no volume needed):
- PowerShell:
```powershell
docker run --rm -it ^
  -e GITHUB_TOKEN=ghp_xxx ^
  code-gen-cli:latest "Two Sum" python
```
- bash:
```bash
docker run --rm -it \
  -e GITHUB_TOKEN=ghp_xxx \
  code-gen-cli:latest "Two Sum" python
```

Run with mounted `secret.txt` and persisted `output/`:
- PowerShell:
```powershell
docker run --rm -it ^
  -v "${PWD}\secret.txt:/app/secret.txt:ro" ^
  -v "${PWD}\output:/app/output" ^
  code-gen-cli:latest "Two Sum" python
```
- bash:
```bash
docker run --rm -it \
  -v "$(pwd)/secret.txt:/app/secret.txt:ro" \
  -v "$(pwd)/output:/app/output" \
  code-gen-cli:latest "Two Sum" python
```

Notes:
- If both env var and `secret.txt` are provided, `secret.txt` takes precedence (file is used).
- Generated files are saved in `/app/output`; mount it to keep results on the host.

## How others can use this tool
1) Fork this repository on GitHub

2) Clone your fork
```bash
git clone https://github.com/<your-username>/Python-Basics-to-Advanced.git
cd Python-Basics-to-Advanced/Python-Basics-to-Advanced/projects/code-gen automation
```

3) Create a virtual environment and install dependencies
```bash
python -m venv .venv
source .venv/bin/activate   # Windows PowerShell: .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

4) Add your token to `secret.txt`
```text
<YOUR_GITHUB_TOKEN_HERE>
```
- Single line, no quotes, no variable name

5) Run the generator
```bash
python code_generator.py "Two Sum" python
```

6) Optional: verbose logs and diagnostics
```bash
python code_generator.py "Two Sum" python --log-level DEBUG
python code_generator.py "Two Sum" python --debug-connect --log-level DEBUG
```

Notes for contributors
- Do not commit real tokens. `secret.txt` is git‑ignored.
- Open a PR from your fork’s branch to propose improvements.
- If you change dependencies, update `requirements.txt` accordingly.

## Troubleshooting
- 401 Unauthorized in diagnostics: token missing/invalid or lacks Models permissions
- 429 Too Many Requests: rate limit; wait and retry
- Connection errors: check proxy/VPN/firewall; try `--debug-connect` and verify TLS succeeds
- Cert issues: ensure `certifi` is installed in the venv (already listed)

## Files
```
code_generator.py   # CLI
secret.txt          # GitHub token (git‑ignored)
requirements.txt    # Dependencies
.gitignore          # Ignore rules
README.md           # This file
```
