# 🎓 Intelligent Student Support Agent

An LLM-based student support assistant built with Groq, Python, SQLite, retrieval-augmented generation (RAG), and Gradio. The project demonstrates agentic tool calling, student record access control, persistent memory, sensitive-action approval workflows, audit logging, and evaluation.

> **Disclaimer:** This is an educational prototype using synthetic student IDs and a public demonstration dataset. It is not an official university student information system.

## Features

- **LLM integration:** Uses Groq with the `openai/gpt-oss-20b` model.
- **Student status lookup:** Retrieves tuition fee and scholarship status from a SQLite database.
- **RAG-based policy questions:** Retrieves relevant sections from a student support policy document using TF-IDF and cosine similarity.
- **Support requests:** Creates support requests with unique IDs and an initial status of `Open`.
- **Persistent memory:** Stores non-sensitive student preferences in SQLite.
- **Access control:** Restricts student record lookup to the authenticated identity configured in the prototype.
- **Sensitive-action workflow:** Records requests for sensitive changes and supports simulated reviewer approval or rejection.
- **Audit logging:** Records action attempts, permission decisions, approval status, and outcomes.
- **Security checks:** Tests access denial for another student and rejection of OTP storage.
- **Interactive interface:** Provides a chatbot built with Gradio.
- **Evaluation:** Records tool selection, latency, token usage, and test outcomes.

## Technology Stack

- Python
- Groq API
- `openai/gpt-oss-20b`
- SQLite
- Pandas
- scikit-learn
- TF-IDF and cosine similarity
- Gradio
- Google Colab

## Architecture

1. A student enters a question through the Gradio chatbot.
2. The LLM determines whether a supported tool is needed.
3. The application executes the selected Python tool.
4. The tool retrieves student data, searches policy evidence, or creates a support request.
5. The result is returned to the LLM to generate a response.
6. The application records execution traces, latency, and token usage where available.
7. Security-related operations use application-level checks and record audit events.

## Available Tools

| Tool | Purpose |
|---|---|
| `check_student_status` | Retrieves the current demo student's fee and scholarship status. |
| `search_student_policy` | Searches the policy document for relevant evidence. |
| `create_support_request` | Creates a support request and returns its ID and status. |

## Security and Privacy

- The student lookup function checks the requested student ID against the configured current student ID.
- The application rejects requests to access another student's record.
- The memory function rejects values containing configured credential-related keywords.
- Sensitive-action requests are recorded as pending until a reviewer decision is recorded.
- Rejected sensitive actions are not executed.
- OTPs and passwords must not be included in test data or committed files.
- API credentials should be stored in environment variables or Colab Secrets, never in source code.

### Prototype Limitations

- Student identity is configured using a fixed demo ID rather than production authentication.
- Reviewer identity is simulated; it is not verified through a real authentication system.
- Approved sensitive actions are not connected to actual record-change execution.
- Credential detection uses keyword checks and is not a complete secret-detection mechanism.
- SQLite memory is not encrypted.
- Policy retrieval uses TF-IDF rather than a vector database or embedding model.
- The demonstration dataset is not an official source of student records.
- Online support-request tracking and status updates are not implemented.
- Evaluation results cover a small number of test cases and do not establish production reliability.

## Evaluation Results

The three core agent tests produced the following results in one evaluation run:

| Test | Expected Tool | Result | Latency |
|---|---|---|---:|
| Fee and scholarship lookup | `check_student_status` | Passed | 2.641 seconds |
| Policy question using RAG | `search_student_policy` | Passed | 1.274 seconds |
| Create support request | `create_support_request` | Passed | 0.961 seconds |

**Tool-selection pass rate:** 100% (3/3 test cases)

**Total tokens reported:** 3,355 across the three tests.

Separate security tests passed for own-record access, denial of another student's record, and rejection of OTP storage.

These results describe the recorded prototype tests. They do not establish overall answer accuracy or production-level security.

## Setup

### 1. Install dependencies

```bash
pip install groq gradio pandas scikit-learn
```

### 2. Configure the API key

Set the `GROQ_API_KEY` environment variable, or store the key securely in Google Colab Secrets.

Never commit API keys to GitHub.

### 3. Run the application

Open the project notebook in Google Colab and execute its cells in order to initialize the database, define tools, configure the Groq client, load the policy document, initialize the agent, and launch the Gradio interface.

The notebook must contain the implementation code; this README alone is not the complete application.

## Project Structure

The intended repository structure is:

```text
intelligent-student-support-agent/
├── README.md
├── student_support_agent.ipynb
├── student_support_policy.txt
└── requirements.txt
```

The notebook filename and supporting files should match the files actually uploaded to the repository.

## Future Improvements

- Add real authentication and role-based access control.
- Implement approved sensitive-action execution with transaction safeguards.
- Improve credential detection and protect stored data.
- Add broader functional, authorization, and answer-quality evaluations.
- Add support-request lookup and status management if required.
- Improve retrieval quality and measure evidence relevance.
- Add structured logging and error-handling tests.

## Educational Purpose

This project demonstrates concepts in agentic AI, LLM tool calling, retrieval-augmented generation, persistent memory, application-level authorization, human approval workflows, audit logging, and evaluation.
