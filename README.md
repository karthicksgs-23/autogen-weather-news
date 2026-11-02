# Weather News - keyless DDGS search and optional AutoGen

## Run without any API key (default)

```powershell
cd D:\GenAI_2\week5\Day2\weather_news_agent
.\.venv\Scripts\Activate.ps1
python weather_news.py
```

Enter a city at `Which city?`. No `.env` file or API key is needed in this mode,
including when an OpenAI key happens to exist in your environment. Internet
access is still required for DDGS news search. The script displays the function
call and exactly three result lines. It selects recent news headlines locally;
it does not use a language model to summarize them. Empty results or a failed
search produce a clear three-line limitation message rather than invented weather.

## AutoGen assignment mode (optional)

The PDF's statement that no API key is needed applies to DDGS. Its AutoGen
example uses an OpenAI model and still needs an OpenAI API key. To use that flow,
put `OPENAI_API_KEY=your-actual-key` in `.env` beside `weather_news.py`, then run:

```powershell
python weather_news.py --autogen
```

This mode uses `AssistantAgent`, `tools=[search_weather_news]`, and
`reflect_on_tool_use=True`. AutoGen Console shows tool-call and execution events,
then the model writes a three-line reply. The model client closes in a `finally`
block. OpenAI API usage may incur charges. Existing environment variables take
precedence over `.env`. Never commit the `.env` file.

The default keyless mode does not provide the PDF's LLM-based AutoGen reflection;
use `--autogen` to demonstrate those requirements for the assignment.

## Fresh installation

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

All application code is in `weather_news.py`. The shared search function has
parameter/return type hints and a docstring describing its purpose. It first requests
up to five news articles from the last day using DDGS, without a search API key.
If that fails or returns no results, it tries DuckDuckGo news from the last week,
then DuckDuckGo web snippets. Fallback freshness limits are included in tool
results and must be stated in the reply. Search diagnostics distinguish missing
results, DNS failures, timeouts, certificate errors, and rate limiting.
The model reply is normalized before Console displays it, so the final response
body always occupies exactly three nonempty lines.
Run the script twice with different cities to compare results. News headlines
may be unavailable or unrelated; they are not a substitute for live weather data.

References:
- https://microsoft.github.io/autogen/stable/user-guide/agentchat-user-guide/tutorial/agents.html
- https://pypi.org/project/ddgs/
