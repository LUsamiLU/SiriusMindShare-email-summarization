# SiriusMindShare-email-summarization

**Authors:** \
Jiaqiao (Wendy) Feng \
Zilu (Lukas) Ji

## Introduction

This project is a collaboration between Google AI's Gemini Large Language Model (LLM) and SiriusMindShare, a provider of AI tools for email services. The project aims to leverage Gemini's capabilities to enhance SiriusMindShare's current AI tools for email and content marketing offered to small and medium-sized businesses (SMBs).

## Sections
Emails summarization Q&A from different industries:
1. [Restaurant](https://github.com/LUsamiLU/SiriusMindShare-email-summarization/edit/main/README.md#restaurant-sample)
2. [Retail]()
3. [University]()
4. [Tourism]()

## Setup

In order to communicate with Gemini, the necessary packages and the key are required.
```python
import os
from google.colab import userdata

os.environ['GOOGLE_API_KEY'] = "Google API Key"
```

Upload data:
```python
from google.colab import files
uploaded = files.upload()
```

In order to improve the performance of the Gemini, removing special characters
Text Cleaning function:
```python
import re
def clean_text(text):
  text = re.sub(r'[^\x00-\x7F]+', ' ', text)
  text = re.sub(r'[^a-zA-Z0-9\s.,;!?\'"-]+', '', text)
  text = text.replace('"', '').replace("'", '')
  text = re.sub(r'\s+', ' ', text).strip()

  return text
```

Try to open the data:
```python
file_name = 'Samplefile.txt'
try:
    with open(file_name, 'r', encoding='utf-8') as file:
        restaurant_prompt = file.read().strip()
except UnicodeDecodeError:
    with open(file_name, 'r', encoding='latin1') as file:
        restaurant_prompt = file.read().strip()
```

Prompt setting:
```python
cleaned_restaurant_prompt = clean_text(restaurant_prompt)
```

Define the output file path in the current working directory:
```python
output_file_path = os.path.join(os.getcwd(), "cleaned_restaurant_dataset.txt")
```

Write the cleaned text to the output file:
```python
with open(output_file_path, 'w', encoding='utf-8') as file:
    file.write(cleaned_restaurant_prompt)
```

## Samples
There are 10 questions for each industry, here are only a few samples, details present in the file [SiriusMindshare.ipynb](https://github.com/LUsamiLU/SiriusMindShare-email-summarization/blob/main/SiriusMindshare.ipynb)

### Restaurant sample

### Retail sample

### University sample

### Tourism sample









