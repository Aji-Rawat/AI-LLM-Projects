# Meeting Minutes Generator

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Aji-Rawat/AI-LLM-Projects/blob/main/meeting-minutes-generator/Meeting_summary.ipynb)

Turn a meeting recording into clean, structured minutes using two open-source models running entirely in a Colab notebook, with no paid API required.

## How it works

```
 meeting audio (.mp3)
        |
        v
 [1] Whisper (openai/whisper-medium.en)  -->  full text transcript
        |
        v
 [2] Llama 3.2 3B Instruct (4-bit quantized)  -->  meeting minutes in Markdown
```

1. **Transcription:** `whisper-medium.en` converts the audio to text through the Hugging Face `automatic-speech-recognition` pipeline.
2. **Summarization:** the transcript is sent to `meta-llama/Llama-3.2-3B-Instruct` with a prompt asking for a summary (attendees, location, date), discussion points, takeaways and action items with owners.
3. **Memory saving:** Llama is loaded in 4-bit (NF4) with `BitsAndBytesConfig`, so it fits on a free Colab T4 GPU.

## Requirements

- Google account and **Google Colab** with a **GPU runtime** (Runtime > Change runtime type > T4 GPU)
- A [Hugging Face](https://huggingface.co) account and access token
- Approved access to the gated model [`meta-llama/Llama-3.2-3B-Instruct`](https://huggingface.co/meta-llama/Llama-3.2-3B-Instruct) (request it on the model page and wait for approval)
- A meeting audio file (the notebook uses an extract of a Denver City Council meeting)

## Setup

1. Click the **Open in Colab** badge above.
2. Switch the runtime to a GPU.
3. In Colab, open the key icon (**Secrets**) in the left sidebar, add a secret named `HF_TOKEN` with your Hugging Face token, and turn on notebook access.
4. Upload your audio file to Google Drive and update the path in the notebook:
   ```python
   audio_filename = "/content/drive/MyDrive/llm/your_meeting.mp3"
   ```
5. Run all cells.

To run locally instead of Colab, install the dependencies with `pip install -r requirements.txt` and make sure you have an NVIDIA GPU, ffmpeg, and replace the Google Drive and `userdata` parts with a local path and an environment variable for your token.

## Configuration

| Setting | Where | Default |
|---------|-------|---------|
| Audio file | `audio_filename` | `/content/drive/MyDrive/llm/denver_extract.mp3` |
| Speech model | `pipeline(model=...)` | `openai/whisper-medium.en` |
| LLM | `LLAMA` constant | `meta-llama/Llama-3.2-3B-Instruct` |
| Output length | `max_new_tokens` | `2000` |
| Output format | `system_message` / `user_prompt` | Markdown minutes |

## Example output

The generated minutes follow this structure:

```markdown
# Denver City Council Meeting Minutes
## Summary
Attendees: ...
Location: ...
Date: ...

## Discussion Points
- ...

## Takeaways
- ...

## Action Items
- ... (owner)
```

The full run, including the transcript and generated minutes, is saved in the notebook.

## Limitations

- A 3B model is small. It can miss details or **invent** them (dates, names, action-item owners), so always check the minutes against the transcript.
- Speaker names are not identified. The transcript is plain text without speaker labels.
- Very long meetings may exceed the model's context window. For those, split the transcript into chunks and summarize each one first.
- `whisper-medium.en` supports English only.

## Ideas for improvement

- Add speaker diarization (e.g. `pyannote.audio`)
- Chunk long transcripts (map-reduce summarization)
- Add a Gradio UI for uploading audio
- Export minutes to PDF or DOCX

## Tech stack

Python, PyTorch, Hugging Face Transformers, bitsandbytes, Whisper, Llama 3.2, Google Colab
