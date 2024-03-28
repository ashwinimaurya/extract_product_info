# Extracting Nutrition from product details

Using Langchain and LLM to extract nutritional info from product names



LLMs can help to answer the contextual infortaion such as ingredients in some item upto great extent. For example consider the following product that we would like to extract the products nutritions.

> question='can you plz find me ingredients for items in triple quotes """poppi ginger lime prebiotic soda""" '

```ruby
import openai, os, pandas as pd
from dotenv import load_dotenv, find_dotenv
from langchain.prompts import ChatPromptTemplate
import json
import time

import pandas as pd
pd.set_option('display.max_rows', 500)
pd.set_option('display.max_columns', 500)
pd.set_option('display.width', 1000)
pd.set_option('max_colwidth', 1000)


_=load_dotenv(find_dotenv())
openai.api_key = os.environ['OPENAI_API_KEY']

llm_model_3 = "gpt-3.5-turbo"    
llm_model_4='gpt-4'    

def get_product_template(product, model):
    
    from langchain.chat_models import ChatOpenAI
    
    review_template = """Take a deep breath. For the following product, extract the following information and output as python dictionary:

                        kwd:              List the top ingredients used to make this product.

                        nutritions:       List the top nutritions found in this product.

                        text: {product}

                        """
        
    prompt = ChatPromptTemplate.from_template(template=review_template)
    
    messages = prompt.format_messages(product=product)
    
    chat_model= ChatOpenAI(temperature=0.2, model=model)
    
    response = chat_model(messages)

    return response.content


                        """
        
    prompt = ChatPromptTemplate.from_template(template=review_template)
    
    messages = prompt.format_messages(text=customer_review)
    
    chat_model= ChatOpenAI(temperature=0.2, model=model)
    
    response = chat_model(messages)

    return response.content


def get_formatted_response(res):
    return json.loads(res)


#### Calls to LLMs GPT Turbo 3.5 and GPT 4

st=time.time()

resp_31=get_product_template(question_1,model=llm_model_3)
resp_32=get_product_template(question_1,model=llm_model_4)

en=time.time()

print(f'time take {en-st}')

re_nutr=pd.DataFrame()
re_nutr['review_text']=[question]
re_nutr['kwd_gpt_3.5']=[get_formatted_response(resp_31)['kwd']]
re_nutr['kwd_gpt_4']=[get_formatted_response(resp_32)['kwd']]
re_nutr['nutritions_gpt_3.5']=[get_formatted_response(resp_31)['nutritions']]
re_nutr['nutritions_gpt_4']=[get_formatted_response(resp_32)['nutritions']]

re_nutr
'''

<img width="1298" alt="image" src="https://github.com/ashwinimaurya/extract_product_info/assets/13278692/5aa7346b-86d7-4856-be9c-44e3e1624f35">
