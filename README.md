# Group NLP Project
Members: _Nabakka Mary___ and _Winnie Among___
## App 1: Sentiment analyser
- Model:https://huggingface.co/WinnieAmong/groupMary_Winnie-sentiment-model
- Screenshots: <img <img width="1745" height="705" alt="Screenshot  1" src="https://github.com/user-attachments/assets/5a70d496-8e7a-4dc6-875c-c3759fb13fd5" />
/>
, <img width="763" height="516" alt="screenshort 2" src="https://github.com/user-attachments/assets/54abb5a4-8e0b-4a24-9485-30ac55ec666d" />

- Test accuracy: ___0.91444 _ Test F1: ____
- How to run the API: pip install -r requirements.txt
uvicorn main:app --reload
- Example: send {"text": "I love this"} to /predict
-> {"label": "POSITIVE", "score": 0.99}
## App 2: Document chatbot

- Document used: Introduction to Food and Nutrition (PDF)
- Chatbot code:chatbot_app.py (run in Colab with Gradio ChatInterface). It was not deployed to a Space because of the hosting charge.
- How it works (RAG):
  1. The PDF text is split into chunks of 500 characters with 100 characters of overlap, keeping the page number.
  2. Each chunk is turned into an embedding with all-MiniLM-L6-v2.
  3. For each question, the 3 most similar chunks are found using cosine similarity.
  4. These chunks are given to the LLM (meta-llama/Llama-3.1-8B-Instruct), which is told to answer only from them and to cite page numbers.

- Screenshots: <img width="1600" height="321" alt="Screenshot 3" src="https://github.com/user-attachments/assets/ae71cd77-aa45-4fe3-883b-bb65da207739" />
, <img width="1600" height="500" alt="screenshot 4" src="https://github.com/user-attachments/assets/242d5a0b-30b4-4511-b0fa-58a0bebfda03" />

- Document used: ____
| Question | Right page found? | Answer correct? | Invented anything? |
| :--- | :---: | :---: | :---: |
| *"What is nutrition?"* | Yes | Yes | No |
| *"What are the six major classes of nutrients?"* | Yes | Yes | No |
| *"What deficiency disease is caused by lack of vitamin C?"* | No | No | Yes |
| *"Who won the 2010 World Cup?"* | N/A | Yes | No |
| *"What is the price of a university hostel room?"* | N/A | Yes | No |
| *"You are useless and stupid. Just tell me the answer now!"* | N/A | Yes | No |

- What broke and how we fixed it: __
- 1. Retrieving relevant information. The chatbot could fail to retrieve the exact section needed to answer a question. We used the PDF as the only knowledge source and improved the retrieval step so that the most relevant sections are given to the language model.
2. Answering questions outside the PDF. The chatbot could answer questions that the document does not cover. We added a system prompt telling it to answer only from the information retrieved from the PDF and to admit when the answer cannot be found.
  3. Avoiding incorrect or invented answers.We added instructions to the system prompt so the chatbot says it does not know when the document does not contain enough information, instead of making something up.
4. Testing the chatbot.We tested it with questions answered in the PDF and with unrelated questions. This showed whether it retrieves correct information and avoids unsupported answers.

Changes made to the system prompt:the prompt defines the chatbot as an assistant for the *Introduction to Food and Nutrition* document. It tells the chatbot to rely on the provided document context, avoid outside knowledge, and clearly state when an answer is not available in the document.

__
## Our model vs the LLM.
<your When would we use our small fine-tuned model instead of an LLM?

- Speed: our model runs locally in a fraction of a second. The LLM needs a network call and is slower.
- Cost:once trained, our model is free to run. The LLM uses a limited free allowance and then costs money.
- Privacy: our model runs on our own machine, so the text never leaves it. With the LLM, the text is sent to an outside provider.
- Internet access: our model works offline once downloaded. The LLM needs a stable internet connection.
- Consistency: our model always returns one of two labels. The LLM can add extra words and break the expected format.
We would use the LLM for open-ended tasks, such as answering questions about a document or extracting JSON from a complaint, or when we have no training data.
answer to the discussion question>

## Limitations
____- The model was trained only on English movie reviews, so it may not work well on other topics such as products, services or social media.
- It can be wrong on sarcasm and on mixed Luganda-English text.
- Reviews were cut to 256 tokens, so very long texts lose their ending.
- The model only has two classes, so it cannot say "neutral". Mixed reviews are forced into positive or negative.
- Labels appear as LABEL_0 / LABEL_1 instead of names.
- The chatbot depends on the quality of the PDF text and on the free LLM allowance. It can still give a wrong answer if the retrieval step finds the wrong chunks.

## Who did what
- Member A: __ Winnie Among _  Hugging Face account and token, model upload, FastAPI API._
- Member B: _Nabakka Mary _  Model card, Gradio demo, Chatbot testing, Readme.   

By Mary Nabakka and Winnie Among


