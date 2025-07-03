# Manipulate configurations before deploying your environment

This sample shows you how to manipulate environment variables before deploying your development environment with Okteto.

1. Create the following Okteto Admin Variables:
        
        MY_FIRST_NAME=cindy
        MY_FAVORITE_COLOR=green

2. Deploy the development environment
3. Notice how we are creating a new environment variable on the deploy section of  [okteto.yaml](okteto.yaml) to handle the fact that the variable has a different name.