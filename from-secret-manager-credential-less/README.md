# Using Okteto with Secret Managers via Credential-Less Authentication

This guide demonstrates how to securely integrate Okteto with external Secret Managers using OIDC (OpenID Connect) federation, eliminating the need to store static credentials. While this example uses Infisical and AWS, the same principles apply to any Secret Manager that supports OIDC authentication.

## Overview

The setup establishes a chain of trust:
1. **Okteto ↔ AWS**: Trust relationship using Okteto's Cloud Credentials feature
2. **AWS ↔ Infisical**: Trust relationship using Infisical's Machine Identity with AWS authentication
3. **Application**: Accesses secrets without storing any credentials

## Prerequisites

- Okteto instance
- AWS account with appropriate permissions
- Infisical instance (cloud or self-hosted)
- Administrative access to configure trust relationships

## Configuration Steps

### 1. Configure Okteto-AWS Trust Relationship

First, establish trust between your Okteto instance and AWS account using OIDC federation.

**Steps:**
1. Follow Okteto's documentation on [creating a trust relationship between your Okteto instance and AWS account](https://www.okteto.com/docs/admin/cloud-credentials/aws-cloud-credentials/)
2. Note the AWS IAM Role ARN and Account ID created during this process

> **Note:** This configuration is performed once per Okteto instance and can be reused across multiple projects.

### 2. Configure Infisical Machine Identity

Create a Machine Identity in Infisical that trusts your AWS IAM role.

**Steps:**
1. Navigate to your Infisical dashboard
2. Go to **Organization Settings > Machine Identities**
3. Create a new Machine Identity with AWS authentication
4. Configure the following settings:
   - **Allowed Principal ARNs**: Add the AWS IAM Role ARN from Step 1
   - **Allowed Account IDs**: Add the AWS Account ID from Step 1
   - **Token TTL**: Use default or customize based on your security requirements
5. Save the Machine Identity and note the **Machine Identity ID**

For detailed instructions, refer to [Infisical's AWS authentication documentation](https://infisical.com/docs/documentation/platform/identities/aws-auth).

### 3. Grant Project Access

Configure the Machine Identity to access your Infisical project.

**Steps:**
1. Navigate to your target project in Infisical
2. Go to **Project Settings > Access Management**
3. Add the Machine Identity created in Step 2
4. Assign the **Viewer** role (or minimum required permissions)

### 4. Configure Okteto Variables

Set up the required environment variables as Okteto Admin Variables:

| Variable | Description | Example |
|----------|-------------|---------|
| `AWS_REGION` | Your preferred AWS region | `us-east-1` |
| `INFISICAL_API_URL` | Your Infisical instance URL | `https://eu.infisical.com/api` |
| `INFISICAL_MACHINE_IDENTITY_ID` | Machine Identity ID from Step 2 | `abc123-bc1...` |
| `INFISICAL_PROJECT_ID` | Your Infisical project identifier | `def456-cf3...` |

**To set variables:**
1. Access your Okteto admin dashboard
2. Navigate to **Admin > Admin Variables**
3. Add each variable

> **Tip:** Find your Project ID in Infisical under **Project Settings > General** or refer to [Infisical's FAQ](https://infisical.com/docs/cli/faq#where-can-i-find-my-project-id).

For more information on Okteto variables, see the [official documentation](https://www.okteto.com/docs/core/okteto-variables/#setting-okteto-variables).

## Testing Your Configuration

### Sample Application Setup

This repository includes a test application that demonstrates the integration.

**Prerequisites:**
1. Create the following secrets in your Infisical project:
   - `MY_NAME`: Any string value (e.g., "Cindy Lopez")
   - `MY_COLOR`: Any color value (e.g., "green")

### Deployment Options

**Option 2: Using CLI**
```bash
git clone https://github.com/okteto-community/use-variables-with-docker-compose
cd use-variables-with-docker-compose/from-secret-manager-credential-less
okteto deploy
```

### Verification

Once deployed, the application should:
1. Successfully authenticate with Infisical using short term credentials
2. Retrieve the configured secrets (`MY_NAME` and `MY_COLOR`)
3. Display the values in the application interface

## Troubleshooting

### Common Issues

**Authentication Failures:**
- Verify AWS IAM Role ARN and Account ID match between Okteto and Infisical configurations
- Ensure the Machine Identity has appropriate project access in Infisical
- Check that all Okteto variables are correctly set and accessible

**Permission Errors:**
- Confirm the Machine Identity has at least Viewer access to the target project
- Verify the AWS role has necessary permissions for OIDC token exchange

**Network Issues:**
- Ensure Okteto can reach your Infisical instance URL
- Check firewall rules if using self-hosted Infisical

### Debug Steps

1. **Check Okteto logs** for authentication errors
2. **Verify variables** are properly injected into the application environment
3. **Test AWS credentials** independently
4. **Validate Infisical connectivity** using the Infisical CLI

## Security Considerations

- **Temporary Credentials**: All authentication uses short-lived tokens, reducing security risks
- **Least Privilege**: Grant only the minimum required permissions to Machine Identities
- **Audit Logging**: Monitor access logs in both AWS CloudTrail and Infisical