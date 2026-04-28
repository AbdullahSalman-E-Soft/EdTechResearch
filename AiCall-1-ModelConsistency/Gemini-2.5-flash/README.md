
# Test Overview

I tried testing gemini-2.5-flash for consistent outputs

For that purpose, I would inference it twice with the same input and system prompt.

This was a done for varying temperatures, the result of which is recorded as following.

The system prompt is as following:

```
SYSTEM_PROMPT = """
You are an expert GCSE English writing analyst.
Your task is to analyze the student's writing and extract measurable features.
Do NOT assign a grade. Do NOT produce feedback. Extract features ONLY.

Return ONLY valid JSON in the following structure:
{
  "ideaClarityScore": number (1-10),
  "structureScore": number (1-10),
  "vocabularyRange": number (1-10),
  "sentenceVariety": number (1-10),
  "languageTechniques": string[],
  "grammarErrors": number,
  "coherenceIssues": number,
  "wordCount": number
}
Return no other text. No preamble. No markdown fences.
"""
```

The following input was used across all examples

```
"The cat sat on the mat. It was a nice day. The sun was shining brightly, and the birds were singing. I like cats because they are cute."
```


# Experiment

The following temperatures were tried:

- 0.7
- 0.5
- 0.35
- 0.3

## TEMPERATURE = 0.7

Attempt 1:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{
  "ideaClarityScore": 7,
  "structureScore": 4,
  "vocabularyRange": 2,
  "sentenceVariety": 6,
  "languageTechniques": [
    "Sensory imagery",
    "Descriptive language"
  ],
  "grammarErrors": 0,
  "coherenceIssues": 0,
  "wordCount": 24
}
```

Attempt 2:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{"ideaClarityScore": 3, "structureScore": 2, "vocabularyRange": 1, "sentenceVariety": 2, "languageTechniques": [], "grammarErrors": 0, "coherenceIssues": 0, "wordCount": 29}
```


## TEMPERATURE = 0.5

Attempt 1:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{
  "ideaClarityScore": 3,
  "structureScore": 2,
  "vocabularyRange": 2,
  "sentenceVariety": 5,
  "languageTechniques": [],
  "grammarErrors": 0,
  "coherenceIssues": 0,
  "wordCount": 26
}
```

Attempt 2:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{
  "ideaClarityScore": 3,
  "structureScore": 2,
  "vocabularyRange": 1,
  "sentenceVariety": 3,
  "languageTechniques": [],
  "grammarErrors": 0,
  "coherenceIssues": 1,
  "wordCount": 29
}
```


## TEMPERATURE = 0.35

Attempt 1:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{"ideaClarityScore": 2, "structureScore": 2, "vocabularyRange": 1, "sentenceVariety": 1, "languageTechniques": [], "grammarErrors": 0, "coherenceIssues": 0, "wordCount": 30}
```

Attempt 2:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{"ideaClarityScore": 2, "structureScore": 3, "vocabularyRange": 1, "sentenceVariety": 4, "languageTechniques": [], "grammarErrors": 0, "coherenceIssues": 0, "wordCount": 29}
```

Attempt 3:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{"ideaClarityScore": 2, "structureScore": 2, "vocabularyRange": 1, "sentenceVariety": 3, "languageTechniques": [], "grammarErrors": 0, "coherenceIssues": 1, "wordCount": 30}
```


## Temperature = 0.3

Attempt 1:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{"ideaClarityScore": 3, "structureScore": 2, "vocabularyRange": 1, "sentenceVariety": 4, "languageTechniques": [], "grammarErrors": 0, "coherenceIssues": 0, "wordCount": 25}
```

Attempt 2:

```
(.venv) PS C:\Users\abdullah.salman\Desktop\MEOW> python .\try.py
{"ideaClarityScore": 2, "structureScore": 2, "vocabularyRange": 1, "sentenceVariety": 4, "languageTechniques": [], "grammarErrors": 0, "coherenceIssues": 0, "wordCount": 30}
```




# Experiment Source Code

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_core.messages import SystemMessage, HumanMessage

SYSTEM_PROMPT = """
You are an expert GCSE English writing analyst.
Your task is to analyze the student's writing and extract measurable features.
Do NOT assign a grade. Do NOT produce feedback. Extract features ONLY.

Return ONLY valid JSON in the following structure:
{
  "ideaClarityScore": number (1-10),
  "structureScore": number (1-10),
  "vocabularyRange": number (1-10),
  "sentenceVariety": number (1-10),
  "languageTechniques": string[],
  "grammarErrors": number,
  "coherenceIssues": number,
  "wordCount": number
}
Return no other text. No preamble. No markdown fences.
"""

llm = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash",
    temperature=0.2,
    api_key="AIzaSyBmlxZCkFRdPdbHYqQk3NEKCFZzJvEaXOI"
)

def analyze_writing(student_writing: str) -> str:
    messages = [
        SystemMessage(content=SYSTEM_PROMPT),
        HumanMessage(content=student_writing),
    ]
    response = llm.invoke(messages)
    return response.content

if __name__ == "__main__":
    student_writing = "The cat sat on the mat. It was a nice day. The sun was shining brightly, and the birds were singing. I like cats because they are cute."
    result = analyze_writing(student_writing)
    print(result)

```

# Conclusion

The model remained slightly inconsistent across the range of temperature 0.7 - 0.3