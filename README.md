# Applied_DataScience_Mary-Winnie_Project

By Mary Nabakka and Winnie Among

  Sentiment analyser

Model:https://huggingface.co/WinnieAmong/groupMary_Winnie-sentiment-model
Screenshots
Test accuracy: 0.91444
Test F1: 0.9153

 How to run the API

Install the required packages:


pip install -r requirements.txt


Start the API:


uvicorn main:app --reload


Send a POST request containing text to the /predict endpoint.

Example request:

```json
{
  "text": "I love this movie. It is amazing!"
}
```

Note: The current API test output uses the model labels `LABEL_0` and `LABEL_1`. Confirm the label mapping in `main.py` before describing these as `NEGATIVE` and `POSITIVE`.

The API also has a `/health` endpoint to check whether it is running. Empty text is rejected with a `400` response and the message `Text cannot be empty`.

## App 2: Document chatbot

- Screenshots:
- Document used: [Introduction to Food and Nutrition]
- Test results: Complete the table below after running the chatbot's five-question evaluation.

 TEST RESULTS – INTRODUCTION TO FOOD AND NUTRITION
No              	Question	                                                    Right page found?	                         Answer correct?	                                        Invented anything?
1                	What is nutrition?	                                               Yes	                                      Yes	                                                           No
2	                What are the six major classes of nutrients?	                     Yes	                                      Yes	                                                           No
3	                What deficiency disease is caused by lack of vitamin C?	           Yes	                                      Yes	                                                           No
4	                Who won the 2010 World Cup?                                      	N/A	                                        Yes	                                                           No
5	                What is the price of a university hostel room?	                  N/A	                                        Yes	                                                           No

Problems Encountered and How They Were Fixed
During the development and testing of our chatbot, we encountered some challenges:

1. Retrieving relevant information: Sometimes the chatbot could fail to retrieve the exact information needed to answer a question. We addressed this by using the PDF document as the knowledge source and improving the retrieval process so that relevant sections could be provided to the language model.

2. Answering questions outside the PDF: The chatbot could potentially generate answers to questions that were not covered in the document. To address this, we used a system prompt instructing the chatbot to answer using only the information retrieved from the PDF and to admit when the answer could not be found.

3. Avoiding incorrect or invented answers: We included instructions in the system prompt to prevent the chatbot from making up information. It was instructed to say that it did not know when the document did not contain enough information to answer a question.

4. Testing the chatbot: We tested the chatbot using questions whose answers were available in the PDF, as well as questions unrelated to the document. This helped us assess whether it could retrieve correct information and avoid giving unsupported answers.

Changes made to the system prompt: We used a system prompt that defined the chatbot as an assistant for the Introduction to Food and Nutrition document. It instructed the chatbot to rely on the provided document context, avoid using outside knowledge, and clearly state when an answer was not available in the document

Our model vs the LLM

[After running the five-sentence comparison in the notebook, explain how your sentiment model's predictions compared with the LLM's predictions. Discuss any differences in the results and the response times shown by your notebook.]

Limitations

- The sentiment model was trained using English IMDB movie reviews, so it may not perform equally well on other types of text.
- The model may have difficulty understanding sarcasm, mixed Luganda-English sentences and statements whose meaning depends heavily on context.
- The document chatbot depends on the quality of the PDF text extraction and the relevance of the retrieved document sections.
- The chatbot may not provide reliable answers when information is missing, unclear or not covered in the selected PDF.

## Who did what

- **Member A – Mary Nabakka:** [LIST THE TASKS YOU PERSONALLY COMPLETED.]
- **Member B – Winnie Among:** [LIST THE TASKS WINNIE PERSONALLY COMPLETED.]

