# Customer Support Chatbot — Amazon Bedrock Flows

## What this project does

A chatbot that reads a customer message and decides what to do with it:

- **Bug report** → logs the issue
- **FAQ question** → answers using the FAQ document
- **Other** → tells the customer to contact human support

## How it works

Customer message → Classifier (decides: bug_report / faq / other) → Condition Node (routes to the right path)

Routes:

- faq → answers using online_shop_faq.md
- other → redirects to support phone line
- bug → see note below

A Guardrail is connected to block harmful content and prompt injection attempts.

## Important note: the Bug Report Agent

The instructions ask for a Bedrock Agent to handle bug reports. I could not create one, because Amazon closed Bedrock Agents (Classic) to new accounts on July 30, 2026. My AWS account is new, so I don't have access to this feature.

I explain this in detail, with screenshots, in [`docs/agent-blocker-explanation.md`](docs/agent-blocker-explanation.md).

What I did instead:
- I tested the Lambda function and DynamoDB table separately, and they work correctly.
- The Bug Report branch in the Flow uses a simple placeholder that acknowledges the report (it does not claim a ticket was created, since the Agent isn't connected).
- I contacted Udacity support about this issue (see screenshots).

## Project structure

- `src/` — Lambda code
- `infrastructure/` — CloudFormation templates
- `flow/` — test cases for the Flow
- `evaluation/` — evaluation script and results
- `docs/` — FAQ document and Agent limitation explanation
- `screenshots/` — evidence for each part of the project

## Evaluation results

- Metric: Correctness
- Score: 1.00

## Observation

A correctness score of 1.00 means the flow's responses matched the expected outcome for all 5 test cases, including the FAQ answers, the direct other request, and the classifier's behavior under a prompt injection attempt. 
This confirms the classifier is reliably choosing the right path, and that the FAQ path stays grounded in the reference document instead of guessing.

One thing I'd improve with more time: the test set is small (5 cases). I would add more FAQ questions, including partially-matching ones, to check the model doesn't overstate confidence when the answer is only loosely related to the FAQ content.
