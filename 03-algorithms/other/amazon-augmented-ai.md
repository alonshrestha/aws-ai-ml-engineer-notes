# Amazon Augmented AI (A2I)

## What is it?
A **traffic controller** between your ML model and human reviewers. It doesn't make predictions or do reviews — it just **routes** low-confidence predictions to humans and manages the workflow.

## How It Works
1. Input data goes to your ML model
2. Model makes a prediction with a **confidence score**
3. A2I checks the confidence against a **threshold you set**
4. **High confidence** → result returned directly to client
5. **Low confidence** → A2I routes it to a human reviewer
6. Human reviews, corrects if needed, and the result is returned
7. Reviewed predictions are fed back to improve the model over time

## What A2I Actually Does
- **Routes** low-confidence predictions to human reviewers
- **Manages** the review workflow (assigns tasks, collects responses)
- **Stores** review history
- **Sends back** human-reviewed results to your client app

It's just the middleman — your model does the thinking, humans do the checking.

## Who Are the Human Reviewers?
- Your own employees
- 500,000+ contractors available through AWS
- Workers on **Amazon Mechanical Turk**
- Pre-screened vendors (for confidentiality requirements)

## Where Can Your Model Live?
- **AWS AI services** (e.g., Rekognition, Textract)
- **SageMaker** (custom-built models)
- **Anywhere else** — A2I integrates with external models too

## Exam Key Points
- A2I = orchestrator/router for human-in-the-loop ML workflows
- Triggers human review only on low-confidence predictions (threshold you configure)
- Review history stored for auditing
- Reviewed data feeds back into model to improve accuracy
- Works with AWS AI services, SageMaker, or custom models
- Supports confidentiality through pre-screened workforce
