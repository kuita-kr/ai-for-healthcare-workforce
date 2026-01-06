# Meta-Prompting

## Definition

Meta-prompting is instructing the AI on *how* to respond, not just *what* to respond with. It defines behavioural rules and constraints for the AI's reasoning and output.

## Prompting Framework: RICEIFE

We used the **RICEIFE framework** for structured prompt design:

| Component | Description |
|-----------|-------------|
| **R**ole Instruction | Define the AI's expertise and perspective |
| **I**nput Data | Provide specific data and parameters |
| **C**ontext | Explain the situation and constraints |
| **E**vidence of best practice | Include frameworks and proven methodologies (optional) |
| **I**teration/Adjustment | Handle updates, changes, and edge cases |
| **F**ormat | Specify output structure |
| **E**xpectation | Define success criteria |

### Example Application: Reading Tracker

| Prompt Component | Reading Books Example |
|------------------|----------------------|
| **Role Instruction** | Act as a productivity coach helping me track my progress towards a personal goal. Create a personal project for me to read 10 books by the end of the year on 31st December. |
| **Context** | I would like to read 5 non-fiction and 5 fiction. The only time that I can read is 20 minutes before going to sleep, it is 29th September today. |
| **Evidence of best practice** | The habit will need to seem easy and be stacked on top of another existing habit (based on *Atomic Habits*). |
| **Input Data** | The fiction books that I would like to read are on average 250 pages and the non-fiction books are on average 200 pages. |
| **Format** | Create the project plan in the form of a reading log which I can track in Excel. The columns should have the following categories: Date, Book Name, Amount of Pages in Book, Pages Read, Pages Left, On Track to Meet Goal, What I've Learnt. |
| **Adjustment** | I am falling behind on my plan and have only read 5 pages since 29th September. It is now 10th October. |

*Source: Example and framework from Multiverse training*

---

## Our Meta-Prompting Implementation

Our full system prompts were not captured after the hackathon. However, we applied two key meta-prompting strategies within the RICEAF framework:

### 1. Age-Adaptive Language
We instructed the system to automatically adjust language complexity based on the user's age (provided as UI input). The AI was prompted to:
- Target appropriate reading levels (Flesch Reading Ease and Gunning Fog Index) for each age group
- Adjust vocabulary, sentence length, and technical terminology accordingly
- Adapt tone and examples to match the user's life stage

### 2. Source-Only Constraint
We instructed the AI to only use provided sources and provide a link to the reference dataset it is getting its information from. We asked requested the AI said **"I don't know"** or **"This information is not available in the provided sources"** when information was absent. This aims to limit hallucination by ensuring all responses are traceable to source documents.
