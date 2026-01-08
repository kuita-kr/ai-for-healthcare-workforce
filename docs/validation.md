# Readability Validation

## Objective

Validate that AI-generated outputs match the reading comprehension level of target user personas (Sandra, 13; Kim, 26). 

**Note:** Kim's output was generated in Korean; validation was performed on the English translation.

## Methodology


Two standard readability metrics were applied to assess output appropriateness. This validaition checks were conducted seperately and were not coupled to the design of the chatbot. However, addtional metrics could easily be added if needed to the workflow (e.g sentiment analysis, subjectivity)

### Flesch Reading Ease (FRE)

Measures readability based on sentence length (average words per sentence) and word length (average syllables per word).

**Formula:**
```
FRE = 206.835 - 1.015(words/sentences) - 84.6(syllables/words)
```

**Scale:** -∞ to 121.22 (higher = easier). Negative scores are valid.

| Score Range | US School Level | UK Equivalent | Notes |
|-------------|-----------------|---------------|-------|
| 100-90 | Grade 5 | Year 6 | Very easy to read. Easily understood by an average 11-year-old student |
| 90-80 | Grade 6 | Year 7 | Easy to read. Conversational English for consumers |
| 80-70 | Grade 7 | Year 8 | Fairly easy to read |
| 70-60 | Grades 8-9 | Years 9-10 | Plain English. Easily understood by 13- to 15-year-old students |
| 60-50 | Grades 10-12 | Years 11-13 | Fairly difficult to read |
| 50-30 | University | Undergraduate | Difficult to read |
| 30-10 | Graduate | Postgraduate | Very difficult to read. Best understood by university graduates |
| 10-0 | Professional | Postgraduate+ | Extremely difficult to read. Best understood by university graduates |

**Strengths:** 
- Industry standard (implemented in Grammarly, MS Word)
- Objective numerical comparison across texts

**Limitations:** 
- Designed for English; cultural and linguistic variations affect accuracy
- Ignores content complexity, semantic difficulty, and domain-specific vocabulary
- Does not consider text cohesion, logical flow, or reader prior knowledge

**Source:** https://readable.com/readability/flesch-reading-ease-flesch-kincaid-grade-level/  
**Reference:** https://en.wikipedia.org/wiki/Flesch%E2%80%93Kincaid_readability_tests

### Gunning Fog Index (GFI)

Estimates years of formal education required to understand text on first reading, based on sentence length and complex word proportion.

**Formula:**
```
GFI = 0.4 × [(words/sentences) + 100 × (complex_words/words)]
```

**Complex word definition:** Words with ≥3 syllables, excluding:
- Proper nouns
- Familiar compound words (e.g., "butterfly")
- Verbs made complex by suffixes (e.g., "created")

**Scale:** Years of education required

| Score | US Educational Level | UK Educational Level | Examples of Materials |
|-------|---------------------|----------------------|----------------------|
| 6-8 | Elementary School | Primary School (KS2) | Children's books, simple informational texts |
| 9-12 | Middle School | Lower Secondary (KS3) | Textbooks, news articles, general fiction |
| 13-16 | High School | Upper Secondary (KS4-5) | Academic texts, technical writing |
| 17+ | University | Higher Education | Advanced academic papers, complex technical documents |

**Strengths:** 
- Maps directly to education levels
- Accounts for lexical and syntactic complexity
- Widely used in technical writing and journalism

**Limitations:** 
- Arbitrary 3-syllable threshold; may not reflect true complexity
- Assumes uniform education standards across regions
- Penalises technical terminology necessary in specialist fields
- May underestimate difficulty for texts with short sentences but complex concepts

**Source:** https://textinspector.com/gunning-fog-index-guiding-students-towards-clarity/

## Implementation

**Library:** `textstat==0.7.12`

**Installation:** https://pypi.org/project/textstat/
```python
import textstat

test_data = (
    "Health informatics is one of the fastest growing areas within healthcare. "
    "To put it simply — health informatics is about getting the right information "
    "to the right person at the right time. You could be introducing electronic "
    "health records for every person in the country or exploring patient data to "
    "identify trends in disease and treatment."
)

# Calculate Flesch Reading Ease
flesch_score = textstat.flesch_reading_ease(test_data)

# Calculate Gunning Fog Index
fog_index = textstat.gunning_fog(test_data)
```

**Example source:** https://www.healthcareers.nhs.uk/working-health/working-nhs/who-works-nhs

**Alternative library:** `py-readability-metrics` (https://pypi.org/project/py-readability-metrics/)

## Results

| Persona | FRE | GFI | Target Alignment |
|---------|-----|-----|------------------|
| Sandra (13, Year 8) | 73.98 | 8.87 | ✓ Grade 7-8 |
| Kim (26, Undergraduate) | 34.93 | 9.97 | ✓ University |

## Validation

**Score differentiation between personas:**
- **FRE difference:** 39.05 points (Sandra: 73.98, Kim: 34.93) - demonstrates clear readability gap
- **GFI difference:** 1.10 points (Sandra: 8.87, Kim: 9.97) - indicates appropriate educational level targeting

Both metrics confirm successful adaptation to the personas 

## Limitations

1. Metrics designed for English
2. Domain-specific terminology in healthcare may inflate complexity scores
3. Readability scores proxy for comprehension but don't measure actual understanding
4. No validation against user feedback or task completion rates