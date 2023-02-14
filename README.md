# Secure password-less deployments to Azure from GitHub Actions

Using a secret, whether a password or a token, to gain access to systems, APIs, or resources has always been the defacto way to protect resources on the Internet.  The problem is that no matter how complicated the secret is or how well protected the secret is kept, if that secret falls into the wrong hands, it exposes your resources and creates a security breach.

Using Federated Credentials in Azure, we can now securely provide access to our Azure resources from our GitHub Actions workflows without using secrets.  This article will walk through the process of setting up the App Registration, Federated Credentials, and GitHub Actions workflows to deploy resources to Azure without using secrets.

## App Registration

In this section, we will create a new App Registration in Azure Active Directory using the Azure portal, then we will give the App Registration access to a Azure Resource Group.

### Create the App Registration
1. Log in to the Azure portal
2. Click on the icon for `Azure Active Directory`
   - If the icon is not displayed, enter `Azure Active Directory` in the search bar at the top and then click on the link from the dropdown
3. In the left side navigation pane, under `Manage`, click the `App registrations` link
4. In the top navigation bar, click the `New registration` link
5. Enter a name for the App Registration
6. Set `Supported account types` to `Accounts in this organizational directory only (Microsoft Non-Production only - Single tenant)`
7. Click the `Register` button

### Create the Federated Credentials
1. In the left side navigation pane, under `Manage`, click the `Certificates & secrets` link
2. In the middle of the page, click the `Federated credentials` tab
3. Click the `+ Add credentials` button, the `Add a credentails` page will be displayed
4. From the `Federated credential scenario` dropdown, select `GitHub Actions deploying Azure resources` option
5. In the `Organization` textbox, enter the name of your GitHub organization
6. In the `Repository` textbox, enter the name of your GitHub repository

The `Entity type` provide 4 contexts in which the GitHub Actions workflow job can be executed from.
- Use `Environment` when the job is assigned to an environment configured in the GitHub repository, this would override workflows triggered by a pull request or push to a specific branch
- Use `Branch` when the workflow is triggered by a push to a specific branch
- Use `Pull request` when the workflow is triggered by the creation of a pull request or push to a branch that is the source branch of a pull request
- Use `Tag` when XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Typically, deployments in a GitHub Actions workflow will be associated with an Environment.  So in this example, we will use the Environment option.

7. In the `Entity` type dropdown, select `Environment`
8. In the `GitHub environment name` textbox, enter the name of the environment in your GitHub repository, for example `production`
9. In the `Name` textbox, enter a short description to identify the Federated Credential
10. Optionally, in the `Description` textbox, enter a more detailed description
11. Click the `Add` button

### Grant Resource Access to App Registration
1. Once the App Registration has been created, either create a new Resource Group or go to an existing Resource Group
2. In the left side navigation pane, click the `Access control (IAM)` link
3. In the top navigation bar, click the `+ Add` menu, and click `Add role assignment`
4. Click the `Contributor` role
5. Then click the `Members` tab at the top, or the `Next` button at the bottom
6. In the `Members` section, click the `+ Select members` link, a slide out will appear on the right
7. In the `Select` textbox, begin typing the name of the App Registration that you created in the previous section
8. When the name of the App Registration appears, click the App Registration name
9. Click the `Select` at the bottom
10. Click the `Review + assign` button at the bottom, then again click the `Review + assign` button


## References
https://github.com/MicrosoftDocs/azure-docs/workload-identity-federation-create-trust.md
