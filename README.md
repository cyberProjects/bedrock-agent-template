# Bedrock Agent RiskBot Deployment - Project Summary

## 🛠️ Project Overview

Built a **full AWS Bedrock Agent infrastructure** to provide **real-time political and economic risk analysis** by country. The system integrates Bedrock, Lambda, IAM roles, and OpenAPI schemas via a single CloudFormation template.

This deployment enables:

- Serverless action groups backed by Lambda.
- Real-time Bedrock model invocations (Nova-Lite).
- Seamless conversational API with structured schema.

---

## 🌐 Technologies Used

- **AWS Bedrock** (Agents, Model Inference)
- **AWS Lambda** (Serverless Action Group executor)
- **Amazon IAM** (Custom roles for Bedrock and Lambda)
- **AWS CloudFormation** (Full infrastructure-as-code)
- **Foundation Model**: `amazon.nova-lite-v1:0`
- **OpenAPI 3.0 Schema** (API contract definition)
- **Python 3.11** (Lambda runtime)

---

## 🔂 Template Structure

```plaintext
Resources:
    - BedrockAgentRole (IAM Role)
    - LambdaExecutionRole (IAM Role)
    - MyActionLambda (Lambda Function)
    - AllowBedrockToInvokeLambda (Lambda Permission)
    - MyBedrockAgent (Bedrock Agent Resource)
    - MyAgentAlias (Bedrock Agent Alias)
Outputs:
    - AgentId
    - AgentAliasId
    - ActionFunctionArn
```

---

## 📊 Functional Breakdown

### 1. **IAM Role Setup**

- `BedrockAgentRole`:
  - Allows Bedrock to assume role and invoke Lambda + Bedrock models.
- `LambdaExecutionRole`:
  - Allows Lambda basic execution logging.
  - Grants Lambda permission to invoke Bedrock foundation models.

### 2. **Lambda Function (Action Group Executor)**

- **Name**: `RiskBotActionExecutor`
- **Runtime**: Python 3.11
- **Functionality**:
  - Dynamically builds prompts for political or economic risk.
  - Calls Bedrock foundation model (`amazon.nova-lite-v1:0`) with structured messages.
  - Parses model output.
  - Returns results formatted per OpenAPI schema.
- **Environment Variable**:
  - `FOUNDATION_MODEL_ID` (default: `amazon.nova-lite-v1:0`)

### 3. **Bedrock Agent Configuration**

- **Name**: `MsecIndustriesAgent`
- **Instruction**: Risk advisor using real-time model evaluation.
- **Foundation Model**: `amazon.nova-lite-v1:0`
- **Idle Session TTL**: 300 seconds
- **AutoPrepare**: True (automatic warmup)
- **Action Group**:
  - Lambda Executor: `RiskBotActionExecutor`
  - OpenAPI Schema:
    - `/getPoliticalStability` (POST)
    - `/getEconomicRisk` (POST)

### 4. **OpenAPI Schema (Embedded in CloudFormation)**

- Defines two API paths:
  - `/getPoliticalStability` (Returns political risk summary)
  - `/getEconomicRisk` (Returns economic risk analysis)
- Strict request body validation on `country` parameter.

### 5. **Agent Alias**

- Alias Name: `prod`
- Links Bedrock Agent to production alias endpoint.

---

## 🚀 Key Features

- Fully serverless Bedrock Agent deployment.
- Real-time, dynamic prompt engineering inside Lambda.
- Secure IAM roles for Bedrock and Lambda interactions.
- Strong API contract enforcement with OpenAPI 3.0.
- Instant scalability and maintainability via CloudFormation.

---

## 📊 Future Enhancements

- Add more Action Groups for deeper country risk factors.
- Implement logging of prompts and responses for audit trails.
- Add fallback error handling and retry logic.
- Expand to multi-language support.

---

## 🔍 Project Outcome

Delivered a **secure, scalable, Bedrock-powered risk analysis agent** with a fully autonomous deployment via CloudFormation, real-time inference using foundation models, and a flexible API interface for integration into global risk monitoring systems.
