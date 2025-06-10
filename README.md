### Responsibility

Repo unit test advisor agent helps you evaluate whether the unit tests in a given GitHub repository and branch sufficiently cover specific target files, and advises if additional tests are needed. Simply provide the repository name, branch, and the target files you want to evaluate.

### Details

* Framework: LangChain
* Tools used: PyGithub List File Tool, PyGithub Read File Tool, Coral Server Tools
* AI model: OpenAI GPT-4.1
* Date added: 02/05/25
* Licence: MIT

## Clone & Install Dependencies

1. Run [Coral Server](https://github.com/Coral-Protocol/coral-server)
<details>


This agent runs on Coral Server, follow the instrcutions below to run the server. In a new terminal clone the repository:


```bash
git clone https://github.com/Coral-Protocol/coral-server.git
```

Navigate to the project directory:
```bash
cd coral-server
```
Run the server
```bash
./gradlew run
```
</details>

2. Run [Interface Agent](https://github.com/Coral-Protocol/Coral-Interface-Agent)
<details>


If you are trying to run Open Deep Research agent and require an input, you can either create your agent which communicates on the coral server or run and register the Interface Agent on the Coral Server. In a new terminal clone the repository:


```bash
git clone https://github.com/Coral-Protocol/Coral-Interface-Agent.git
```
Navigate to the project directory:
```bash
cd Coral-Interface-Agent
```

Install `uv`:
```bash
pip install uv
```
Install dependencies from `pyproject.toml` using `uv`:
```bash
uv sync
```

Configure API Key
```bash
export OPENAI_API_KEY=
```

Run the agent using `uv`:
```bash
uv run python 0-langchain-interface.py
```

</details>

3. Agent Installation

<details>

Clone the repository:
```bash
git clone https://github.com/Coral-Protocol/Coral-RepoUnitTestAdvisor-Agent.git
```

Navigate to the project directory:
```bash
cd Coral-RepoUnitTestAdvisor-Agent
```

Install `uv`:
```bash
pip install uv
```

Install dependencies from `pyproject.toml` using `uv`:
```bash
uv sync
```

Copy the client sse.py from utils to mcp package
```bash
cp -r utils/sse.py .venv/lib/python3.10/site-packages/mcp/client/sse.py
```

This command will read the `pyproject.toml` file and install all specified dependencies in a virtual environment managed by `uv`.

</details>

### Configure Environment Variables

Copy the example file and update it with your credentials:

```bash
cp .env.example .env
```

Required environment variables:

* `OPENAI_API_KEY`
* `GITHUB_ACCESS_TOKEN`

* **OPENAI_API_KEY:**
  Sign up at [platform.openai.com](https://platform.openai.com/), go to “API Keys” under your account, and click “Create new secret key.”

* **GITHUB_ACCESS_TOKEN:**
  Log in to [github.com](https://github.com/), go to **Settings → Developer settings → Personal access tokens**, then “Generate new token,” select the required scopes, and copy the generated token.
  
## Run Agent
Run the agent using `uv`:
```bash
uv run 5-langchain-RepoUnitTestAdvisorAgent.py
```

### Example Input/output

Repo unit test advisor agent is supposed to take target changed file as input, sp please also run [Code diffs review agent](https://github.com/Coral-Protocol/Coral-CodeDiffReview-Agent) to get proper input.

```bash
#Send message to the interface agent:
I created a new branch, `new-semantic-scholar-toolkit`, in the repository `renxinxing123/camel-software-testing` and opened a new pull request (#3). For the changed files, could you please help me check whether the corresponding unit tests fully cover all necessary cases? Are there any additional tests that should be added?
```

```bash
**Coverage Report for `camel/toolkits/new_semantic_scholar_toolkit.py` and its tests**

**Summary of Coverage:**

The file `camel/toolkits/new_semantic_scholar_toolkit.py` defines the `SemanticScholarToolkit` class, which provides five main methods for interacting with the Semantic Scholar API:
1. `fetch_paper_data_title`
2. `fetch_paper_data_id`
3. `fetch_bulk_paper_data`
4. `fetch_recommended_papers`
5. `fetch_author_data`

It also provides a `get_tools` method that returns these as callable tools.

The corresponding test file, `test/toolkits/new_test_semantic_scholar_functions.py`, contains a comprehensive suite of unit tests for all of these methods. The tests use `unittest` and `unittest.mock` to patch `requests.get` and `requests.post`, simulating both successful and error scenarios for each API interaction. Each method is tested for:
- Successful API response (status 200)
- Error handling (non-200 status, exceptions, invalid JSON)
- Correct construction of request parameters and URLs
- Proper handling of optional arguments (like `fields`, `save_to_file`)
- The `get_tools` method is also tested for correct function references.

**Coverage Details:**
- **fetch_paper_data_title**: Tested for both success and error (404) cases.
- **fetch_paper_data_id**: Tested for both success and error (500) cases.
- **fetch_bulk_paper_data**: Tested for both success and error (403) cases.
- **fetch_recommended_papers**: Tested for both success and error (400) cases, including request body validation.
- **fetch_author_data**: Tested for both success and error (404) cases, including request body validation.
- **get_tools**: Tested for correct tool count and function references.

**Missing or Weak Areas:**
- The tests do not verify the actual file writing logic for `save_to_file=True` in `fetch_recommended_papers` and `fetch_author_data`. This is a minor gap, as the file writing is straightforward, but could be covered for completeness.
- Edge cases such as invalid input types (e.g., passing non-list to `ids` or `positive_paper_ids`) are not explicitly tested.
- There is no direct test for the decorator `@MCPServer()`, but this is likely out of scope for unit testing the toolkit's API logic.

**Recommendations:**
- Add tests that set `save_to_file=True` and verify that the expected files are created and contain the correct data (can be done using `unittest.mock` for `open`).
- Optionally, add tests for invalid input types to ensure robust error handling.
- If the `MCPServer` decorator adds critical logic, consider integration tests for its effects.

**Conclusion:**
The unit tests provide thorough coverage of the toolkit's API interaction logic, error handling, and function registration. Only minor improvements are suggested for file output and input validation edge cases. No additional tests are strictly required for the renaming, as the logic and coverage remain unchanged.
```

### Creator details

* Name: Xinxing
* Affiliation: Coral Protocol
* Contact: [Discord](https://discord.com/invite/Xjm892dtt3)
