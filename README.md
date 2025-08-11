# promptsapi

A Python client library for interacting with the prompts-testing API. This library simplifies making requests to both the content generation and prompt inference endpoints.

# Version 0.2.0
This release introduces significant enhancements and new functionality:
Enhanced generate_prompt function: Now supports additional parameters like prompt_mode, category, subcategory, and persona for more controlled content generation.
New reverse_prompt function: A dedicated function to interact with the API's inference endpoint, allowing you to infer information from given text or code.

# Installation
You can install promptsapi directly from PyPI using pip. If you have a previous version installed, you can upgrade using the --upgrade flag:

pip install --upgrade promptsapi

# Usage

# Generating Content with generate_prompt 
This function makes a POST request to generate content based on your raw input and a variety of optional parameters.

from promptsapi import generate_prompt

API_KEY = "YOUR_API_KEY_HERE"

API_GENERATE_URL = "https://prompts-testing-v1-0.onrender.com/api/v1/generate"

prompt_text = "write a short story about a brave knight"

prompt_mode = "text"  # Can also be 'image_gen', 'video_gen' depending on API

category = "fantasy"

subcategory = "adventure"

persona = "wise sage"

response = generate_prompt(
    api_key=API_KEY,
    api_url=API_GENERATE_URL,
    raw_input=prompt_text,
    prompt_mode=prompt_mode,
    category=category,
    subcategory=subcategory,
    persona=persona,
    timeout=15 # Optional: specify a custom timeout
)

if response:
    print("\n--- Generated Content ---")
    import json
    print(json.dumps(response, indent=2))
else:
    print("\nError: Failed to generate content.")

# Inferring Content with reverse_prompt
This new function makes a POST request to the API's inference endpoint, allowing you to infer characteristics or summaries from provided text or code.

from promptsapi import reverse_prompt

API_KEY = "YOUR_API_KEY_HERE"

API_REVERSE_URL = "https://prompts-testing-v1-0.onrender.com/api/v1/reverse"

input_text_for_inference = "def factorial(n):\n    if n == 0:\n        return 1\n    else:\n        return n * factorial(n-1)" 

response_inference = reverse_prompt(
    api_key=API_KEY,
    api_url=API_REVERSE_URL,
    input_text=input_text_for_inference,
    language_code="en-US", # Optional: specify language if needed
    prompt_mode="text",   # Optional: mode for inference (typically 'text')
    timeout=15             # Optional: specify a custom timeout
)

if response_inference:
    print("\n--- Inferred Content ---")
    import json
    print(json.dumps(response_inference, indent=2))
else:
    print("\nError: Failed to infer content.")

# API Reference
generate_prompt(api_key, api_url, raw_input, language_code="en-US", prompt_mode="text", category=None, subcategory=None, persona=None, timeout=10)

api_key (str): (Required) Your authentication API key for the service.

api_url (str): (Required) The full URL to the content generation endpoint (e.g., https://prompts-testing-v1-0.onrender.com/api/v1/generate).

raw_input (str): (Required) The primary text input or prompt for content generation.

language_code (str, optional): The language code of the raw_input. Defaults to "en-US".

prompt_mode (str, optional): The desired mode of content generation. Expected values typically include "text", "image_gen", or "video_gen". Defaults to "text".

category (str, optional): A category to help guide the content generation (e.g., "fiction", "technical"). Defaults to None.

subcategory (str, optional): A more specific subcategory within the chosen category. Defaults to None.

persona (str, optional): A persona or style to influence the generated content (e.g., "humorous", "formal"). Defaults to None.

timeout (int, optional): The maximum number of seconds to wait for the API response. Defaults to 10.

# Returns:

dict: A dictionary containing the JSON response from the API if the request is successful.

None: If an error (e.g., network timeout, HTTP error) occurs during the request. Error messages are printed to the console.

reverse_prompt(api_key, api_url, input_text, language_code="en-US", prompt_mode="text", timeout=10)

api_key (str): (Required) Your authentication API key for the service.

api_url (str): (Required) The full URL to the prompt inference endpoint (e.g., https://prompts-testing-v1-0.onrender.com/api/v1/reverse).

input_text (str): (Required) The text or code input from which to infer information.

language_code (str, optional): The language code of the input_text. Defaults to "en-US".

prompt_mode (str, optional): The mode of the input. Defaults to "text". Currently, this is primarily relevant for "text" inference.

timeout (int, optional): The maximum number of seconds to wait for the API response. Defaults to 10.

# Returns:

dict: A dictionary containing the JSON response from the API if the request is successful.

None: If an error (e.g., network timeout, HTTP error) occurs during the request. Error messages are printed to the console.

# Error Handling
Both generate_prompt and reverse_prompt include robust error handling. They catch common requests exceptions (like Timeout and HTTPError) and a general Exception. In case of an error, they will print a descriptive message to the console and return None, allowing your calling code to handle the failure gracefully.

# License
This project is licensed under the MIT License. See the LICENSE file in the repository for full details.

