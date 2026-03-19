# AWS Amplify Deployment Guide

## Overview

This AWS Amplify configuration sets up a fullstack backend for the imissblog application with email-based authentication and a public Todo data model. Amplify Gen 2 provides a code-first approach to building backend services using TypeScript, with infrastructure defined as code and deployed via AWS Cloud Development Kit (CDK).

### Configuration Summary

- **Authentication**: Email-based login with guest (unauthenticated) access
- **Data Model**: Todo model with public read/write access
- **Authorization Mode**: Identity Pool (for public access)
- **Framework**: Next.js 15.2.4 with App Router
- **Backend Library**: @aws-amplify/backend v1.20.0

---

## Amplify Configuration

The Amplify backend is configured through TypeScript files in the `amplify/` directory:

```
amplify/
├── backend.ts          # Backend definition entry point
├── auth/
│   └── resource.ts     # Authentication configuration
├── data/
│   └── resource.ts     # Data schema and configuration
├── package.json        # Backend dependencies
└── tsconfig.json       # TypeScript configuration
```

### Backend Definition (`backend.ts`)

The `backend.ts` file defines which backend resources to create:

```typescript
import { defineBackend } from '@aws-amplify/backend'
import { auth } from './auth/resource'
import { data } from './data/resource'

defineBackend({
  auth,
  data,
})
```

This creates the two core resources: authentication and a data API.

---

## Auth Configuration (Email-based Login)

The authentication configuration is defined in `auth/resource.ts`:

```typescript
import { defineAuth } from '@aws-amplify/backend'

export const auth = defineAuth({
  loginWith: {
    email: true,
  },
})
```

### Features

- **Email-based authentication**: Users sign up and sign in using their email address
- **Passwordless option**: Can be extended to support passwordless authentication flows
- **Guest access**: Allows unauthenticated users to access resources (configured in data layer)
- **User pools**: Uses Amazon Cognito User Pools under the hood

### Authentication Flow

1. User provides email address
2. Cognito sends a one-time password (OTP) to the email
3. User enters OTP to complete sign-in
4. Cognito issues JWT tokens for authenticated requests

### Extending Authentication

To add password-based login:

```typescript
export const auth = defineAuth({
  loginWith: {
    email: {
      verificationEmailStyle: 'CODE',
    },
    password: true, // Add password support
  },
})
```

To add external identity providers (Google, Apple, etc.):

```typescript
export const auth = defineAuth({
  loginWith: {
    email: true,
    externalProviders: {
      google: {
        clientId: process.env.GOOGLE_CLIENT_ID!,
        clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
      },
    },
  },
})
```

---

## Data Schema (Todo Model with Public Access)

The data configuration is defined in `data/resource.ts`:

```typescript
import { type ClientSchema, a, defineData } from '@aws-amplify/backend'

const schema = a.schema({
  Todo: a
    .model({
      content: a.string(),
    })
    .authorization((allow) => [allow.guest()]),
})

export const data = defineData({
  schema,
  authorizationModes: {
    defaultAuthorizationMode: 'identityPool',
  },
})
```

### Schema Definition

- **Todo Model**: Basic to-do item with a single `content` field (string)
- **Authorization**: `allow.guest()` permits unauthenticated users to perform CRUD operations
- **Database**: Backed by Amazon DynamoDB with automatic provisioned capacity

### Data Model Fields

| Field     | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| id        | ID     | Auto-generated unique identifier |
| content   | String | The todo item content            |
| createdAt | String | Timestamp (auto-generated)       |
| updatedAt | String | Timestamp (auto-generated)       |

### Authorization Modes

- **Identity Pool**: Uses Cognito Identity Pools for authentication
- **Public Access**: guest() allows anyone to create, read, update, and delete records
- **Alternative modes**: Consider `allow.authenticated()` for signed-in users only

### Extending the Schema

Add a boolean `isDone` field:

```typescript
const schema = a.schema({
  Todo: a
    .model({
      content: a.string(),
      isDone: a.boolean(), // New field
    })
    .authorization((allow) => [allow.guest()]),
})
```

---

## Deployment Prerequisites

### AWS Account Setup

- AWS account with appropriate permissions (see AWS Permissions section below)
- AWS CLI configured with `aws configure` (credentials and region)
- Billing alerts configured to monitor costs

### Local Development Environment

- **Node.js**: v20.x or later (check Amplify compatibility)
- **npm** or **yarn**: Package manager (yarn推荐 for this project)
- **Git**: Version control for deployment workflows

### Install Amplify CLI

```bash
# Using npm
npm install -g aws-amplify

# Or using yarn
yarn global add aws-amplify
```

### Install Backend Dependencies

```bash
cd amplify
npm install
# or
yarn
```

---

## AWS Permissions Needed

The following minimum IAM permissions are required for deployment:

### Required Permissions

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:UpdateStack",
        "cloudformation:DescribeStacks",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeStackEvents",
        "cloudformation:GetTemplate",
        "cognito-idp:CreateUserPool",
        "cognito-idp:DescribeUserPool",
        "cognito-idp:UpdateUserPool",
        "cognito-idp:DeleteUserPool",
        "cognito-idp:CreateUserPoolClient",
        "cognito-idp:DescribeUserPoolClient",
        "cognito-idp:UpdateUserPoolClient",
        "cognito-idp:DeleteUserPoolClient",
        "cognito-idp:SetUserPoolMfaConfig",
        "cognito-identity:CreateIdentityPool",
        "cognito-identity:DescribeIdentityPool",
        "cognito-identity:UpdateIdentityPool",
        "cognito-identity:DeleteIdentityPool",
        "cognito-identity:SetIdentityPoolRules",
        "dynamodb:CreateTable",
        "dynamodb:DescribeTable",
        "dynamodb:UpdateTable",
        "dynamodb:DeleteTable",
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:Query",
        "dynamodb:Scan",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteItem",
        "apigateway:POST",
        "apigateway:GET",
        "apigateway:PUT",
        "apigateway:DELETE",
        "iam:CreateRole",
        "iam:AttachRolePolicy",
        "iam:PutRolePolicy",
        "lambda:CreateFunction",
        "lambda:InvokeFunction",
        "lambda:UpdateFunctionCode",
        "lambda:UpdateFunctionConfiguration",
        "lambda:GetFunction",
        "lambda:DeleteFunction",
        "s3:CreateBucket",
        "s3:PutBucketPolicy",
        "s3:DeleteBucket"
      ],
      "Resource": "*"
    }
  ]
}
```

### Recommended: Use AWS Managed Policies

For development, attach the `AWSDeveloperAccess` or `AWSCloudFormationFullAccess` policy. For production, create a custom policy with least-privilege permissions.

---

## CLI Commands Reference

### Setup Commands

```bash
# Initialize Amplify in the project
amplify init

# Install backend dependencies
cd amplify && npm install

# Type-check the backend code
npx tsc --noEmit
```

### Deployment Commands

```bash
# Preview changes before deploying
amplify push --dryrun

# Deploy all resources
amplify push

# Deploy with confirmation prompt disabled
amplify push --y

# Deploy and follow logs in real-time
amplify push --follow

# View deployed resources
amplify.status()

# List all resources
amplify status
```

### Backend Management

```bash
# Add new backend resources
amplify add <resource-type>

# Remove backend resources
amplify remove <resource-type>

# Pull latest backend changes (for team collaboration)
amplify pull

# Generate Typescript types for frontend
amplify generate types

# Clean build artifacts
amplify clean
```

### Environment Commands

```bash
# Create new environment
amplify env add

# Switch environments
amplify env switch <env-name>

# List environments
amplify env list

# Push to specific environment
amplify push --env <env-name>
```

---

## Deployment Steps

### Step 1: Initialize Amplify

```bash
# From project root
cd /Users/xichen/workplace/imissblog/imissblog

# Initialize Amplify (first time only)
amplify init
```

During initialization:

- Select your AWS profile
- Choose environment name (e.g., `dev`, `prod`)
- Confirm backend configuration defaults
- Wait for stack setup completion

### Step 2: Install Dependencies

```bash
cd amplify
npm install
# or
yarn
```

### Step 3: Type Check

```bash
npx tsc --noEmit
```

Fix any TypeScript errors before proceeding.

### Step 4: Deploy Backend

```bash
# From project root
amplify push
```

Review the changes and confirm deployment.

### Step 5: Verify Deployment

```bash
# Check resource status
amplify status

# View output values (API endpoints, region, etc.)
cat amplify/backend/amplify_outputs.json
```

### Step 6: Connect Frontend

Add Amplify configuration to your Next.js app:

```typescript
// app/layout.tsx or app/providers.tsx
import { Amplify } from 'aws-amplify'
import outputs from '@/amplify_outputs.json'

Amplify.configure(outputs)
```

---

## Frontend Integration Example

### Installing AWS Amplify Client

```bash
npm install aws-amplify
# or
yarn add aws-amplify
```

### Configuring the Client

```typescript
// In your root layout or provider
import { Amplify } from 'aws-amplify'
import outputs from '@/amplify_outputs.json'

Amplify.configure(outputs)
```

### Making API Calls

```typescript
// Client-side component
'use client'
import { generateClient } from 'aws-amplify/data'
import type { Schema } from '@/amplify/data/resource'

const client = generateClient<Schema>()

// Create a todo
const newTodo = await client.models.Todo.create({
  content: 'Learn AWS Amplify',
})

// List all todos
const { data: todos } = await client.models.Todo.list()
```

---

## Environment Management

### Creating Multiple Environments

```bash
# Create development environment
amplify env add --env dev

# Create production environment
amplify env add --env prod

# Deploy to specific environment
amplify push --env prod
```

### Environment-Specific Configuration

Each environment gets its own:

- Cognito User Pool
- DynamoDB Table
- API Gateway Endpoint
- IAM Roles and Policies

### Sharing Environment with Team

```bash
# Push backend to cloud
amplify push

# Team members pull the configuration
amplify pull --appId <app-id> --envName <env-name>
```

---

## Troubleshooting

### Common Issues

**Issue**: `Cannot find module '@aws-amplify/backend'`

**Solution**: Install backend dependencies

```bash
cd amplify
npm install
```

**Issue**: `User is not authorized to perform cloudformation:CreateStack`

**Solution**: Ensure AWS credentials have proper IAM permissions (see AWS Permissions section)

**Issue**: TypeScript compilation errors

**Solution**: Run `npx tsc --noEmit` to see errors and fix them before deploying

**Issue**: Stack deployment fails

**Solution**:

```bash
# Check stack events
aws cloudformation describe-stack-events --stack-name <stack-name>

# Delete failed stack and retry
amplify push --force
```

### Debug Commands

```bash
# View CloudFormation stack
amplify status --verbose

# See detailed logs
amplify push --debug

# Check generated files
ls -la .amplify/
```

---

## Notes About the Setup

### Backend Structure

- The `amplify/` directory contains all backend code
- Backend resources are defined in TypeScript files
- Infrastructure is deployed via AWS CloudFormation
- Generated files are stored in `.amplify/generated/`

### Authorization Considerations

- **Public Access**: The current configuration allows anyone to read/write Todos
- **Security**: For production, consider using `allow.authenticated()` or `allow.owner()`
- **GDPR/Compliance**: Public data may have regulatory implications

### Cost Considerations

- DynamoDB: Free tier includes 25 GB storage and 25 RCUs/WCUs
- Cognito: First 50,000 MAUs are free
- API Gateway: Free tier includes 1 million API calls per month
- CloudFront: Free tier includes 10 TB of data transfer per month

### Production Deployment

1. Create production environment: `amplify env add --env prod`
2. Update authorization rules for production (remove guest access)
3. Configure custom domain for hosting
4. Enable CloudWatch logging
5. Set up monitoring and alerts

### Customization Options

- **Add Storage**: for file uploads
- **Add Functions**: for custom business logic
- **Add API**: REST or GraphQL endpoints
- **Add Auth**: Social providers, MFA, custom auth challenges
- **Add Analytics**: Track user behavior
- **Add Geo**: Map and location services

---

## Additional Resources

- [Amplify Documentation](https://docs.amplify.aws/)
- [Auth Configuration](https://docs.amplify.aws/gen2/build-a-backend/auth)
- [Data Configuration](https://docs.amplify.aws/gen2/build-a-backend/data)
- [TypeScript Schema](https://docs.amplify.aws/gen2/build-a-backend/data/select-fields-and-relations)
- [AWS CDK](https://docs.amplify.aws/gen2/extending-cdk)
