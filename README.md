# PP-Link-Shortener
This repository stores a link shortener application that uses Azure Static Web Apps and Power Apps to manage links

# Pre-requisites
*Azure Subscription*
You need an Azure Static Web app.  You can follow this [quickstart guide](https://learn.microsoft.com/en-us/azure/static-web-apps/get-started-portal?tabs=vanilla-javascript&pivots=github) to deploy one.

*GitHub Repo*
This solution depends on GitHub being the source repository for the Static Web App.  It can be amended to use other git repositories.

*Power Platform Environment*
This solution must be deployed into a Power Platform environment.  It makes use of premium features, so requires a Power Apps per App/Premium licence for the people using the app to manage the links.

# Components

## Model Driven App
This is a simple app which allows the administrator to create, read, update and delete short links.

## Power Automate Flow
This flow will run whenever a link has been created, updated or deleted.  It will then update the staticwebapp.config.json for the static web app.

## Environment Variables
There is an environment variables for GitHub repo and owner.

## GitHub custom connector
Big thanks to Jan Vidar Elven and his post on [how to create a custom connector to GitHub](https://gotoguy.blog/2021/01/20/how-to-send-requests-to-github-api-from-power-platform-using-custom-connector).  This made the process a lot easier.

# How to use

- Create GitHub Repo
- Create Azure Static Web App and connect it to the GitHub Repo
- Configure Azure Static Web App with a custom domain (optional)
- Create GitHub personal access token.  More information on how to generate a personal access token can be found [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).
- Install the solution into your Power Platform environment.  Ensure the environment variables for owner and repo are updated appropriately.
- Create a connection for the GitHub connector ensuring that the key is in format [Bearer [Token]].
- Create some link records
- The CI/CD actions pipeline for the Static Web App should run and update the configuration
- Test the short url  

