# Use Okteto with a Secret Manager

This sample shows you how to use Okteto with a Secret Manager. This sample uses infisical. 

1. Create a project in infisical and add the MY_NAME and MY_COLOR secrets
2. Run `infisical init` to initialize/update the `.infisical.json` file
3. Create a Service Token in Infisical and add it as an Admin Variable named `INFISICAL_TOKEN` in Okteto
4. Deploy the environment