# SonarQube - Azure DevOps Integration

SonarQube's integration with Azure DevOps allows you to maintain code quality and security in your Azure DevOps repositories. It is compatible with both Azure DevOps Server and Azure DevOps Services.

With this integration, you'll be able to:

- Import your Azure DevOps repositories - Import your Azure DevOps repositories into SonarQube to easily set up SonarQube projects.
- Analyze projects with Azure Pipelines - Integrate analysis into your build pipeline. Starting in Developer Edition, SonarScanners running in Azure Pipelines jobs can automatically detect branches or pull requests being built, so you don't need to specifically pass them as parameters to the scanner.
- Report your Quality Gate status to your pull requests - (starting in Developer Edition) See your Quality Gate and code metric results right in Azure DevOps so you know if it's safe to merge your changes.

<br>

### Branch Analysis

Community Edition **doesn't support** the analysis of multiple branches, so you can only analyze your main branch. Starting in Developer Edition, you can analyze multiple branches and pull requests.

<br>

## Importing your Azure DevOps repositories into SonarQube

Setting up the import of Azure DevOps repositories into SonarQube allows you to easily create SonarQube projects from your Azure DevOps repositories. If you're using Developer Edition or above, this is also the first step in adding pull request decoration.

To set up the import of Azure DevOps repositories:

1. Set your global DevOps Platform settings
2. Add a personal access token for importing repositories

<br>

### Setting your global settings

To import your Azure DevOps repositories into SonarQube, you need to first set your global SonarQube settings. Navigate to **Administration > Configuration > General Settings > DevOps Platform Integrations**, select the **Azure DevOps** tab, and click the **Create configuration** button. Specify the following settings:

- **Configuration Name** (Enterprise and Data Center Edition only) – The name used to identify your Azure DevOps configuration at the project level. Use something succinct and easily recognizable.

- **Azure DevOps collection/organization URL** – If you are using Azure DevOps Server, provide your full Azure DevOps collection URL. For example, https://ado.your-company.com/DefaultCollection. If you are using Azure DevOps Services, provide your full Azure DevOps organization URL. For example, https://dev.azure.com/your_organization.

- **Personal Access Token** – An Azure DevOps user account is used to decorate Pull Requests. We recommend using a dedicated Azure DevOps account with Administrator permissions. You need a [personal access token](https://docs.microsoft.com/en-us/previous-versions/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=tfs-2017&tabs=Windows) from this account with the scope authorized for **Code > Read & Write** for the repositories that will be analyzed. Administrators can encrypt this token at **Administration > Configuration > Encryption** See the **Settings Encryption** section of the [Security](https://docs.sonarqube.org/latest/instance-administration/security/) page for more information.
<br>
This personal access token is used to report your Quality Gate status to your pull requests. You'll be asked for another personal access token for importing projects in the following section.

<br>

### Adding a personal access token for importing repositories

After setting your global settings, you can add a project from Azure DevOps by clicking the **Add project** button in the upper-right corner of the **Projects** homepage and selecting **Azure DevOps**.

Then, you'll be asked to provide a personal access token with Code (Read & Write) scope so SonarQube can access and list your Azure DevOps projects. This token will be stored in SonarQube and can be revoked at anytime in Azure DevOps.

After saving your personal access token, you'll see a list of your Azure DevOps projects that you can **set up** to add them to SonarQube. Setting up your projects this way also sets your project settings for pull request decoration.

For information on analyzing your projects with Azure Pipelines, see the **Analyzing projects with Azure Pipelines** section below.

## Analyzing projects with Azure Pipelines

SonarScanners running in Azure Pipelines jobs can automatically detect branches or pull requests being built, so you don't need to specifically pass them as parameters to the scanner.

#### **_WARNING: Automatic branch detection is only available when using Git._**

### Installing your extension

From Visual Studio Marketplace, install the [SonarQube extension}(https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarqube) by clicking the **Get it free** button.

#### Azure DevOps Server - build agents

If you are using [Microsoft-hosted build agents](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml) then there is nothing else to install. The extension will work with all of the hosted agents (Windows, Linux, and macOS).

If you are self-hosting the build agents, make sure you have at least the minimum SonarQube-supported version of Java installed.

### Adding a new SonarQube Service Endpoint

After installing your extension, you need to declare your SonarQube server as a service endpoint in your Azure DevOps project settings:

1. In Azure DevOps, go to Project Settings > Service connections.
2. Click New service connection and select SonarQube from the service connection list.
3. Enter your SonarQube Server URL, an Authentication Token, and a memorable Service connection name. Then, click Save.

### Configuring branch analysis

After adding your SonarQube service endpoint, you'll need to configure branch analysis. You'll use the following tasks in your build definitions to analyze your projects:

- Prepare Analysis Configuration - This task configures the required settings before executing the build.
- Run Code Analysis - (Not used in Maven or Gradle projects) This task executes the analysis of source code.
- Publish Quality Gate Result - this task displays the Quality Gate status in the build summary letting you know if your code meets quality standards for production. This task may increase your build time as your pipeline has to wait for SonarQube to process the analysis report. It is highly recommended but optional.

Select your build technology below to expand the instructions for configuring branch analysis and to see an example .yml file.

### - .NET

1. In Azure DevOps, create or edit a Build Pipeline, and add a new Prepare Analysis Configuration task before your build task:
   - Select the SonarQube server endpoint you created in the Adding a new SonarQube Service Endpoint section.
   - Under Choose a way to run the analysis, select Integrate with MSBuild.
   - In the project key field, enter your project key.
2. Add a new Run Code Analysis task after your build task.
3. Add a new Publish Quality Gate Result on your build pipeline summary.
4. Under the Triggers tab of your pipeline, check Enable continuous integration, and select all of the branches for which you want SonarQube analysis to run automatically.
5. Save your pipeline.

**.yml example:**

    trigger:
    - master # or the name of your main branch
    - feature/*

    steps:
    # Prepare Analysis Configuration task
    - task: SonarQubePrepare@5
    inputs:
        SonarQube: 'YourSonarqubeServerEndpoint'
        scannerMode: 'MSBuild'
        projectKey: 'YourProjectKey'

    # Run Code Analysis task
    - task: SonarQubeAnalyze@5

    # Publish Quality Gate Result task
    - task: SonarQubePublish@5
    inputs:
        pollingTimeoutSec: '300'

### Maven or Gradle

1. In Azure DevOps, create or edit a Build Pipeline, and add a new Prepare Analysis Configuration task before your build task:
- Select the SonarQube server endpoint you created in the Adding a new SonarQube Service Endpoint section.
- Under Choose a way to run the analysis, select Integrate with Maven or Gradle.
- Expand the Advanced section and replace the Additional Properties with the following snippet:

    ```
    # Additional properties that will be passed to the scanner,
    # Put one key=value per line, example:
    # sonar.exclusions=**/*.bin
    sonar.projectKey=YourProjectKey
    ```

2. Edit or add a new Maven or Gradle task
    - Under Code Analysis, check Run SonarQube or SonarCloud Analysis.

3. Add a new Publish Quality Gate Result on your build pipeline summary.
4. Under the Triggers tab of your pipeline, check Enable continuous integration, and select all of the branches for which you want SonarQube analysis to run automatically.
5. Save your pipeline.

.yml example:
```
trigger:
- master # or the name of your main branch
- feature/*

steps:
# Prepare Analysis Configuration task
- task: SonarQubePrepare@5
  inputs:
    SonarQube: 'YourSonarqubeServerEndpoint'
    scannerMode: 'Other'
    extraProperties: 'sonar.projectKey=YourProjectKey'

# Publish Quality Gate Result task
- task: SonarQubePublish@5
  inputs:
    pollingTimeoutSec: '300'
```

### [Other (JavaScript, TypeScript, Go, Python, PHP, etc.)](https://docs.sonarqube.org/latest/analysis/azuredevops-integration/#)

### [Analyzing a C/C++/Obj-C project](https://docs.sonarqube.org/latest/analysis/azuredevops-integration/#)

### **Running your pipeline**

Commit and push your code to trigger the pipeline execution and SonarQube analysis. New pushes on your branches (and pull requests if you set up pull request analysis) trigger a new analysis in SonarQube.

### **Maintaining pull request code quality and security**

Using pull requests allows you to prevent unsafe or substandard code from being merged with your main branch. The following branch policies can help you maintain your code quality and safety by analyzing code and identifying issues in all of the pull requests on your project. These policies are optional, but they're highly recommended so you can quickly track, identify, and remediate issues in your code.

#### **Ensuring your pull requests are automatically analyzed**

Ensure all of your pull requests get automatically analyzed by adding a [build validation branch policy](https://docs.microsoft.com/en-us/azure/devops/pipelines/repos/azure-repos-git#pr-triggers) on the target branch.

### **Preventing pull request merges when the Quality Gate fails**

Prevent the merge of pull requests with a failed Quality Gate by adding a SonarQube/quality gate [status check branch policy](https://docs.microsoft.com/en-us/azure/devops/repos/git/pr-status-policy) on the target branch.

```
Projects configured as part of a mono repository cannot use this status check branch policy to prevent pull request merges.
```

Check out this [video](https://www.youtube.com/watch?v=be5aw9_7bBU) for a quick overview on preventing pull requests from being merged when they are failing the Quality Gate.

## **Reporting your Quality Gate status in Azure DevOps**

After you've set up SonarQube to import your Azure DevOps repositories as shown in the **Importing your Azure DevOps repositories into SonarQube** above, SonarQube can report your Quality Gate status and analysis metrics directly to your Azure DevOps pull requests.

To do this, add a project from Azure DevOps by clicking the **Add project** button in the upper-right corner of the **Projects** homepage and select **Azure DevOps** from the drop-down menu.

Then, follow the steps in SonarQube to analyze your project. SonarQube automatically sets the project settings required to show your Quality Gate in your pull requests.

```
To report your Quality Gate status in your pull requests, a SonarQube analysis needs to be run on your code. You can find the additional parameters required for pull request analysis on the Pull Request Analysis page.
```

If you're creating your projects manually or adding Quality Gate reporting to an existing project, see the following section.

### **Reporting your Quality Gate status in manually created or existing projects**

SonarQube can also report your Quality Gate status to Azure DevOps pull requests for existing and manually-created projects. After setting your global settings as shown in the **Importing your Azure DevOps repositories into SonarQube** section above, set the following project settings at **Project Settings > General Settings > DevOps Platform Integration:**

- **Project name**
- **Repository name**

<br>

### **Advanced configuration**

[Reporting your Quality Gate status on pull requests in a mono repository](https://docs.sonarqube.org/latest/analysis/azuredevops-integration/#)

[Configuring multiple DevOps Platform instances](https://docs.sonarqube.org/latest/analysis/azuredevops-integration/#)

[Linking issues](https://docs.sonarqube.org/latest/analysis/azuredevops-integration/#)

