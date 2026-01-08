# NHS Career Coach

*Hack for Health, November 2024 - Team 4*

## Overview

An multilingual AI career guidance chatbot designed to help students and graduates navigate the complex landscape of NHS careers. Developed during the Microsoft, Kainos for the NHS Website team, this tool addresses the critical gap in accessible, personalised career guidance for aspiring healthcare professionals.  

## The Challenge

**NHS Risks Losing Future Talent Pipeline**

- Each year, thousands of students and graduates aspire to have NHS and healthcare careers but lack clear, accessible guidance
- Current career resources are overwhelming, with over 350 different NHS career pathways to navigate
- Critical decision points (GCSE selection, university choices, career transitions) lack targeted intervention
- Potential talent is lost due to information overload and unclear entry routes

**Our Solution:**

An multilingual AI career coach leveraging Health Education England (HEE) website pages to guide users through NHS entry points with personalsed, age-appropriate advice tailored to their educational stage and career interests.

## Hackathon Achievement

**🥈 We won Second Place** - [Hack for Health Microsoft AI Hackathon](https://nhsengland.github.io/datascience/articles/2024/12/04/microsoft-hackathon/)

The NHS England Data Science Team, alongside analysts from across the organisation, participated in this AI hackathon at Microsoft. The event was organised collaboratively by the NHS England Data Science Team, Microsoft, and Kainos, with NHS Website Services Team as key stakeholders.

We were awarded second place our solution as judgemed by these stakeholders.

## Technical Implementation
This is the front-end code base for Team 4 in the Hack for Health event with Microsoft and Kainos. The back-end is having limitations on the original subscription which is no longer available to serve with.

We produced an NHS Career Coach chat bot, using Azure AI Foundry. The code in this repo is a simple Streamlit app based on this code: [build a basic LLM chat app](https://docs.streamlit.io/develop/tutorials/llms/build-conversational-apps).

This connects to an API Endpoint for the LLM that was set up in Azure AI Foundry.

This repository also includes our prompting and meta-prompting strategy, validation process, and user personas.

### Demo

To see the app in action, please take a look at the [demo video](videos/nhs-career-coach.mp4).


### Contact

> **This material is maintained by the [NHS England Data Science team](mailto:datascience@nhs.net)**.

For more information please contact us at the above email address FAO:

Warren Davies, Data Scientist - Data Science team, Advanced Analytics (D&A)

Chaeyoon Kim, Data Scientist - Workforce, Training and Education analytics, Advanced Analytics (D&A)

Mary Amanuel, Data Scientist - Strategic Analysis, Advanced Analytics (D&A)

### License

Unless stated otherwise, the codebase is released under [the MIT Licence][mit].
This covers both the codebase and any sample code in the documentation.

_See [LICENSE](./LICENSE) for more information._

The documentation is [© Crown copyright][copyright] and available under the terms
of the [Open Government 3.0][ogl] licence.

[mit]: LICENCE
[copyright]: http://www.nationalarchives.gov.uk/information-management/re-using-public-sector-information/uk-government-licensing-framework/crown-copyright/
[ogl]: http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
