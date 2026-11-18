# Customer Support Chatbot on Amazon Bedrock AgentCore

> **Udacity project.** This is my solution to the "Customer Support Chatbot with Amazon Bedrock AgentCore" project from Udacity's AWS Agentic AI Nanodegree (Future AWS Agent Engineer). The project brief, starter code, CloudFormation templates and Lambda tool are provided by Udacity. The system prompt, test cases, evaluation runs and write-up are my own work.

## What it does

A support chatbot for a fictional online shop. One system prompt routes every message to one of three paths, with no separate classifier:

- **Bug reports:** asks for the description, steps to reproduce and environment, one question at a time. It then files a single ticket through the `create_bug_report` tool (AgentCore Gateway → Lambda → DynamoDB) and gives the customer the ticket ID.
- **Platform questions:** answers only from the shop FAQ embedded in the prompt.
- **Anything else:** hands the customer off to the human support phone line.

**Stack:** Amazon Bedrock AgentCore managed harness, Amazon Nova Pro, AgentCore Gateway, AWS Lambda, DynamoDB, Bedrock Evaluations (LLM-as-a-judge).

## Prompt iterations and results

| Run | Correctness | What changed |
|---|---|---|
| 1 | 0.67 | The bot called the tool on the first message, so the bug-report case scored 0. |
| 2 | 0.83 | Added a rule requiring a clarifying question before any tool call. The bug-report case scored 0.5. |
| 4 | [your score] | Required all three fields to be confirmed before the tool is called, and reworded the test expectation to match what a single turn can show. |

Multi-turn behavior (collecting all three fields, then filing exactly one ticket) was verified manually with `chat.py`, and the tickets appear in DynamoDB.

## Repo layout

- `project/starter/system_prompt.txt`: the chatbot prompt (my main deliverable)
- `project/starter/harness-tests.json`: test cases covering all three routes
- `project/starter/output_eval_dataset.jsonl`: harness responses used for evaluation
- `project/starter/submission/`: chat transcripts and evaluation screenshots

## How to run

```bash
cd project/starter
pip install -r requirements.txt
aws cloudformation deploy --template-file cloudformation-tool.yaml --stack-name bug-report-tool-stack --capabilities CAPABILITY_NAMED_IAM --region us-east-1
python setup_gateway.py
python create_harness.py
python chat.py
```

Delete everything when finished: `python cleanup_agentcore.py`, then delete the CloudFormation stacks.

## Credits and license

Starter code and project brief © Udacity, used under the license in `LICENSE.md`.
