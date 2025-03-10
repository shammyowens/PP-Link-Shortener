# PP-Link-Shortener
This repository stores a link shortener application that uses Azure Static Web Apps and Power Apps to manage links.

Using this could allow you to have a standard domain and to use folders to launch different apps e.g.

https://apps.teamas.uk/Shout - launches the shoutout app
https://apps.teamas.uk/escape - launches the escape room app

Better than remembering this:

https://apps.powerapps.com/play/e/default-f2ed8e92-c285-4254-9a1d-33bb7557375a/a/7f43ba52-ed3f-4cad-a96c-4fa7d4faab82?tenantId=f2ed8e92-c285-4254-9a1d-33bb7557375a&hidenavbar=true

# Pre-requisites
*Azure Subscription*<br>
You need an Azure Static Web app.  You can follow this [quickstart guide](https://learn.microsoft.com/en-us/azure/static-web-apps/get-started-portal?tabs=vanilla-javascript&pivots=github) to deploy one.

*GitHub Repo*<br>
This solution depends on GitHub being the source repository for the Static Web App.  It can be amended to use other git repositories.

*Power Platform Environment*<br>
This solution must be deployed into a Power Platform environment.  It makes use of premium features, so requires a Power Apps per App/Premium licence for the people using the app to manage the links.

# Components

## Model Driven App
This is a simple app which allows the administrator to create, read, update and delete short links.

<img width="959" alt="image" src="https://github.com/user-attachments/assets/88292e65-fba0-4091-b4aa-e0b9d1ab0b27" />

## Power Automate Flow
This flow will run whenever a link has been created, updated or deleted.  It will then update the staticwebapp.config.json for the static web app.

![image](https://github.com/user-attachments/assets/2842aafb-a86d-4519-9368-6947483beaad)

## Environment Variables
There is an environment variables for GitHub repo, owner, name and email.

![image](https://github.com/user-attachments/assets/68d2a422-a637-4ee4-863d-dc5b1187c464)

## GitHub custom connector
Big thanks to Jan Vidar Elven and his post on [how to create a custom connector to GitHub](https://gotoguy.blog/2021/01/20/how-to-send-requests-to-github-api-from-power-platform-using-custom-connector).  This made the process a lot easier.

![image](https://github.com/user-attachments/assets/c5898376-b01c-43b9-bacd-81ec23ad68f0)

# How to use

- Create GitHub Repo
- Create Azure Static Web App and connect it to the GitHub Repo
- Configure Azure Static Web App with a custom domain (optional)
- Create GitHub personal access token.  More information on how to generate a personal access token can be found [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).
- Install the GitHub Connector solution into your Power Platform environment.  This has the custom connector that needs to be loaded before the main solution.
- Install the Link Shortener solution into your Power Platform environment.  Ensure the environment variables for owner and repo are updated appropriately.
- Create a connection for the GitHub connector ensuring that the key is in format [Bearer [Token]].
- Create some link records
- The CI/CD actions pipeline for the Static Web App should run and update the configuration
- Test the short url  

# HTML options

If your app needs to maintain the URL in the browser bar or you require parameters to be passed through into the app there are two options for you in this repo.

[iFrame](./html/iFrameExample.html) - This HTML file can be loaded into an Azure Static Web App and it will load the Power App in an iFrame and pass any parameters through.  Some functions will not work e.g. Copy function and responsiveness may be affected.  The custom URL will be maintained in the URL browser bar.

[Redirect](./html/RedirectExample.html) - This HTML file can be loaded into an Azure Static Web App and it will redirect the user to the native Power Apps URL with any parameters e.g. apps.powerapps.com.  App works natively but contain over app launch is lost.
