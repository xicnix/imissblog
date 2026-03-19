# Blog Post Created: Deploying ImissBlog to AWS Amplify

## Title

Deploying ImissBlog to AWS Amplify - Complete Guide

## File Location

`/Users/xichen/workplace/imissblog/imissblog/data/blog/opencode/deploying-to-aws-amplify.mdx`

## Main Sections Covered

1. Introduction to AWS Amplify Gen 2
2. Prerequisites (Node.js, AWS account, AWS CLI, package manager)
3. Project Structure Overview (backend.ts, auth/, data/, package.json, tsconfig.json)
4. Auth Configuration (Email-based login with OTP)
5. Data Schema (Todo model with guest access)
6. Deployment Steps (Install dependencies, type check, push backend, verify)
7. Frontend Integration (Install aws-amplify, configure provider, make API calls)
8. Environment Management (Multiple environments, team collaboration)
9. Common CLI Commands (Reference table)
10. Troubleshooting (Common issues and solutions)
11. Conclusion and Next Steps

## Key Information About the Deployment Process

- Backend is defined in TypeScript using `@aws-amplify/backend` package
- Two core resources: authentication (Cognito User Pools) and data API
- Email-based auth with optional password support
- Data schema defined using Amplify's schema builder with authorization policies
- Deployment via `amplify push` command which builds TypeScript and creates CloudFormation stack
- Generates `amplify_outputs.json` with client configuration
- Frontend uses `aws-amplify` package with client generated from Schema types

## Notable Features or Highlights

- Code-first backend configuration (infrastructure as code)
- Type-safe fullstack development with TypeScript
- Guest access support for unauthenticated users
- Multi-environment support (dev, prod) with amplify env commands
- Team collaboration via amplify push/pull workflow
- Complete reference table of common CLI commands
- Troubleshooting section with common error solutions
- Next.js integration examples with provider pattern
- Link to Amplify Documentation for further learning
