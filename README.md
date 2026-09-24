# StudyBuddy: AI study chatbot

Flask backend + SQLite + single-page frontend, powered by the Anthropic API.

## Run it
```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env        # then paste your ANTHROPIC_API_KEY into .env
python app.py               # open http://localhost:5000
```

## Features
- Modes: Explain, Study notes, Quiz (5 MCQs + answer key), Study plan, Ask my file
- Difficulty: Beginner / Intermediate / Advanced (changes the tutor prompt)
- Multiple conversations with history stored in SQLite (`study.db`)
- Generated notes, quizzes and plans are saved and exportable as .md or .txt
- Upload a PDF, .txt or .md file and ask questions about it
- Loading indicator, error messages, API key kept in environment variables

## API
| Method | Path | Purpose |
|---|---|---|
| GET/POST | /api/conversations | list / create |
| GET/DELETE | /api/conversations/<id> | load messages / delete |
| POST | /api/conversations/<id>/chat | `{message, mode, level}` |
| POST | /api/conversations/<id>/upload | multipart `file` |
| GET | /api/saved, /api/saved/<id>/export?format=md\|txt | saved items and export |

## Prompt engineering
Each request combines a base tutor prompt, a level instruction, a mode-specific
task prompt (output structure, length limits), the last 12 messages, and the
uploaded document (wrapped in `<document>` tags and marked as material, not instructions).
