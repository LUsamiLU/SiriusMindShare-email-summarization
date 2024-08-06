# SiriusMindShare-email-summarization

**Authors:** \
Jiaqiao (Wendy) Feng \
Zilu (Lukas) Ji

## Introduction

This project is a collaboration between Google AI's Gemini Large Language Model (LLM) and SiriusMindShare, a provider of AI tools for email services. The project aims to leverage Gemini's capabilities to enhance SiriusMindShare's current AI tools for email and content marketing offered to small and medium-sized businesses (SMBs).

## Sections
Emails summarization Q&A from different industries:
1. [Restaurant](https://github.com/LUsamiLU/SiriusMindShare-email-summarization/tree/main?tab=readme-ov-file#restaurant-sample)
2. [Retail](https://github.com/LUsamiLU/SiriusMindShare-email-summarization/tree/main?tab=readme-ov-file#retail-sample)
3. [University](https://github.com/LUsamiLU/SiriusMindShare-email-summarization/tree/main?tab=readme-ov-file#university-sample)
4. [Tourism](https://github.com/LUsamiLU/SiriusMindShare-email-summarization/tree/main?tab=readme-ov-file#tourism-sample)

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
Sample question:\
What promotion strategies in these provided restaurant emails can attract more users?
```bash
cleaned_restaurant_text=$(<cleaned_restaurant_dataset.txt)
json_data='{
  "contents": [{
    "parts": [{
      "text": "What promotion strategies in these provided restaurant emails can attract more users? \n Dataset: '"$cleaned_restaurant_text"'"
    }]
  }]
}'
curl https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key="AIzaSyAKTNGRrO7Wg7STVoy79fQ1HYdRtkpw6Zg" \
    -H 'Content-Type: application/json' \
    -X POST \
    -d "$json_data" 2> /dev/null | grep text
```
Answer:\
            "text": "**Effective Promotion Strategies**\n\n**1. Exclusive Deals and Discounts:**\n* Offer limited-time exclusive deals and discounts for first-time mobile orders (McDonalds, Habit Burger Grill).\n* Provide free meals or discounts for reaching certain spending milestones (Denny's, Kura Revolving Sushi Bar).\n\n**2. Loyalty Programs:**\n* Reward customers with points, tiers, and exclusive benefits through membership programs (Chick-fil-A, Kura Revolving Sushi Bar).\n* Offer discounts or free items to members (Popeyes, Denny's).\n\n**3. Freebies and Deals:**\n* Offer free items with minimum purchases (Denny's, Popeyes).\n* Provide discounts on delivery or 0 delivery fees (Uber Eats, Postmates, Uber One).\n* Host events with freebies or special offers (Denny's, Boathouse at Hendrys Beach).\n\n**4. Targeted Promotions:**\n* Segment customers and offer promotions based on their preferences (Safeway, Yelp).\n* Provide offers for specific events or holidays (Universal Studios Hollywood).\n\n**5. Partnerships and Collaborations:**\n* Partner with other businesses to offer joint promotions (Amazon.com, Grubhub).\n* Offer discounts or exclusive deals to users of specific apps or payment methods (Uber Eats, Uber One).\n\n**6. Contests and Sweepstakes:**\n* Run contests or sweepstakes to generate excitement and attract new customers (Kura Revolving Sushi Bar).\n\n**7. Personalized Recommendations and Content:**\n* Use algorithms to provide personalized recommendations and content to customers based on their preferences (Yelp).\n* Share recipes, articles, and other engaging content to foster connection with customers (Amazon.com)."

### Retail sample
Sample question:\
What are the differences in strategies between retail emails targeted at members versus those targeted at the general audience?
```bash
cleaned_retail_text=$(<cleaned_retail_prompt.txt)
json_data='{
  "contents": [{
    "parts": [{
      "text": "What are the differences in strategies between retail emails targeted at members versus those targeted at the general audience?  \n Dataset: '"$cleaned_retail_text"'"
    }]
  }]
}'
curl https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key="AIzaSyAKTNGRrO7Wg7STVoy79fQ1HYdRtkpw6Zg" \
    -H 'Content-Type: application/json' \
    -X POST \
    -d "$json_data" 2> /dev/null | grep text
```
Answer:\
            "text": "**Member-Targeted Emails**\n\n* **Personalized subject lines:** Addresses members by name or includes exclusive offers for members. (e.g., \"Final chance for our VIPs\", \"Exclusive daily deals celebrating you!\")\n* **Rewards and incentives:** Offers exclusive discounts, rewards points, or chances to win prizes for members only. (e.g., \"Win Free AEAerie Styles!\", \"Realreward member love week.\")\n* **Member-specific content:** Provides content tailored to members' previous purchases or preferences. (e.g., \"Your passion is our passion and helps drive every move we make.\")\n* **Invites to exclusive events or experiences:** Offers members access to special events, workshops, or insider experiences. (e.g., \"Dearest AE Aerie family, As we prepare to launch our first ever Real Rewards Member Love Week...\")\n* **Calls to action for engagement:** Encourage members to interact with the brand through surveys, reviews, or social media. (e.g., \"many of you take time to share your thoughts and tell us what we need to hear.\")\n\n**General Audience-Targeted Emails**\n\n* **Catchy subject lines:** Designed to grab attention and evoke a sense of urgency or curiosity. (e.g., \"4 HRS ONLY! Extra 10 off almost everything\", \"Knock knock Its your new spring favorites!\")\n* **Sales and promotions:** Offers discounts, special deals, or limited-time offers for the general audience. (e.g., \"extra 10 off almost everything.\", \"Extra 40 OFF all SALE ITEMS\")\n* **Product highlights:** Showcases specific products or collections, featuring their unique features and benefits. (e.g., \"Introducing The Cow Print Collection\", \"The color edit\")\n* **General product recommendations:** Offers a curated selection of products based on trends or seasonal themes. (e.g., \"spring accessories ??\", \"The top of the hour\")\n* **Call to action for purchases:** Encourages the general audience to make purchases by providing direct links to product pages or shopping carts. (e.g., \"Shop now.\", \"Shop the pool-to-party collection.\")"


### University sample
Sample question:\
Are there any ethical or compliance issues related to the content or targeting of these promotions?
```bash
cleaned_retail_text=$(<cleaned_retail_prompt.txt)
json_data='{
  "contents": [{
    "parts": [{
      "text": "Are there any ethical or compliance issues related to the content or targeting of these promotions? \n Dataset: '"$cleaned_retail_text"'"
    }]
  }]
}'
curl https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key="AIzaSyAKTNGRrO7Wg7STVoy79fQ1HYdRtkpw6Zg" \
    -H 'Content-Type: application/json' \
    -X POST \
    -d "$json_data" 2> /dev/null | grep text
```
Answer:\
            "text": "The promotions may target transfer students who are applying or who have been accepted to the university. The promotions may also target alumni of the university. The content of the promotions may include information about the university's programs, faculty, and research opportunities. The promotions may also include testimonials from current or former students. The targeting of the promotions may be based on factors such as the student's academic record, interests, and demographics. The promotions may also be targeted based on the student's location or relationship to the university."

### Tourism sample
Sample question:\
What are the differences in strategies between these tourism emails targeted at members versus those targeted at the general audience?
```bash
cleaned_tourism_text=$(<cleaned_tourism_dataset.txt)
json_data='{
  "contents": [{
    "parts": [{
      "text": "What are the differences in strategies between these tourism emails targeted at members versus those targeted at the general audience? \n Dataset: '"$cleaned_tourism_text"'"
    }]
  }]
}'
curl https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key="AIzaSyAKTNGRrO7Wg7STVoy79fQ1HYdRtkpw6Zg" \
    -H 'Content-Type: application/json' \
    -X POST \
    -d "$json_data" 2> /dev/null | grep text
```
Answer:\
            "text": "**Tourism emails targeted at members:**\n\n* **Personalized experiences:** Emphasize exclusive access to VIP experiences, special offers, and personalized recommendations tailored to the member's interests and travel preferences.\n* **Member-only promotions:** Provide exclusive discounts, loyalty rewards, and early access to new tours and experiences.\n* **Community building:** Foster a sense of community among members by sharing travel tips, hosting online events, and offering opportunities for member-only meetups and networking.\n* **Content tailored to member interests:** Share curated articles, blog posts, and videos related to the destinations and activities that members are most interested in.\n\n**Tourism emails targeted at the general audience:**\n\n* **Destination highlights:** Showcase popular destinations, iconic landmarks, and must-see attractions to inspire wanderlust and generate interest.\n* **Call-to-action:** Encourage bookings by providing clear links to booking pages and highlighting time-sensitive promotions or limited-time offers.\n* **Social proof:** Include testimonials, reviews, and social media mentions to build credibility and establish the brand as a reliable and trusted source of travel information.\n* **General travel tips:** Offer practical advice on topics such as travel planning, packing essentials, and navigating unfamiliar destinations to enhance the overall travel experience."









