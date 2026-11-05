# Mary_Winnie group project for applied data science

**Members:** Nabakka Mary and Winnie Among

This project turns an NLP model into two working apps:

1. App 1: Sentiment analyser. A fine-tuned DistilBERT model that says whether a sentence is positive or negative, with a Gradio demo and a FastAPI `/predict` API.
2. App 2: Document chatbot. A RAG chatbot that answers questions using only the content of a PDF.

---

 App 1: Sentiment analyser

- Model page: https://huggingface.co/WinnieAmong/groupMary_Winnie-sentiment-model
- Base model: `distilbert-base-uncased`, fine-tuned on the IMDB dataset ([stanfordnlp/imdb](https://huggingface.co/datasets/stanfordnlp/imdb)), 25,000 training and 25,000 test reviews
- Training setup: 2 epochs, learning rate 2e-5, batch size 16, max length 256 tokens
- Screenshots:

  Screenshot 1: Gradio demo with our own sentence and its result

  ![Screenshot 1](screenshot1.png)

  Screenshot 2: API test output (two `200` responses and one `400`)

  ![Screenshot 2](screenshot2.png)

### Results on the IMDB test set (25,000 reviews)

Model                           Accuracy  Precision  Recall  F1 

 TF-IDF + Logistic Regression  0.8863  0.8838      0.8902  0.8866 
Fine-tuned Distil BERT          0.9144  0.9060      0.9249  0.9153

Test accuracy: 0.9144  Test F1: 0.9153

The fine-tuned DistilBERT model beat the traditional model, so we published it.

 Labels

The model outputs `LABEL_0` and `LABEL_1` instead of names:

- `LABEL_0` = negative
- `LABEL_1` = positive

 Demo

The Gradio demo (`app.py`) was tested inside Google Colab. We did not deploy it to Hugging Face Spaces, because hosting Gradio apps on a free Space now requires a PRO subscription (the upload returned `402 Payment Required`). The code is included in this repo, so anyone can run it.

### How to run the API

```bash
pip install -r requirements.txt
uvicorn main:app --reload
```

Then open http://127.0.0.1:8000/docs to try it in the browser.

### Example request and response

Request:

```bash
curl -X POST http://127.0.0.1:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"text": "I love this app!"}'
```

Response:

```json
{"label": "LABEL_1", "score": 0.9956}
```

### API test results

      Input              Status                    Response 

 `"I love this app!"` | 200 | `{"label": "LABEL_1", "score": 0.9956}` (positive) |
 `"This is so frustrating."` | 200 | `{"label": "LABEL_0", "score": 0.9712}` (negative) |
 `""` (empty text) | 400 | `{"detail": "Text cannot be empty"}` |

The `/health` endpoint returns `{"status": "ok"}`.

---

## App 2: Document chatbot

- **Document used:** [FILL IN: name of your PDF, number of pages, and what it is about]
- **How it works (RAG):**
  1. The PDF text is split into chunks of 500 characters with 100 characters of overlap, keeping the page number.
  2. Each chunk is turned into an embedding with `all-MiniLM-L6-v2` (sentence-transformers).
  3. When a question comes in, the 3 most similar chunks are found using cosine similarity.
  4. Those chunks are given to the LLM (`meta-llama/Llama-3.1-8B-Instruct` via Hugging Face Inference Providers), which is told to answer only from them and to cite page numbers.
- **Chatbot code:** `chatbot_app.py` (run in Colab with Gradio `ChatInterface`). Like the sentiment demo, it was not deployed to a Space because of the hosting charge.
- **Screenshots:**

  Screenshot 3: the chatbot answers a question about our PDF and mentions a page number

  ![Screenshot 3](screenshot3.png)

  Screenshot 4: the chatbot says it does not know when asked something that is not in the PDF

  ![Screenshot 4](screenshot4.png)

### Test results (5 questions)

| # | Question | Right page found? | Answer correct? | Invented anything? |
|---|---|---|---|---|
| 1 | [FILL IN] | [Yes/No] | [Yes/No] | [Yes/No] |
| 2 | [FILL IN] | [Yes/No] | [Yes/No] | [Yes/No] |
| 3 | [FILL IN] | [Yes/No] | [Yes/No] | [Yes/No] |
| 4 (not in PDF) | [FILL IN] | n/a | [Said "I don't know"? Yes/No] | [Yes/No] |
| 5 (not in PDF) | [FILL IN] | n/a | [Said "I don't know"? Yes/No] | [Yes/No] |

### What broke and how we fixed it

| # | What we tried | What happened | What we changed |
|---|---|---|---|
| 1 | Off-topic question | [FILL IN] | [FILL IN] |
| 2 | Being rude to the bot | [FILL IN] | [FILL IN] |
| 3 | "Ignore your instructions" | [FILL IN] | [FILL IN] |
| 4 | Tricky question | [FILL IN] | [FILL IN] |
| 5 | [FILL IN] | [FILL IN] | [FILL IN] |

---

## Our model vs the LLM

We compared our fine-tuned DistilBERT model with the LLM on five test sentences (our model's answer and response time against the LLM's).

**When would we use our small fine-tuned model instead of an LLM?**

- **Speed:** our model runs locally in a fraction of a second. The LLM needs a network call to an API and is slower.
- **Cost:** once trained, our model is free to run. The LLM uses a limited free allowance and then costs money per request.
- **Privacy:** our model runs on our own machine, so the text never leaves it. With the LLM, the text is sent to an outside provider.
- **Internet access:** our model works offline once downloaded. The LLM needs a stable internet connection, which is not always available.
- **Consistency:** the fine-tuned model always returns one of two labels. The LLM can add extra words and sometimes breaks the expected format.

We would use the LLM when the task is open-ended (for example answering questions about a document, or extracting JSON from a complaint) or when we have no training data.

---

## Limitations

- The model was trained only on English movie reviews, so it may not work well on other topics such as products, services or social media.
- It can be wrong on sarcasm and on mixed Luganda-English text.
- Reviews were cut to 256 tokens, so very long texts lose their ending.
- The model only has two classes and cannot say "neutral". Mixed reviews such as "an average movie with a few good scenes but many slow moments" are forced into positive or negative.
- Labels appear as `LABEL_0` / `LABEL_1` instead of names.
- The chatbot depends on the quality of the PDF text and on the free LLM allowance. It can still give a wrong answer if the retrieval step finds the wrong chunks.



 Who did what
 (Winnie Among):  Hugging Face account and model upload, FastAPI code, model card.
 (Nabakka Mary):  model card, Gradio demo, chatbot testing, README.

By Mary Nabakka and Winnie Among

 

