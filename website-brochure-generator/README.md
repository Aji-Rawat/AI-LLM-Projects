# Website Brochure Generator

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Aji-Rawat/AI-LLM-Projects/blob/main/website-brochure-generator/Brochure.ipynb)

Give it a company name and a website URL, and it writes a short marketing brochure in Markdown. The LLM decides which pages of the site are worth reading, so the brochure is built from the About, Careers, Courses pages and so on, not just the landing page.

## How it works

```
 company URL
     |
     v
 [1] Scrape all links on the landing page (requests + BeautifulSoup)
     |
     v
 [2] LLM call #1: pick the relevant links (About, Careers, ...)  --> JSON
     |
     v
 [3] Scrape the landing page + each selected page
     |
     v
 [4] LLM call #2: write the brochure from the collected text  --> Markdown
```

1. **Link extraction:** `fetch_website_links()` collects every `<a href>` on the page.
2. **Link selection:** the model gets the link list and returns JSON (`response_format={"type": "json_object"}`) naming the pages worth including, and skipping things like Terms and Privacy.
3. **Content scraping:** `fetch_website_content()` strips scripts, styles, images and inputs, and keeps the page title and body text.
4. **Brochure writing:** all collected text goes to the model with a brochure system prompt, and the result is rendered as Markdown.

This is a classic **two-step LLM pipeline**: one call makes a decision (which links), the next call does the generation.

## Requirements

- Google account and **Google Colab** (no GPU needed)
- A free **Google Gemini API key** from [Google AI Studio](https://aistudio.google.com/apikey)

The notebook talks to Gemini through its OpenAI-compatible endpoint, using the `openai` Python library.

## Setup

1. Click the **Open in Colab** badge above.
2. In Colab, open the key icon (**Secrets**) in the left sidebar, add a secret named `GOOGLE_API_KEY` with your key, and turn on notebook access.
3. Run all cells. To try your own site, change the last cell:
   ```python
   create_brochure("Company Name", "https://www.example.com/")
   ```

To run locally, install the dependencies with `pip install -r requirements.txt`, set an environment variable `GOOGLE_API_KEY`, and run the notebook in Jupyter or VS Code.

> **Never hardcode your API key in the notebook or commit it to GitHub.** Anyone can find and use a leaked key.

## Configuration

| Setting | Where | Default |
|---------|-------|---------|
| Model | `MODEL` | `gemini-3.5-flash-lite` |
| API endpoint | `GEMINI_BASE_URL` | Gemini OpenAI-compatible endpoint |
| Characters per scraped page | `fetch_website_content()` | `2,000` |
| Total prompt size | `get_brochure_user_prompt()` | `5,000` characters |
| Tone of the brochure | `brochure_system_prompt` | Professional (a humorous version is included, commented out) |

## Example output

Run on `https://www.geeksforgeeks.org/`, the notebook produces a brochure with sections like:

```markdown
# Welcome to GeeksforGeeks (GFG)
## Empowering Millions Worldwide
## What We Offer
## Our Customers & Community
## Company Culture & Careers
## Get in Touch
```

The full run is saved in the notebook.

## Limitations

- **JavaScript-rendered sites** won't work well. `requests` only sees the HTML the server sends, not content added by JavaScript.
- **Relative links** (like `/about`) are passed to the model as they are, so some selected pages may fail to load. Fix this with `urllib.parse.urljoin`.
- **Truncation:** pages are cut to 2,000 characters and the final prompt to 5,000, so long sites lose information.
- **Hallucination:** the model may add details that are not on the site, so check facts before using the brochure.
- Some sites block scrapers. Respect each site's `robots.txt` and terms of service.

## Ideas for improvement

- Resolve relative URLs with `urljoin`
- Raise the character limits for models with larger context windows
- Stream the output as it is generated
- Add a Gradio UI with a URL box
- Support JavaScript sites with Playwright or Selenium
- Export the brochure to PDF or DOCX

## Tech stack

Python, OpenAI SDK (with Gemini), BeautifulSoup, Requests, Google Colab
