# Open Model Arena

Ask one question and compare streaming answers from four models side by side.
Each panel has its own model selector and conversation history. Changing a
panel's model clears that panel, while **Clear all chats** resets the full arena.

## Setup

Create and activate a virtual environment, then install the dependencies:

```bash
cd model-arena
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Copy the environment template and add your OpenRouter API key:

```bash
cp .env.example .env
```

Start Mercury from this directory:

```bash
mercury
```

Open `model-arena.ipynb` in the Mercury home page. Enter a prompt in the
bottom input to send it to all four selected models concurrently.

OpenRouter usage may incur charges for each selected model.
