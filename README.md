# AI Error Helper

A small Python terminal app for beginners. Enter a one-line error message to
receive an explanation, possible causes, and three diagnostic steps.
The app only prints suggestions. It does not execute suggested commands.

## Windows PowerShell setup

Open PowerShell in this project folder. Python 3.10 or newer is required.
For a fresh installation, create a virtual environment and install dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

The `.venv` folder contains this project's Python environment and libraries.
Using its Python directly avoids activation and execution policy changes.
If the environment is already prepared, skip these setup commands.

Create your local configuration without overwriting an existing file:

```powershell
if (-not (Test-Path .env)) { Copy-Item .env.example .env }
notepad .env
```

Replace `your_api_key_here` with your API key in the editor and save the file.
Keep the key private; do not paste it into a conversation.
`OPENAI_MODEL` selects the model. The example uses `gpt-5.5` from the SDK docs.
You can choose another Responses API model available to your account.
Settings are read only from the `.env` next to `app.py`, using `python-dotenv`.

Run the app:

```powershell
.\.venv\Scripts\python.exe app.py
```

Example input:

```text
ModuleNotFoundError: No module named 'requests'
```

Each run handles one message. Empty input exits without contacting the API.
Press Ctrl+C to exit. Remove secrets from error messages before submitting them:
the message is sent to OpenAI. API calls may incur charges on your API account.

## Offline demo

After installing the dependencies, run this without a key or internet connection:

```powershell
.\.venv\Scripts\python.exe app.py --demo
```

If your project environment is active, you can also use `python app.py --demo`.
You do not need to create `.env` for this mode. The app displays:

```text
DEMO MODE — predefined response, no AI API call.
```

Enter this exact example:

```text
ModuleNotFoundError: No module named 'pandas'
```

The app prints a predefined English explanation, possible causes, and exactly
three diagnostic steps. Other messages display a notice that only this example
is supported. Empty input exits with the usual empty-message notice.
Demo mode does not read `.env`, access your key, or call the API.
You do not need to install pandas to try the demo; suggested commands are never run.
Running `python app.py` without `--demo` keeps the normal API behavior.

## Understanding app.py

`argparse` reads the `--demo` flag from the command line. `action="store_true"`
makes `args.demo` true when the flag is present. After reading input, the demo
branch prints `DEMO_RESPONSE` for `DEMO_ERROR` and uses `return` to exit before
configuration or API code can run. Both constants are predefined strings.

1. Imports provide file paths, `.env` parsing, and the official OpenAI SDK.
2. `INSTRUCTIONS` tells the model to use plain English and three response sections.
3. `main()` reads text with `input()`. `strip()` removes surrounding whitespace.
   An `if` check and `return` stop execution when input or configuration is missing.
4. `dotenv_values()` reads configuration into a dictionary. `Path(__file__)`
   locates the script so the terminal's current folder does not affect the path.
5. `with OpenAI(...)` creates the API client and closes it after use.
   Requests have a 60-second timeout and no automatic retries.
6. `client.responses.create()` sends the model name, instructions, and error text.
   `store=False` disables storing the response for later retrieval through the API.
   `response.output_text` supplies the answer that `print()` displays.
7. `try` and `except` turn API failures into readable messages without printing
   raw exceptions or the API key.
8. `if __name__ == "__main__"` starts the app when run directly, but not when
   imported by a local check.

## Project files

- `app.py`: all application logic.
- `requirements.txt`: the two required libraries.
- `.env.example`: a configuration template without a real key.
- `.gitignore`: excludes `.env`, `.venv`, and generated Python files from Git.
- `README.md`: setup instructions and code explanation.

## Verification and documentation

Local checks with a simulated API cannot verify real connectivity, model access,
or billing. A real API test requires your own key in `.env` and running the app.

Implementation references: [official Python SDK documentation](https://developers.openai.com/api/reference/python)
and [Responses API](https://developers.openai.com/api/reference/python/resources/responses/methods/create).
