# LLM Prompt Engineering
 
## Making Effective Prompts
- Use action verbs like **write**, **create** instead of ambiguous verbs like think or feel.
- Output limit on number of **sentences**, **words**, **characters** although this doesn't guarantee. **Max token** configuration in API can be used too.
- Specify **output structure or tone**: table, dot points, formal and persuasive tone, etc.
- Ask LLM to **focus on any specific aspect/point**.
- Provide content in context by using **delimiters**. <br>
  - E.g., Summarize the review delimited by triple backticks, in three sentences, focusing on the key features and user experience: \```{text}\```  
 
## Prompt Use Cases
- **Text summarization**: Ask model to summarise delimited text. Specify output size and structure, and specify aspect to focus on.
- **Text expansion**: Ask model to expand delimited text. Highlight aspect to focus on. Provide output requirements (tone, length, structure, audience).
- **Text transformation**: Includes language translation, tone adjustment, writing improvement.
- **Text analysis**:
  - Text classification (e.g., sentiment analysis)
  - Entity extraction (extracting specific entity like name, place, organisation from text)
  - Specifying the desired outputs and few-shot prompting help LLM to understand the task better.
- **Code generation/explanation**
 
## Behind Prompting: API Roles
- **System**: Provides purpose, output guidance, safety guardrail, behaviour guidance.
- **User**: User input content.
- **Assistant**: Response from LLM (can be seeded via API for shot prompting).
 
## Prompt Techniques
### Shot Prompting
- **Zero-shot**, **One-shot** & **Few-shot prompting**.
- Example can be given directly to the LLM in the user content or it can be given using user and assistant roles in API.
 
### Multi-step Prompting
- Provides steps of tasks for LLM to go through in order to make LLM work in the desired sequence.
 
### Chain-of-Thought Prompting
- Making LLM provide reasoning steps for the answer.
- Used for complex reasoning tasks.
- Can be done by asking LLM to think step by step.
- Can be done via few-shots prompting by giving examples of reasoning response.
- **Difference with multi-step prompting**:
  - Multi-step prompting = steps explicitly given by the user.  
  - Chain-of-thought = LLM creates its own reasoning steps.
- **Limitation**: If LLM starts with the wrong reasoning step in the beginning, the final result will likely be wrong too.
 
### Self-Consistency
- Generates multiple chain-of-thoughts by prompting multiple LLM responses and making LLM to choose the best output.
- Helps to choose the best reasoning response.
- Can be done by:
  - Using multiple prompts
  - Making LLM give out multiple response options and choose the best one in a single prompt-response.
 
### Iterative Prompt Engineering
- Reiterate prompts by analysing the output and refining prompts to make a better prompt to achieve desired response.
- This can be achieved by:
1. Craft a prompt
2. Send prompt to LLM
3. Observe and analyse output
4. Identify potential issue
5. Refine prompt
6. Repeat until desired response is reached
