# Local Agentic AI System for Business Card Information Extraction and Sales Workflow Integration

**Master's Thesis — Artificial Intelligence for Connected Industries (AI4CI)**
**Conservatoire National des Arts et Métiers (CNAM), Paris**
**Internship at Instituto Tecnológico de Castilla y León (ITCL)**
**Author:** Sheela Davi MALHI
**Defended:** September 28, 2026

---

## Overview

This repository contains the work carried out during my Master's thesis internship at **Instituto Tecnológico de Castilla y León (ITCL)**, in collaboration with **CNAM Paris**.

The project investigated the deployment of a **local agentic AI system** for automating sales-related activities using open-source Large Language Models (LLMs), with **OpenClaw** as the central agent orchestration layer.

The work started from a broader objective: exploring how an AI agent could interact with business tools such as email, calendars, task-management systems and communication channels.

During the internship, I deployed and experimented with OpenClaw on an **NVIDIA Jetson AGX Orin**, integrated it with several external services, tested different local models, and investigated practical issues related to tool use, authentication, session management, structured outputs, latency and local inference.

The scope was later narrowed to a more focused and measurable problem:

> **Business-card information extraction using locally deployed vision-language models.**

This allowed the project to move from general agentic experimentation to a reproducible benchmark involving multiple models, a manually annotated dataset, field-level evaluation, BERTScore, JSON reliability and response-time analysis.

The final system therefore combines two parts:

1. **Agentic system experimentation** — OpenClaw deployment and integration with enterprise-oriented services.
2. **Quantitative model evaluation** — benchmarking vision-enabled LLMs for structured business-card information extraction.

---

## Project Motivation

Sales workflows often involve repetitive administrative activities such as:

* processing contact information;
* updating customer records;
* managing emails;
* scheduling meetings;
* tracking tasks;
* preparing meeting information;
* transferring information from business cards into digital systems.

The goal of this project was not simply to use an LLM as a chatbot, but to investigate how a locally deployed agent could connect a language model to external tools and perform multi-step tasks.

A local deployment was particularly relevant because business workflows can involve personal and commercially sensitive information. Running models locally can reduce the need to send this information to external inference services, although local deployment introduces its own hardware, latency and security constraints.

The project therefore focused on the practical relationship between:

**LLM capability + agentic orchestration + external tools + local hardware + reliability.**

---

## Research Objectives

The main objectives of the thesis were:

* Establish a practical methodology for deploying, configuring and tuning OpenClaw on NVIDIA Jetson hardware.
* Experiment with different open-source LLMs and study their capabilities, limitations and resource requirements.
* Integrate OpenClaw with enterprise-oriented tools and communication services.
* Develop a benchmark for vision-enabled LLMs performing structured business-card information extraction.
* Evaluate extraction accuracy, missing information, invented information, incorrect information, structural JSON reliability and response time.
* Identify practical limitations and propose improvements for local agentic AI deployment.

---

# 1. System at a Glance

The overall architecture consisted of a locally deployed agentic system running on the **NVIDIA Jetson AGX Orin**.

At a high level:

```text
                    User / External Channel
                             |
        +--------------------+--------------------+
        |                    |                    |
      Gmail              Google Chat          WhatsApp
        |                    |                    |
        +--------------------+--------------------+
                             |
                          OpenClaw
                           Gateway
                             |
                 +-----------+-----------+
                 |                       |
             AI Agent                 Tools/APIs
                 |                       |
                 |              +--------+--------+
                 |              |        |        |
                 |            Gmail   Calendar   JIRA
                 |
              Ollama
                 |
       Vision / Language Models
                 |
        Business Card Extraction
                 |
          Structured JSON
                 |
            Evaluation
```
<img width="1140" height="689" alt="openclaw_workflow" src="https://github.com/user-attachments/assets/74079d1e-451b-4548-ae78-e938941f9104" />


The OpenClaw Gateway acted as the central orchestration component. The underlying models were served locally through **Ollama**, allowing different models to be tested without redesigning the complete agent workflow.

The system also included a **FastAPI backend** and a **Streamlit prototype interface** during development.

---

# 2. Hardware Environment

The experiments were performed on an:

### NVIDIA Jetson AGX Orin Developer Kit

The device was used as the main local inference and agentic deployment environment.

Key characteristics relevant to the project:

* NVIDIA Jetson AGX Orin Developer Kit
* 64 GB memory
* Local GPU-accelerated inference
* Local OpenClaw deployment
* Ollama-based model serving
* Resource monitoring using `jtop` and system tools

The hardware was important to the project because model selection was constrained by what could realistically be executed locally.

Larger models could not be evaluated because the main ITCL server was allocated to other projects during the experimental period.

<img width="425" height="171" alt="jetson" src="https://github.com/user-attachments/assets/be3c05ab-4133-4aec-b9ec-68aa05e34805" />


---

# 3. Software Stack

The main technologies used during the internship included:

| Component              | Role                                       |
| ---------------------- | ------------------------------------------ |
| OpenClaw               | Agent orchestration and workflow execution |
| Ollama                 | Local LLM/VLM serving                      |
| NVIDIA Jetson AGX Orin | Local inference hardware                   |
| FastAPI                | Backend/API layer                          |
| Streamlit              | Interface prototyping                      |
| Python                 | Development and evaluation                 |
| JSON                   | Structured model output                    |
| Gmail API              | Email integration                          |
| Google Calendar API    | Scheduling and event management            |
| JIRA                   | Task and issue management                  |
| Google Chat            | Team communication                         |
| WhatsApp               | Messaging and workflow interaction         |
| Webchat                | Local agent interaction and debugging      |

The exact role of each component changed as the project evolved. The final quantitative benchmark focused primarily on the local model inference and evaluation pipeline.

---

# 4. OpenClaw Deployment

OpenClaw was used as the central agentic and orchestration layer.

The system separated the agent orchestration process from the underlying model inference. This made it possible to change the local model while keeping the surrounding workflow architecture.

The local OpenClaw Gateway was configured with:

* local gateway mode;
* loopback binding;
* token-based authentication;
* local communication with the backend and model-serving components.

The gateway handled communication between the agent, models, tools and connected channels.

The project also explored OpenClaw's extensibility through skills, plugins and integrations.

---

# 5. API Development and Backend Work

A significant part of the internship involved learning and experimenting with APIs and communication protocols.

I worked with:

* REST APIs;
* FastAPI;
* HTTP requests;
* WebSockets;
* JSON payloads;
* authentication tokens;
* OAuth 2.0;
* asynchronous workflows;
* API debugging.

FastAPI was used as middleware between the prototype interface and the OpenClaw Gateway.

The backend received user inputs and, when required, image files from the interface. It then communicated with the OpenClaw Gateway through WebSockets.

This also provided practical experience with:

* parsing API responses;
* handling authentication errors;
* debugging permission problems;
* managing sessions;
* handling synchronous and asynchronous communication.

---

# 6. OpenClaw Integrations

Before narrowing the research scope, I experimented with several services relevant to sales and enterprise workflows.

All integrations were tested using controlled or dummy accounts where applicable. They were experiments rather than production deployments.

## 6.1 Gmail

The Gmail integration was used to investigate automated email workflows.

The tested functionality included:

* reading emails;
* searching emails;
* composing emails;
* replying;
* forwarding;
* managing folders;
* downloading attachments;
* generating scheduled email briefings.

A daily briefing workflow was developed to retrieve relevant test emails, summarize them and generate a briefing containing items such as new leads, meeting reminders and action items.

The workflow was tested using manually generated test messages and scheduled execution.

The experiments showed that Gmail could be integrated effectively into the agentic workflow, while credential management and permission control remained important considerations.

---

## 6.2 Google Calendar

Google Calendar was integrated for scheduling and event-management experiments.

The tested functionality included:

* retrieving events;
* summarizing upcoming meetings;
* identifying available time slots;
* creating events;
* modifying events;
* deleting events;
* retrieving event details;
* handling recurring events;
* detecting scheduling conflicts;
* generating notifications and summaries.

The integration worked with the test scenarios, while OAuth configuration and permission management required careful setup.

Read permissions were tested before enabling write operations to reduce the risk of unintended changes.

---

## 6.3 JIRA and Calendar Synchronization

A separate dummy JIRA account and project were used for experimentation.

JIRA tasks were synchronized with Google Calendar through the **Google Calendar Bridge for JIRA**.

The experiment demonstrated bidirectional synchronization between selected JIRA tasks and calendar events.

One important issue was observed during the experiment: the agent could generate additional tasks or events that were not present in the underlying data.

This became a concrete example of the reliability risks associated with giving an agent write access to external systems.

For this reason, validation, confirmation mechanisms, restricted permissions and human approval are important before allowing autonomous modifications to business systems.

---

## 6.4 WhatsApp

WhatsApp was tested as a communication and notification channel.

The experiments included:

* sending notifications;
* receiving messages;
* responding to simple commands;
* retrieving information from connected services;
* triggering predefined workflows.

The integration was restricted to the configured test number.

Access to the broader contact list, message history and group conversations was not available.

This made WhatsApp useful for simple interaction and notifications, but less suitable for workflows requiring access to broader historical communication data.

---

## 6.5 Webchat

OpenClaw's Webchat interface was used during development for:

* real-time interaction;
* prompt testing;
* workflow triggering;
* business-card extraction;
* testing Gmail, Calendar and JIRA commands;
* debugging.

Webchat was particularly useful because changes to prompts and workflows could be tested immediately.

However, persistent conversation history and production-level access control remained limitations.

> **[ADD WEBCHAT SCREENSHOT HERE]**

---

## 6.6 Google Chat

Google Chat was also tested as a communication and workflow-triggering interface.

The experiments included:

* sending notifications;
* processing user queries;
* triggering workflows;
* managing user approval.

The integration required additional OAuth, webhook, network and user-pairing configuration.

It provided an interface suitable for organizations already using Google Workspace.

<img width="695" height="325" alt="Googlechat" src="https://github.com/user-attachments/assets/396e0ef9-e8d9-4aed-854b-2cb472c06fb6" />


---

# 7. Early Model Experiments

Before the final benchmark, a range of models were tested to understand what could realistically run on the Jetson.

Exploratory trials included models such as:

* Llama 2 variants;
* Mistral variants;
* Qwen variants;
* Phi-4 Mini;
* Qwen 3.5;
* Qwen 3.6;
* Gemma 4;
* Ministral 3;
* DeepSeek OCR.

These initial experiments were used to understand:

* computational requirements;
* inference time;
* memory usage;
* model behaviour;
* prompt sensitivity;
* structured output reliability.

Prompt engineering was also performed before the formal benchmark.

Particular attention was given to reducing unsupported information generation and encouraging the models to return `null` when a field was missing or unclear.

---

# 8. Prompt Engineering

The final extraction task used a fixed structured-output format.

The model was instructed to extract:

```text
name
job_title
company
email
phone
address
website
```

The main principle was:

> If information is absent, unclear or only partially visible, return `null` rather than guessing.

The expected output was a JSON object containing only the predefined fields.

This was important because the benchmark was not only evaluating whether a model could read text from an image, but whether it could produce information that was safe to pass to a downstream structured workflow.

---

# 9. From General Agentic Automation to Business Cards

The original project scope was broader than business-card extraction.

The initial architecture was designed around sales-workflow automation and integration with multiple external services.

During the internship, the project was narrowed to business-card information extraction.

This change was made because a focused extraction task provided a more controlled way to investigate:

* model accuracy;
* prompt engineering;
* computational constraints;
* inference latency;
* structured output;
* hallucinated/invented information;
* evaluation methodology.

The broader OpenClaw architecture remained important as the integration and orchestration framework, while the business-card task became the main quantitative experiment.

---

# 10. Business Card Dataset

The final evaluation used **33 unique business-card images**.

Each image was processed **three times per model configuration**, giving:

```text
33 images × 3 runs = 99 responses per model
```

Each response was evaluated across seven contact fields:

```text
33 images × 3 runs × 7 fields
= 693 field-level evaluation instances per model
```

The dataset included several image categories:

* `canvas`
* `non_standard`
* `real_blurred`
* `real_rotated`
* `real_sharp`

These categories were intended to capture differences in layout and image conditions.

<img width="1920" height="1080" alt="business_card_dataset" src="https://github.com/user-attachments/assets/b0eee229-1740-421b-84ce-0015307d4ec7" />


<img width="377" height="167" alt="imagescategories" src="https://github.com/user-attachments/assets/a49439c3-af89-45be-9bb7-9fe77117f3cc" />


---

# 11. Ground Truth

A manually curated ground-truth JSON file was created for the business-card dataset.

Each card was annotated using the same seven fields:

```json
{
  "name": "...",
  "job_title": "...",
  "company": "...",
  "email": "...",
  "phone": "...",
  "address": "...",
  "website": "..."
}
```

When information was not available on the card, the corresponding field was treated as missing.

The ground truth was used as the reference for the automated evaluation.

---

# 12. Models in the Final Benchmark

Five systems were evaluated:

| Model / System                   |         Approx. Size | Configuration               |
| -------------------------------- | -------------------: | --------------------------- |
| Qwen3.6 35B-A3B                  | 35B total parameters | Q4_K_M                      |
| Qwen3.6 27B                      |                  27B | Q4_K_M                      |
| Gemma 4 31B                      |                  31B | Local multimodal model      |
| Ministral 3 14B Instruct 2512    |                  14B | Q8_0                        |
| DeepSeek OCR + rule-based parser |                   3B | OCR + deterministic parsing |

The DeepSeek system was evaluated as a complete OCR-plus-rule-based pipeline rather than as a directly equivalent general-purpose multimodal LLM system.

---

# 13. Experimental Workflow

The final pipeline followed the following process:

```text
Business Card Image
        |
        v
Image Selection
        |
        v
Local Model via Ollama
        |
        v
Structured JSON Prediction
        |
        v
Save Raw Response + Metadata
        |
        v
JSON Validation
        |
        v
Field Normalization
        |
        v
Compare with Ground Truth
        |
        +-------------------+
        |                   |
        v                   v
Field-level Accuracy    BERTScore
        |
        v
Reliability + Latency Analysis
```

Each model was executed independently three times per image.

For each prediction, the experimental records included information such as:

* model configuration;
* input image;
* ground truth;
* model response;
* prediction;
* thinking setting;
* inference time;
* input tokens;
* output tokens.

A SHA-256 hash was also calculated from the raw image bytes to identify images consistently across the experiments.

<img width="1760" height="576" alt="architecture" src="https://github.com/user-attachments/assets/b5447a22-6855-41ae-a009-41256da03642" />


---

# 14. Evaluation Methodology

Two main evaluation approaches were used.

## 14.1 Normalized Field-Level Evaluation

This was the primary evaluation method.

Each extracted field was compared with its corresponding ground-truth value after predefined normalization.

The evaluation distinguished between:

* **Correct**
* **Missing**
* **Invented**
* **Wrong**

This distinction was important because these errors have different meanings.

For example:

* returning `null` when a field is actually present is a missing-information error;
* producing a value that does not exist on the card is an invented-information error;
* returning a real value but assigning the wrong value to a field is a wrong-information error.

---

## 14.2 BERTScore

BERTScore was used as a supplementary semantic-similarity metric.

It was useful for cases where a prediction was semantically close to the reference but not textually identical.

For example, two job titles may express similar meanings while using different wording.

However, BERTScore was **not** treated as a replacement for exact structured evaluation.

This is especially important for:

* email addresses;
* phone numbers;
* websites.

A prediction can be semantically similar to a reference while still being operationally incorrect.

---

# 15. Main Results

The overall normalized field-level results were:

| Model                | Field Accuracy | Missing | Invented | Wrong |
| -------------------- | -------------: | ------: | -------: | ----: |
| Qwen3.6 35B-A3B      |      **88.6%** |    0.3% |     0.8% | 10.3% |
| Qwen3.6 27B          |          87.6% |    0.4% |     0.8% | 11.2% |
| Gemma 4 31B          |          87.3% |    1.1% |     0.7% | 10.9% |
| Ministral 3 14B      |          83.3% |    3.1% |     1.9% | 11.7% |
| DeepSeek OCR + rules |          58.4% |    7.6% |     6.1% | 27.8% |

Qwen3.6 35B-A3B achieved the highest normalized field accuracy at **88.6%**.

However, accuracy alone did not tell the complete story.

Gemma 4 achieved **87.3%** accuracy while also producing a much lower invalid-JSON rate, making it a more balanced option for a structured pipeline.

<img width="3271" height="3597" alt="normalized_metrics_dotplot" src="https://github.com/user-attachments/assets/bbb2a46e-961c-41cc-ae54-a726831bae34" />


---

# 16. BERTScore Results

The mean BERTScore F1 results were:

| Model                | BERTScore F1 |
| -------------------- | -----------: |
| Gemma 4 31B          |   **0.9548** |
| Qwen3.6 27B          |       0.9177 |
| Ministral 3 14B      |       0.9174 |
| Qwen3.6 35B-A3B      |       0.8886 |
| DeepSeek OCR + rules |       0.8284 |

The BERTScore ranking was not identical to the normalized field-accuracy ranking.

This is an important result from the experiment: **strict correctness and semantic similarity measure different properties of the output.**

<img width="2645" height="1472" alt="newbertscore_f1_by_model" src="https://github.com/user-attachments/assets/1feae6be-eab0-4069-b094-de92b6c0f50d" />


---

# 17. Results by Contact Field

The seven evaluated fields were:

```text
Name
Job title
Company
Email
Phone
Address
Website
```

The language models generally performed strongly on names and job titles.

Some of the more difficult fields were phone numbers and addresses.

The overall field-level results were:

| Model                | Name | Job title | Company | Email | Phone | Address | Website |
| -------------------- | ---: | --------: | ------: | ----: | ----: | ------: | ------: |
| Gemma 4 31B          | 1.00 |      0.98 |    0.85 |  0.86 |  0.77 |    0.68 |    0.97 |
| Ministral 3 14B      | 0.97 |      0.93 |    0.74 |  0.87 |  0.75 |    0.74 |    0.84 |
| Qwen3.6 27B          | 0.98 |      0.95 |    0.89 |  0.86 |  0.80 |    0.72 |    0.93 |
| Qwen3.6 35B-A3B      | 0.99 |      0.96 |    0.89 |  0.91 |  0.79 |    0.73 |    0.94 |
| DeepSeek OCR + rules | 0.42 |      0.70 |    0.45 |  0.58 |  0.73 |    0.45 |    0.76 |

The results show that model performance depended on the type of information being extracted.

For example, the DeepSeek OCR and rule-based pipeline remained relatively competitive for phone-number extraction, while showing much larger gaps for names, companies and addresses.

<img width="3121" height="1624" alt="normalized_field_accuracy_heatmap" src="https://github.com/user-attachments/assets/28faceed-cef3-4fd5-839d-cf1e341f7e4e" />


---

# 18. Results by Image Category

The evaluation also compared the models across different image conditions.

### Canvas

The language models performed particularly well on the regular canvas images.

Gemma 4, Ministral 3 and Qwen3.6 35B-A3B each achieved 100% normalized field accuracy in this category.

### Non-standard

Performance decreased when the layouts became less standard.

Gemma 4 achieved approximately 85.2%, while the DeepSeek pipeline reached approximately 47.6%.

### Real blurred

Image blur reduced extraction performance for most systems.

Gemma 4 achieved approximately 87.5%, while DeepSeek reached approximately 42.9%.

### Real rotated

Qwen3.6 27B achieved the highest accuracy in this category at approximately 87.9%.

DeepSeek achieved approximately 41.4%.

### Real sharp

Gemma 4 achieved approximately 75.0%, Qwen3.6 27B approximately 77.3%, and Qwen3.6 35B-A3B approximately 76.4%.

DeepSeek achieved approximately 55.1%.

<img width="2964" height="1624" alt="normalized_accuracy_by_category" src="https://github.com/user-attachments/assets/f5b3c78f-b476-4274-ab4d-e10545731b81" />


---

# 19. Structural Reliability

Extraction accuracy was not the only reliability measure.

The experiments also measured whether the model returned valid JSON.

Invalid JSON rates included:

| Model                | Invalid JSON |
| -------------------- | -----------: |
| DeepSeek OCR + rules |         0.0% |
| Gemma 4 31B          |         2.0% |
| Ministral 3 14B      |         3.0% |
| Qwen models          | Higher rates |
| Qwen3.6 35B-A3B      |    **13.8%** |

Qwen3.6 35B-A3B achieved the highest field accuracy, but it also had the highest invalid-JSON rate among the evaluated language models.

This demonstrates why a model cannot be selected based on extraction accuracy alone when its output is intended for an automated structured workflow.

<img width="3271" height="1323" alt="invalid_json_responses_by_model" src="https://github.com/user-attachments/assets/a55c780d-2032-477f-a2d5-853919ac1675" />


---

# 20. Response Time

Average end-to-end response times were:

| Model / System       | Average Response Time |
| -------------------- | --------------------: |
| DeepSeek OCR + rules |            **~5.6 s** |
| Ministral 3          |           **~13.2 s** |
| Qwen3.6 35B-A3B      |           **~42.2 s** |
| Qwen3.6 27B          |          **~185.5 s** |
| Gemma 4 31B          |          **~190.2 s** |

This produced a clear accuracy–efficiency trade-off.

The DeepSeek pipeline was considerably faster but substantially less accurate.

Qwen3.6 35B-A3B produced the highest normalized field accuracy but required significantly more processing time.

Ministral 3 provided another balance between response time and extraction accuracy.

<img width="3275" height="1323" alt="average_response_time_by_model" src="https://github.com/user-attachments/assets/cdb3c1e8-b3d2-4fc4-b478-cc8e6ac1ec2c" />


---

# 21. Main Findings

Several findings stood out from the experiments.

### 1. Local multimodal models can perform structured extraction

The evaluated vision-language models achieved between approximately **83% and 89% field accuracy**, substantially above the DeepSeek OCR and rule-based baseline at 58.4%.

### 2. The best accuracy is not automatically the best deployment choice

Qwen3.6 35B-A3B achieved the highest field accuracy, but its invalid-JSON rate was also high.

Gemma 4 achieved slightly lower accuracy while providing a more balanced combination of extraction quality and structural reliability.

### 3. Image conditions matter

Model performance changed considerably across regular, non-standard, blurred and rotated business cards.

### 4. Different fields have different difficulty levels

Names and email addresses were generally easier to extract than addresses and phone numbers.

### 5. Semantic similarity and exact correctness are different

The BERTScore results did not exactly follow the normalized field-accuracy ranking.

### 6. Local deployment introduces an accuracy–latency–resource trade-off

A model must be evaluated not only according to its output quality, but also according to whether it can run within the available hardware and latency requirements.

### 7. Agentic automation requires safeguards

The integration experiments demonstrated that giving an agent write access to external services introduces additional risks.

The observed JIRA–Calendar experiment provided a concrete example of why validation and human approval can be important before allowing autonomous changes to business data.

---

# 22. Security and Privacy Considerations

Because the project involved services containing potentially sensitive business information, experiments were conducted using controlled or dummy accounts where applicable.

The local deployment was also motivated by the possibility of keeping model inference within controlled infrastructure.

However, local deployment does not automatically make a system secure.

The thesis identifies several requirements for production deployment:

* access control;
* authentication;
* restricted permissions;
* validation;
* confirmation mechanisms;
* human approval for externally visible or destructive actions;
* secure endpoint configuration;
* cybersecurity assessment;
* monitoring and logging.

The experiments should therefore be understood as a research and development environment rather than a production-ready enterprise deployment.

---

# 23. Limitations

The main limitations identified during the project were:

### Hardware constraints

Access to more powerful GPU resources was limited, so larger models could not be fully evaluated.

### Dataset size

The benchmark used 33 unique business-card images.

Although each image was processed three times, repeated runs are not fully independent observations.

### Production deployment

The integrated services were tested in controlled environments. A complete production deployment was outside the scope of the thesis.

### Security assessment

A full cybersecurity audit, penetration testing and compliance assessment were not performed.

### Webchat persistence

The default OpenClaw Webchat interface did not provide the persistent conversation history required for a more complete multi-user application.

### Reasoning effort

Different reasoning levels were tested, but the relationship between reasoning effort, accuracy and response time was not exhaustively investigated across all possible configurations and document types.

---

# 24. Improvement Proposals

Based on the experimental results, several improvements were proposed.

## Structured Output Validation

Add a validation layer between the model and downstream systems.

```text
LLM
 |
 v
JSON Schema Validation
 |
 +---- Valid ------> Continue
 |
 +---- Invalid ----> Retry / Repair
```

This would reduce the impact of malformed model responses.

---

## Field-Specific Post-Processing

Address and phone extraction remained more difficult than several other fields.

A hybrid pipeline could therefore use dedicated validation and normalization after LLM extraction.

For example:

```text
Business Card
      |
      v
Vision-Language Model
      |
      v
Structured JSON
      |
      +---- Phone ---> Phone validation
      |
      +---- Address -> Address parsing
      |
      +---- Other fields
      |
      v
Validated record
```

---

## Persistent Sessions

MongoDB was proposed as a possible storage layer for persistent conversation history in the Streamlit interface.

This would allow sessions to continue across interactions instead of being reset.

---

## Reasoning-Effort Allocation

A possible strategy is to allocate reasoning effort according to field difficulty.

For example:

```text
High-confidence fields
        |
        +--> Lower reasoning effort
             ↓
          Lower latency

Difficult fields
        |
        +--> Higher reasoning effort
             ↓
          More computation
```

The proposal is based on the observed difference between easier fields such as names and more difficult fields such as addresses and phone numbers.

---

# 25. Future Work

The thesis identifies several directions for future work:

* Evaluate larger and more advanced multimodal models when sufficient GPU resources are available.
* Expand the business-card dataset with more diverse real-world samples.
* Investigate model fine-tuning.
* Study context-window size and its relationship with response time.
* Systematically evaluate different reasoning/thinking levels.
* Investigate additional evaluation methods, including LLM-as-a-Judge approaches.
* Improve persistent session management.
* Conduct a full cybersecurity assessment before production deployment.
* Perform penetration testing, endpoint protection and compliance checks.

---

# 26. Research Outputs

The project resulted in:

* a local OpenClaw deployment on NVIDIA Jetson AGX Orin;
* integrations with Gmail, Google Calendar, JIRA, WhatsApp, Google Chat and Webchat;
* exploratory model and prompt experiments;
* a structured business-card extraction pipeline;
* a manually annotated evaluation dataset;
* a reproducible evaluation workflow;
* field-level accuracy analysis;
* BERTScore evaluation;
* JSON structural reliability analysis;
* response-time measurements;
* analysis of local deployment constraints;
* improvement proposals for more reliable agentic automation.

---

# 27. Thesis

The complete Master's thesis documents the methodology, implementation, experiments, results and analysis in detail.

**Title:**
*Local agentic AI system for automating business feature extraction and sales workflow integration*

**Institution:** Conservatoire National des Arts et Métiers (CNAM)

**Host organization:** Instituto Tecnológico de Castilla y León (ITCL)

**Field:** Computer Science

**Specialization:** Artificial Intelligence for Connected Industries

**Supervisor:** Prof. Stefano Secci, CNAM

**Co-supervisor:** Marteyn van Gasteren, ITCL


---

# 31. Acknowledgements

I would like to thank Professor Stefano Secci for sharing the internship opportunity at ITCL and for his support throughout my Master's studies.

I am grateful to Marteyn van Gasteren for his supervision and guidance throughout the internship, and to Rubén Francisco Burgos and the AI Agents team at ITCL for their support and the computational resources provided for this work.

I also thank Jorge Vara Rodriguez, Edison Jair Bejarano Sepulveda, Kevin Monsálvez and Adrián Encinas Lozano for their technical advice, feedback and assistance during the project.

---

## Citation

If you use or reference this work, please cite the thesis:

```text
Malhi, Sheela Davi.
"Local agentic AI system for automating business feature extraction
and sales workflow integration."
Master's Thesis, Conservatoire National des Arts et Métiers (CNAM), 2026.
```

---

## Author

**Sheela Davi MALHI**

Master's Degree in Artificial Intelligence for Connected Industries
Conservatoire National des Arts et Métiers (CNAM), Paris

Research interests include:

* Agentic AI
* Large Language Models
* Multimodal AI
* Local and Edge AI
* AI Agents and Tool Use
* AI Evaluation
* Reliable AI Systems

---

## Note on the Experimental Scope

The integrations described in this repository were developed and tested as part of the research and development process. They should not be interpreted as a production-ready enterprise system.

The quantitative benchmark specifically evaluates locally deployed vision-enabled models for business-card information extraction. Production deployment would require additional security, validation, access-control and human-oversight mechanisms.
