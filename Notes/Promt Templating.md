
A chat model is an LLM finetuned for chat. It is train Ned on data in certain way and this formatted
formating is basically the chat template. In the formatting, we will have some rose like the user, the assistant ETA. A rose is just a token which the model sees during the training.
In hugging Face tokenizen, we can access to a template by calling apply-chat-template methods and we can see the template in the repo by looking at the file chat-template ninja
Eg: Use LFM2.

Apply chat template
We need to indicate the rose in a form of diet as
follows:
- user: the user message
- assistant: the model response - system: the instruction to guide how model should act I at the beginning sometimes I
We can add tokenire flag, add-generation-prompt on enable-thinking in reasoning model. Add- generation_ prompt.
-True: include the start of assistant message at the end and useful to tell the model that we expect it response next.
- False: During training and does not include the start of model response.
Model Training
using the model chat template is good to match with tokens that the model is trained on.
- If we template and not tokenize, we apply
must remove the start token - Hf we tokenize, we don't need to do remove.

Unclothe.
Unclothe template can handle template differently from hugging face.
We can get it by importing unsloth.chat-temp We can see all supported template using CHAT-TEMPLATES. Reys ().
It will patch the tokenizer and give use apply - chat template method from Hugging face.

Take example that Chat GPT can underst and multi turn cover station.
Tim plates
It tells that it is similar mark-up in XML. • Chains is the most popular. •Another example by Zephyr
How to use them?
- He showed how chat template is saved in transforma - Give an exam
slt of jingu
- He showed that we can apply using tokenizer apply-chat-template and we can read more using HF docs.
Importance of templates
He showed how LLM behave without template, donc by Daniel Fumant on Git Hub.