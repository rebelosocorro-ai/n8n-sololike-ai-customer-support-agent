# n8n-sololike-ai-customer-support-agent
An AI-powered customer support automation built with n8n, OpenAI, Pinecone, and Gmail. Retrieves policy information from Pinecone's Documents API and uses an AI Agent to generate customer support email drafts.


# SOLOLIKE Customer Support AI Agent

An AI-powered customer support automation built with **n8n, OpenAI, Pinecone, and Gmail**.

This project demonstrates how workflow automation, retrieval-augmented generation (RAG), and AI agents can be combined to retrieve customer policies, understand customer enquiries, and prepare consistent, policy-based email replies.

## Project Overview

SOLOLIKE is a fictional direct-to-consumer (D2C) T-shirt brand created for this demonstration.

The objective is to reduce repetitive customer support work by retrieving relevant policy information and using an AI Agent to draft helpful, professional email responses.

The workflow retrieves the SOLOLIKE policy from a Pinecone document-schema index through the Pinecone Documents API. The retrieved policy is passed to the AI Agent, which generates a customer support response and uses a Gmail tool to create a draft.

## Workflow

The main workflow follows this sequence:

1. **Gmail Trigger** — Detects incoming customer emails.
2. **Message a Model** — Processes the incoming message for the workflow's routing and classification logic.
3. **IF Node** — Determines whether the email should continue through the customer support workflow.
4. **HTTP Request** — Searches the existing Pinecone document-schema index using the Pinecone Documents API and retrieves the stored policy text.
5. **AI Agent** — Uses the customer email, retrieved policy, and system instructions to prepare a relevant response.
6. **Gmail Draft Tool** — Creates a draft response for review.

The AI Agent also has an OpenAI Chat Model connection.

> The standard n8n Pinecone Vector Store node was incompatible with the document-schema index used in this project. The working retrieval path uses an HTTP Request node and the Pinecone Documents API instead.

## Technologies Used

- **n8n** — Workflow automation and orchestration.
- **OpenAI** — Language model and embedding integration.
- **Pinecone** — Storage and retrieval of customer support policy information.
- **Pinecone Documents API** — Document search and retrieval for a document-schema index.
- **Gmail** — Incoming email monitoring and draft creation.
- **Google Colab** — Initial Pinecone connectivity, index inspection, and retrieval testing.

## Knowledge Base

The Pinecone index stores a fictional SOLOLIKE customer policy document containing information about:

- Shipping and delivery
- Returns and exchanges
- Refund processing
- Order cancellation
- Payment issues
- Damaged or incorrect products
- Product sizing and care
- Customer support contact procedures
- Policy limitations

The policy is used as the reference source for generating responses.

## Key Implementation Details

### Pinecone Documents API

The index used in this project has a document schema and a `chunk_text` field.

The conventional Pinecone vector/record read endpoints do not support this index's document schema. The workflow therefore uses the Documents API through an n8n HTTP Request node.

The search request uses:

- HTTP method: `POST`
- Endpoint pattern: `/namespaces/{namespace}/documents/search`
- API version header: `X-Pinecone-API-Version`
- Search field: `chunk_text`

The HTTP Request node was tested successfully and returned the stored SOLOLIKE policy document.

### AI Agent Instructions

The AI Agent is instructed to:

- Use the retrieved policy as its source of truth.
- Respond politely and professionally.
- Avoid inventing policies or exceptions.
- Explain relevant return, damaged-item, and refund conditions accurately.
- Request missing information where necessary.
- Avoid claiming that a refund, return, or replacement has been approved without confirmation.
- Produce complete email drafts with appropriate closings.

### Human Review

The workflow creates Gmail drafts rather than automatically sending replies.

This allows a support representative to review the generated message before sending it to a customer.

## Example Use Cases

- A customer asks about the return window.
- A customer wants to understand the refund timeline.
- A customer reports a damaged or incorrect T-shirt.
- A customer asks whether an order can be cancelled.
- A customer needs guidance about delivery or tracking.

## Current Project Status

The following components have been tested:

- Gmail Trigger receives customer emails.
- Pinecone Documents API retrieves the stored policy through the n8n HTTP Request node.
- The AI Agent generates a response using the retrieved policy.
- The Gmail draft tool reports successful draft creation.

The standard n8n Pinecone Vector Store and its dependent question-answering tool remain incompatible with the document-schema index. The working workflow uses the HTTP Request retrieval path instead.

Further improvements could include more robust handling of missing search results, additional customer-email test cases, and automated checks for incomplete drafts.

## Setup Overview

To reproduce the project, you will need:

1. An n8n instance.
2. An OpenAI API credential.
3. A Pinecone account and a document-schema index containing the policy document.
4. A Gmail credential connected to n8n.
5. An HTTP Request node configured to use the Pinecone Documents API.
6. An AI Agent configured to receive the customer email and retrieved policy.
7. A Gmail draft tool connected to the AI Agent.

Store credentials securely using n8n's credential management. Do not hard-code API keys in workflow expressions or publish them in GitHub.

## Security and Privacy

- Never commit API keys or access tokens.
- Do not publish Gmail credentials or private customer information.
- Remove sensitive execution data before sharing screenshots or workflow exports.
- Review AI-generated drafts before sending them.
- Use fictional or anonymised test emails when demonstrating the workflow.

## Limitations

This is a demonstration project using fictional customer support policies. It is not connected to a real SOLOLIKE business.

The AI-generated responses depend on the retrieved policy and the quality of the incoming email. The system should be tested with different customer scenarios before any real-world deployment.

## Future Improvements

- Add better handling for empty or failed policy searches.
- Test a wider range of customer enquiries.
- Improve response-completeness validation.
- Add structured logging and error handling.
- Explore additional document retrieval and ranking strategies.
- Add evaluation tests to check policy accuracy.
- Consider escalation procedures for enquiries that require human judgement.

## Author

**Socorro Rebelo**

Data Science | Generative AI | AI Agents | Workflow Automation

This project was developed as a practical demonstration of AI-powered customer support automation using n8n and retrieval-augmented generation.
