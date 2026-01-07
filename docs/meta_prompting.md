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
# System Prompt Implementation

Our full system prompts were not captured after the hackathon. However, we applied several key meta-prompting strategies within the RICEIFE framework:

## Prompt Flow Architecture

![Prompt flow screenshots showing different prompting routes depending on age](../images/promptflow.png)

We used Azure Machine Learning prompt flow to design a system with a multi-layered prompting approach comprising the following components:

1. **Persona and Language Adaptation**
2. **Query Context (vectorised retrieval and entity recognition)**
3. **Content Safety Verification**
4. **Customised System Prompt**

---

## 1. Age-Adaptive Language

The system automatically adjusted language complexity based on user age (child or adult), provided as UI input.

**Adaptive parameters:**
- Target reading levels appropriate for each age group
- Vocabulary, sentence length, and technical terminology
- Tone and examples matched to user's life stage

---

## 2. Input Language Detection

The prompt adapted to match the user's input language, ensuring responses were provided in the same language as the query.

---

## 3. Azure Content Safety

Content safety verification was implemented to filter inappropriate content, particularly important for child users. Whilst NHS career sources are generally safe, these safeguards provide an essential protective layer.

### Content Safety Parameters

| Parameter | Type | Description | Sensitivity Options | Default |
|-----------|------|-------------|---------------------|---------|
| `text` | string | Text to be moderated | N/A | Required |
| `hate_category` | string | Moderation for hate speech | `disable`, `low_sensitivity`, `medium_sensitivity`, `high_sensitivity` | `medium_sensitivity` |
| `sexual_category` | string | Moderation for sexual content | `disable`, `low_sensitivity`, `medium_sensitivity`, `high_sensitivity` | `medium_sensitivity` |
| `self_harm_category` | string | Moderation for self-harm content | `disable`, `low_sensitivity`, `medium_sensitivity`, `high_sensitivity` | `medium_sensitivity` |
| `violence_category` | string | Moderation for violence | `disable`, `low_sensitivity`, `medium_sensitivity`, `high_sensitivity` | `medium_sensitivity` |

**Sensitivity levels:**
- `disable` - No moderation for this category
- `low_sensitivity` - Minimal filtering
- `medium_sensitivity` - Balanced filtering (default)
- `high_sensitivity` - Strictest filtering

### Content Safety Output

The content safety tool returns a binary decision for each category:
```json
{
  "action_by_category": {
    "Hate": "Accept",
    "SelfHarm": "Accept",
    "Sexual": "Accept",
    "Violence": "Accept"
  },
  "suggested_action": "Accept"
}
```

- `action_by_category` - Binary value (`Accept` or `Reject`) for each category based on configured sensitivity levels
- `suggested_action` - Overall recommendation; returns `Reject` if any category is rejected

Source: https://microsoft.github.io/promptflow/reference/tools-reference/contentsafety_text_tool.html
---

## 4. Source-Only Constraints

The AI was instructed to only use provided sources and include references to source documentation.

**Boundaries:**
- When information was unavailable in sources, the system responded with: **"I don't know"** or **"This information is not available in the provided sources"**
- All responses included links to reference datasets
- This approach limits hallucination by ensuring response traceability

---

## Combined Prompt Pipeline

The following layers worked together to produce safe, appropriate, and accurate responses tailored to each user's profile and query.

The final output was generated by combining all components:

1. **Base system prompt** (role, task definition, behaviour rules)
2. **Age-specific adaptation** (child/adult language adjustments)
3. **Language context** (user's input language)
4. **Content safety verification** (filtering inappropriate content)
5. **Source constraints** (grounding responses in verified documentation)

You can learn more about prompt flow here: https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/overview-what-is-prompt-flow?view=azureml-api-2)