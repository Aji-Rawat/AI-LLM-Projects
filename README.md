# AI-LLM-Projects

Developing real world solutions with AI.

A collection of hands-on projects built with large language models (LLMs) and other AI tools. Each project lives in its own folder with its own README, dependencies and instructions, so you can pick one and run it on its own.

## Projects

| # | Project | What it does | Stack |
|---|---------|--------------|-------|
| 1 | [Meeting Minutes Generator](./meeting-minutes-generator) | Turns a meeting audio recording into structured minutes (summary, discussion points, takeaways, action items) | Whisper, Llama 3.2, Hugging Face Transformers, 4-bit quantization |

More projects coming soon.

## Repository structure

```
AI-LLM-Projects/
├── README.md                    <- you are here
├── LICENSE
├── .gitignore
├── .gitattributes
└── meeting-minutes-generator/   <- one folder per project
    ├── README.md
    ├── requirements.txt
    └── Meeting_summary.ipynb
```

## Getting started

```bash
git clone https://github.com/Aji-Rawat/AI-LLM-Projects.git
cd AI-LLM-Projects/meeting-minutes-generator
```

Then follow the README inside the project folder. Most notebooks are designed for **Google Colab** with a free GPU, so you don't need a powerful local machine.

## Adding a new project

1. Create a folder with a short, lowercase, hyphenated name (e.g. `pdf-question-answering`).
2. Put the code, a `requirements.txt` and a project `README.md` inside it.
3. Add a row to the **Projects** table above.

## Secrets and API keys

Never commit API keys or tokens. Use Colab Secrets or a local `.env` file (already listed in `.gitignore`).

## License

Released under the [MIT License](./LICENSE).
