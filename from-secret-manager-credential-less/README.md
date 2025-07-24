# Use Okteto with a Secret Manager using Credential-Less Authentication

This sample shows you how to use Okteto's Cloud Credentials feature to authenticate with a Secret Manager without exchanging credentials.   

This sample uses Infisical and AWS, but the concepts apply for any Secret Manager that supports OIDC authentication.


## Configuration 

### Configure Cloud Credentials

The first step is to configure a trust relationship between Okteto and your AWS account.  This will be accomplished using Okteto's Cloud Credential features together with AWS' OIDC federation feature. 

Please refer to Okteto's documentation on [how to create a trust relationship between your Okteto instance and your AWS account](https://www.okteto.com/docs/admin/cloud-credentials/aws-cloud-credentials/) to complete this step. This needs to be performed once per Okteto instance.

### Configure Infisical Machine Identity

The second step is to create a trust relationship between Infisical and a role on your AWS account. This is how Infisical knows who it is supposed to give access and to which projects. 

Please refer to Infisical's documentation on [how to register a Machine Identity for AWS](https://infisical.com/docs/documentation/platform/identities/aws-auth).

- In "Allowed Pricipal ARNs", add the AWS IAM Role ARN that you configured in Okteto Cloud Credentials.
- In "Allowed Account IDs", add the AWS Account Number that you configured in Okteto Cloud Credentials.
- Leave the default values for everything else.

### Give the Infisical Machine Identity access to your Project
Navigate to your project on Infisical, and in the Access Management section, give the Machine Identity that you created in the previous step access to the project. We recommend you give it `Viewer` access.

### Configure Okteto Variables

For lhe last part of the configuration, we will create the following Admin variables. These are used by the Infisical CLI when requesting access. 

- `AWS_REGION`:  your prefered AWS Region (e.g. `us-east-1`).
- `INFISICAL_API_URL`: the URL of your Infisical instance (e.g. `https://eu.infisical.com/api`).
- `INFISICAL_MACHINE_IDENTITY_ID`: The machine identity ID you created.
- `INFISICAL_PROJECT_ID`: [Your Infisical Project ID](https://infisical.com/docs/cli/faq#where-can-i-find-my-project-id).


> For more information, please refer to Okteto's documentation on [how to set and use variables](https://www.okteto.com/docs/core/okteto-variables/#setting-okteto-variables). 

## Test your configuration

This repository includes a sample app. The app expects `MY_NAME` and `MY_COLOR` variables to be defined on the Infisical project. Once created, deploy the application using Okteto's UI or by running `okteto deploy` on the terminal. 

