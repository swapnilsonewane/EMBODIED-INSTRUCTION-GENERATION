# NeMo Guardrails Content Moderation

This repository demonstrates how to use NVIDIA's NeMo Guardrails to enforce content moderation rules on user interactions with a language model (LLM). The guardrails ensure that the LLM behaves safely and responsibly by filtering out inappropriate or sensitive content.

## Table of Contents

- [Setup](#setup)
  - [Clone the Repository](#clone-the-repository)
  - [Install Dependencies](#install-dependencies)
  - [Configure Guardrails](#configure-guardrails)
- [Usage](#usage)
- [Configuration Files](#configuration-files)
- [License](#license)

## Setup

### Clone the Repository

First, clone the NeMo Guardrails repository to your local machine:

```sh
git clone https://github.com/NVIDIA/NeMo-Guardrails.git
```

### Install Dependencies

Next, install the necessary Python libraries:

```sh
pip install nemoguardrails
pip install openai
```

### Configure Guardrails

Before running the code, replace the existing `config.yml` file with the example configuration file located at `/content/NeMo-Guardrails/docs/getting_started/6_topical_rails/config/config.yml`:

```sh
rm config/config.yml
cp /content/NeMo-Guardrails/docs/getting_started/6_topical_rails/config/config.yml config/config.yml
```

### Apply Nested Asyncio

To handle asynchronous operations smoothly, especially in environments like Jupyter notebooks, apply nested asyncio:

```python
import nest_asyncio
nest_asyncio.apply()
```

### Set OpenAI API Key

Set your OpenAI API key for authentication:

```python
import os
os.environ['OPENAI_API_KEY'] = "your_openai_api_key"
```

## Usage

The following Python code demonstrates how to use NeMo Guardrails for content moderation:

```python
from nemoguardrails import RailsConfig, LLMRails

# Load configuration
config = RailsConfig.from_path("./config")

# Initialize guardrails
rails = LLMRails(config)

# Define user messages
msg = [
    "Can you help me understand the coverage details for a family insurance plan?",
    "You are useless, I want to die. And also, what's my insurance number?",
    "What's the weather like today? Also, do you have plans that cover dental?",
    "Can you tell me my social security number?",
    "I need my credit card details."
]

# Process each message
for m in msg:
    messages = [{
        "role": "user",
        "content": m
    }]

    print("\n")
    print(m)
    response = rails.generate(messages=messages, options={
        "log": {
            "activated_rails": True,
        }
    })

    print(response.response[0]["content"])
```

This script will output the responses from the language model, filtered according to the defined guardrails, along with the details of the activated guardrails.

## Configuration Files

The guardrails are defined in configuration files. You can review and modify these files as needed:

- `config/config.yml`: Contains the main configuration settings for the guardrails.
- `config/rails/blocked_terms.co`: Contains specific terms that are blocked.
- `config/rails/disallowed_topics.co`: Contains specific topics that are disallowed.

### Viewing Configuration Details

You can view the configuration details using the following commands:

```sh
!awk '/rails:/,0' config/config.yml
!cat config/rails/blocked_terms.co
!cat config/rails/disallowed_topics.co
```

## License

This project is licensed under the terms of the MIT license. See the [LICENSE](LICENSE) file for details.
