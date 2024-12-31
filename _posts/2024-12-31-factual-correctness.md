# Factual Correctness

Date: December 30, 2024

**FactualCorrectness** is a metric that compares and evaluates the factual accuracy of the generated `response` and `reference`.

Range: 0 → 1

How to do: To measure the alignment between the `response` and `reference`, the metric uses the LLM for first break down the response and reference into *claims* and then uses natural language inference to determine the facutal overlap `response` and `reference`.

— Using Precision, Recall and F1 score and might be controlled using the `mode` parameter.

### Controlling the number of claims

This section, I copy from Ragas document for explaining how to control the number of claims in the method.

In claim decomposition, two important parameters influence the output:

1. **Atomicity**
2. **Coverage**

These parameters help control the granularity and completeness of the generated claims.

### **Atomicity**

**Atomicity** refers to how much a sentence is broken down into its smallest, meaningful components. It can be adjusted based on whether you need highly detailed claims or a more consolidated view.

- **High Atomicity**: The sentence is broken down into its fundamental, indivisible claims. This results in **multiple**, smaller claims, each representing a distinct piece of information.

**Example:**

 - Original Sentence: 

- "Albert Einstein was a German theoretical physicist who developed the theory of relativity and contributed to quantum mechanics." 

- Decomposed Claims: 

- "Albert Einstein was a German theoretical physicist." 

- "Albert Einstein developed the theory of relativity." 

- "Albert Einstein contributed to quantum mechanics."

- **Low Atomicity**: The sentence is kept more **intact**, resulting in fewer claims that may contain multiple pieces of information.

**Example:** 

- Original Sentence: 

- "Albert Einstein was a German theoretical physicist who developed the theory of relativity and contributed to quantum mechanics." 

- Decomposed Claims: 

- "Albert Einstein was a German theoretical physicist who developed the theory of relativity and contributed to quantum mechanics."

### **Coverage**

**Coverage** refers to how comprehensively the claims represent the information in the original sentence. It can be adjusted to either include all details or to generalize the content.

- **High Coverage**: The decomposed claims capture all the information present in the original sentence, preserving every detail.

**Example:** 

- Original Sentence: 

- "Marie Curie was a Polish and naturalized-French physicist and chemist who conducted pioneering research on radioactivity." 

- Decomposed Claims: 

- "Marie Curie was a Polish physicist." 

- "Marie Curie was a naturalized-French physicist." 

- "Marie Curie was a chemist." 

- "Marie Curie conducted pioneering research on radioactivity."

- **Low Coverage**: The decomposed claims cover only the main points, omitting some details to provide a more generalized view.

**Example:** 

- Original Sentence: 

- "Marie Curie was a Polish and naturalized-French physicist and chemist who conducted pioneering research on radioactivity." 

- Decomposed Claims: 

- "Marie Curie was a physicist." 

- "Marie Curie conducted research on radioactivity."

### **Combining Atomicity and Coverage**

By adjusting both atomicity and coverage, you can customize the level of detail and completeness to meet the needs of your specific use case.

- **High Atomicity & High Coverage**: Produces highly detailed and comprehensive claims that cover all aspects of the original sentence.

**Example:** 

- Original Sentence: 

- "Charles Babbage was an English mathematician, philosopher, inventor, and mechanical engineer." 

- Decomposed Claims: 

- "Charles Babbage was an English mathematician." 

- "Charles Babbage was a philosopher." - "Charles Babbage was an inventor." 

- "Charles Babbage was a mechanical engineer."

- **Low Atomicity & Low Coverage**: Produces fewer claims with less detail, summarizing the main idea without going into specifics.

**Example:** 

- Original Sentence: 

- "Charles Babbage was an English mathematician, philosopher, inventor, and mechanical engineer." 

- Decomposed Claims: 

- "Charles Babbage was an English mathematician." 

- "Charles Babbage was an inventor."

### **Practical Application**

- Use **High Atomicity and High Coverage** when you need a detailed and comprehensive breakdown for in-depth analysis or information extraction.
- Use **Low Atomicity and Low Coverage** when only the key information is necessary, such as for summarization.

This flexibility in controlling the number of claims helps ensure that the information is presented at the right level of granularity for your application's requirements.

# Code implementation

Due to that `ragas` is using Langchain for LLM calling, we must install `ragas` and `langchain` .

- Prefer: Python 3.11 and uv for package install

 

```bash
uv pip install ragas
uv pip install langchain-ollama # I will run local model
```

```python
import asyncio
from ragas.dataset_schema import SingleTurnSample
from ragas.metrics._factual_correctness import FactualCorrectness
from ragas.llms import LangchainLLMWrapper
from langchain_ollama.llms import OllamaLLM

async def main():
    sample = SingleTurnSample(
        response="what is your name?",
        reference="The Eiffel Tower is located in Paris. It has a height of 1000ft."
    )

    model = OllamaLLM(
        model="llava:13b",
        base_url="http://10.6.0.129:11434"
    )

    scorer = FactualCorrectness()
    scorer.llm = LangchainLLMWrapper(model)
    s = await scorer.single_turn_ascore(sample)
    print(s)

# Run the async main function
asyncio.run(main())

```

# Question?

How to break the sentence into claims? — Prompting (few-shot)