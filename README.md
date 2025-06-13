## [Coral RepoUnitTestAdvisor Agent](https://github.com/Coral-Protocol/Coral-RepoUnitTestAdvisor-Agent)

The RepoUnitTestAdvisor Agent helps you evaluate whether the unit tests in a given GitHub repository and branch sufficiently cover specific target files, and advises if additional tests are needed.

## Responsibility
The RepoUnitTestAdvisor Agent analyzes test coverage for target files in a repository and branch, and recommends additional tests if needed.

## Details
- **Framework**: LangChain
- **Tools used**: PyGithub List File Tool, PyGithub Read File Tool, Coral Server Tools
- **AI model**: OpenAI GPT-4.1
- **Date added**: 02/05/25
- **License**: MIT

## Use the Agent  

### 1. Clone & Install Dependencies


<details>  

Ensure that the [Coral Server](https://github.com/Coral-Protocol/coral-server) is running on your system and the [Interface Agent](https://github.com/Coral-Protocol/Coral-Interface-Agent) is running on the Coral Server.  

```bash
# Clone the RepoUnitTestAdvisor Agent repository
git clone https://github.com/Coral-Protocol/Coral-RepoUnitTestAdvisor-Agent.git

# Navigate to the project directory
cd Coral-RepoUnitTestAdvisor-Agent

# Install `uv`:
pip install uv

# Install dependencies from `pyproject.toml` using `uv`:
uv sync
```
This command will read the `pyproject.toml` file and install all specified dependencies in a virtual environment managed by `uv`.

</details>

### 2. Configure Environment Variables
<details>

Get the API Keys:
- [OpenAI API Key](https://platform.openai.com/api-keys)
- [GitHub Personal Access Token](https://github.com/settings/tokens)

Create a .env file in the project root:
```bash
cp -r .env.example .env
```

Add your API keys and any other required environment variables to the .env file.

Required environment variables:
- `OPENAI_API_KEY`
- `GITHUB_ACCESS_TOKEN`

</details>

### 3. Run Agent
<details>

Run the agent using `uv`:
```bash
uv run 5-langchain-RepoUnitTestAdvisorAgent.py
```
</details>

### 4. Example
<details>

```bash
# Input:
Could you please help me check if the unit test file for new_semantic_scholar_toolkit.py in the branch `new-semantic-scholar-toolkit` of the repo `renxinxing123/camel-software-testing` fully cover all necessary cases? Are there any additional tests that should be added?

# Output:
**Coverage Summary for `camel/toolkits/new_semantic_scholar_toolkit.py`:**

**Target File Overview:**
This file defines the `SemanticScholarToolkit` class, which provides methods to interact with the Semantic Scholar API, including:
- `fetch_paper_data_title`
- `fetch_paper_data_id`
- `fetch_bulk_paper_data`
- `fetch_recommended_papers`
- `fetch_author_data`
- `get_tools`

**Associated Unit Test File:**
- `test/toolkits/new_test_semantic_scholar_functions.py`

**Test Coverage Analysis:**
- **fetch_paper_data_title**: Tested for both successful (200) and error (non-200) responses, including correct parameter passing and error handling.
- **fetch_paper_data_id**: Tested for successful and error responses, including correct URL and parameter usage, and error message propagation.
- **fetch_bulk_paper_data**: Tested for both successful and error responses, including parameter checks and error handling.
- **fetch_recommended_papers**: Tested for successful and error (non-200) responses, including correct request body, URL, and error handling.
- **fetch_author_data**: Tested for both successful and error responses, including correct request body, parameter passing, and error handling.
- **get_tools**: Explicitly tested to ensure all toolkit functions are included and correctly referenced.

**Aspects Covered:**
- Typical use cases (successful API responses)
- Error handling for non-200 responses and exceptions
- Parameter and URL correctness
- Mocking of external dependencies (`requests.get` and `requests.post`)
- Toolkit registration and function referencing

**Potential Gaps:**
- Edge cases for malformed or unexpected API responses (e.g., invalid JSON, network timeouts) are partially covered via exception handling, but not all possible error scenarios (like timeouts or malformed data) are explicitly tested.
- File saving logic (`save_to_file=True` in `fetch_recommended_papers` and `fetch_author_data`) is not tested; tests always use `save_to_file=False`.
- No tests for very large input lists or stress testing (e.g., very large `positive_paper_ids` or `ids`).
- No tests for optional `fields` parameter variations (e.g., passing custom field lists).

**Recommendations:**
- Add tests for the `save_to_file=True` scenario to ensure file writing works and handles file I/O errors gracefully.
- Add tests for malformed JSON responses (simulate API returning invalid JSON).
- Add tests for network timeouts and other `requests` exceptions (e.g., `Timeout`, `ConnectionError`).
- Add tests for custom `fields` parameter values to ensure correct query construction.
- Consider adding stress tests for large input lists if performance or memory is a concern.

**Conclusion:**
The current unit tests provide strong coverage of the main functionality and error handling for the toolkit. Addressing the above recommendations would further strengthen robustness, especially for edge cases and file I/O.
```
</details>

## Creator Details
- **Name**: Xinxing
- **Affiliation**: Coral Protocol
- **Contact**: [Discord](https://discord.com/invite/Xjm892dtt3)
