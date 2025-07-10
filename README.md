Welcome to Ambient Agent 101!

## Introduction
In this session, you will learn about the fundamentals of LangGraph through one of our notebooks. It builds up to an ["ambient"](https://blog.langchain.dev/introducing-ambient-agents/) agent that can manage your email with connection to the Gmail API. It's grouped into 4 sections in the notebook in `notebooks` , with accompanying code in the `src/email_assistant` directory. These section build from the basics of agents, to human-in-the-loop, and finally to memory. These all come together in an agent that we will deploy and evaluate, and the principles can be applied to other agents across a wide range of tasks. 

This is a condensed version of LangChain Academy, and is intended to be run in a session with a LangChain engineer. If you're interested in going into more depth, or working through a tutorial on your own, check out LangChain Academy here! LangChain Academy has helpful pre-recorded videos from one of our LangChain engineers.

![overview](notebooks/img/overview.png)




## Environment Setup 

### Clone the Ambient Agent 101 repo
```
git clone https://github.com/langchain-ai/langgraph-101.git
```

### Python Version

* Ensure you're using Python 3.11 or later. 
* This version is required for optimal compatibility with LangGraph. 

```shell
python3 --version
```

### API Keys

* If you don't have an OpenAI API key, you can sign up [here](https://openai.com/index/openai-api/).
* Sign up for LangSmith [here](https://smith.langchain.com/).
* Generate a LangSmith API key.

### Set Environment Variables

* Create a `.env` file in the root directory:
```shell
# Copy the .env.example file to .env
cp .env.example .env
```

### Alternative Models Instructions 

If you are using alternative models (e.g., Anthropic, Bedrock, AzureOpenAI) instead of OpenAI, there are a few things you need to do.

* Set necessary environment variables in the `.env` file.

* Navigate to [utils.py](/src/email_assistant/utils.py), and uncomment the code for the model that you are looking to run. 



### Package Installation

**Recommended: Using uv (faster and more reliable)**

```shell
# Install uv if you haven't already
pip install uv

# Install the package with development dependencies
uv sync --extra dev

# Activate the virtual environment
source .venv/bin/activate
```

**Alternative: Using pip**

```shell
$ python3 -m venv .venv
$ source .venv/bin/activate
# Ensure you have a recent version of pip (required for editable installs with pyproject.toml)
$ python3 -m pip install --upgrade pip
# Install the package in editable mode
$ pip install -e .
```

> **⚠️ IMPORTANT**: Do not skip the package installation step! This editable install is **required** for the notebooks to work correctly. The package is installed as `interrupt_workshop` with import name `email_assistant`, allowing you to import from anywhere with `from email_assistant import ...`

## Structure 

The repo is organized into the 4 sections, with accompanying code in the `src/email_assistant` directory.

* Notebook: [notebooks/ambient_agent.ipynb](/notebooks/ambient_agent.ipynb)

### Section 1. Building an ReAct agent 

* Code: [src/email_assistant/email_assistant.py](/src/email_assistant/email_assistant.py)

![overview-agent](notebooks/img/overview_agent.png)

This section shows how to build the email assistant, combining an [email triage step](https://langchain-ai.github.io/langgraph/tutorials/workflows/) with an agent that handles the email response. You can see the linked code for the full implementation in `src/email_assistant/email_assistant.py`.

![Screenshot 2025-04-04 at 4 06 18 PM](notebooks/img/studio.png)


### Section 2. Human-in-the-loop 
* Code: [src/email_assistant/email_assistant_hitl.py](/src/email_assistant/email_assistant_hitl.py)

![overview-hitl](notebooks/img/overview_hitl.png)

This section shows how to add human-in-the-loop (HITL), allowing the user to review specific tool calls (e.g., send email, schedule meeting). For this, we use [Agent Inbox](https://github.com/langchain-ai/agent-inbox) as an interface for human in the loop. You can see the linked code for the full implementation in [src/email_assistant/email_assistant_hitl.py](/src/email_assistant/email_assistant_hitl.py).

![Agent Inbox showing email threads](notebooks/img/agent-inbox.png)

### Section 3. Memory  
* Code: [src/email_assistant/email_assistant_hitl_memory.py](/src/email_assistant/email_assistant_hitl_memory.py)

This notebook shows how to add memory to the email assistant, allowing it to learn from user feedback and adapt to preferences over time. The memory-enabled assistant ([email_assistant_hitl_memory.py](/src/email_assistant/email_assistant_hitl_memory.py)) uses the [LangGraph Store](https://langchain-ai.github.io/langgraph/concepts/memory/#long-term-memory) to persist memories. You can see the linked code for the full implementation in [src/email_assistant/email_assistant_hitl_memory.py](/src/email_assistant/email_assistant_hitl_memory.py).

![overview-memory](notebooks/img/overview_memory.png)  




### [Optional for Training] Section 4. Evaluation 
* Notebook: [notebooks/evaluation.ipynb](/notebooks/evaluation.ipynb)
![overview-eval](notebooks/img/overview_eval.png)

This notebook introduces evaluation with an email dataset in [eval/email_dataset.py](/eval/email_dataset.py). It shows how to run evaluations using Pytest and the LangSmith `evaluate` API. It runs evaluation for emails responses using LLM-as-a-judge as well as evaluations for tools calls and triage decisions.

![Screenshot 2025-04-08 at 8 07 48 PM](notebooks/img/eval.png)




## Connecting to APIs  

The above notebooks using mock email and calendar tools. 

### Gmail Integration and Deployment

Set up Google API credentials following the instructions in [Gmail Tools README](src/email_assistant/tools/gmail/README.md).

The README also explains how to deploy the graph to LangGraph Platform.

The full implementation of the Gmail integration is in [src/email_assistant/email_assistant_hitl_memory_gmail.py](/src/email_assistant/email_assistant_hitl_memory_gmail.py).

## Running Tests

The repository includes an automated test suite to evaluate the email assistant. 

Tests verify correct tool usage and response quality using LangSmith for tracking.

### Running Tests with [run_all_tests.py](/tests/run_all_tests.py)

```shell
python tests/run_all_tests.py
```

### Test Results

Test results are logged to LangSmith under the project name specified in your `.env` file (`LANGSMITH_PROJECT`). This provides:
- Visual inspection of agent traces
- Detailed evaluation metrics
- Comparison of different agent implementations

### Available Test Implementations

The available implementations for testing are:
- `email_assistant` - Basic email assistant

### Testing Notebooks

You can also run tests to verify all notebooks execute without errors:

```shell
# Run all notebook tests
python tests/test_notebooks.py

# Or run via pytest
pytest tests/test_notebooks.py -v
```

