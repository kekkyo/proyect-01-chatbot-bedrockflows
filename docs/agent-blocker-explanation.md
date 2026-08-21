# Why the Bug Report Agent is Missing

## The problem

This project asks for a Bedrock Agent to handle bug reports (using a Lambda tool to create tickets). I could not create this Agent.

Amazon Bedrock Agents (Classic) stopped accepting new customers on July 30, 2026. My AWS account is new, so the option to create an Agent is not available in the console.


## What I built instead

1. I tested the Lambda function (`create_bug_report`) by itself. It works correctly and creates records in DynamoDB. See `screenshots/04-lambda-test-invoke.png` and `screenshots/05-dynamodb-record.png`.

2. In the Flow, the Bug Report path uses a simple Prompt node instead of an Agent. It replies to the customer acknowledging the report, but does not say a ticket was created (because it wasn't, automatically).

3. Everything else in the project works and was tested: FAQ answers, redirecting other requests, Guardrails, and automated evaluation.

## What the full version would look like

With a working Agent, the Bug Report path would connect to a Bedrock Agent that:
- Uses the `create_bug_report` Lambda as a tool
- Asks the customer for missing details (description is required; steps to reproduce and environment are optional)
- Creates a real ticket in DynamoDB

This is the intended design. It only needs the Agent to be created once this AWS limitation is resolved.