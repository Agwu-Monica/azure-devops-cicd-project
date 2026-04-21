# azure-devops-cicd-project

This guide documents the complete process of setting up End-to-end Azure DevOps CI/CD pipeline with self-hosted agent, and Slack notifications

-------------------------------------------------------------------------------------------------------

1. Create Azure DevOps Organization
Go to https://dev.azure.com
Sign in with your Microsoft account
Click New Organization
Provide:
Organization Name
Region (choose closest location)
Click Create

-----------------------------------------------------------------------------------------

2. Create a New Project
Inside your organization, click New Project
Enter:
Project Name
Visibility (Private recommended)
Click Create

---------------------------------------------------------------------------------------------------

3. Create a Repository
Navigate to Repos
Click the repository dropdown → New Repository
Select:
Type: Git
Name: your-repo-name
Click Create

-------------------------------------------------------------------------------------------------------

4. Generate Personal Access Token (PAT)
Click your profile icon (top right)
Select Security
Click Personal Access Tokens
Click New Token
Configure:
Name: e.g. devops-access
Expiration: set as needed
Scopes: Full Access (or Code + Agent Pools)
Click Create
Copy and store the token securely

-----------------------------------------------------------------------------------------------------------

5. Clone Repository Using MobaXterm

Prerequisite
Ensure Git is installed:
git --version

Steps
In File Explorer, create a new folder
Open the folder
Right-click and select “Open MobaXterm here” (or navigate to the folder manually inside MobaXterm)

Run:
git clone https://dev.azure.com/<org-name>/<project-name>/_git/<repo-name>
Authenticate when prompted:
Username: your Azure email (or any value)
Password: paste your Personal Access Token (PAT)

---------------------------------------------------------------------------------------------

6. Configure Git Identity (Important)
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

------------------------------------------------------------------------------------------------

7. Verify Repository Locally
Open File Explorer
Navigate to the folder where you ran the clone command
Confirm a new folder with your repository name is created

---------------------------------------------------------------------------------------------------------

8. Inspect Repository Contents
Open the repository folder
You will see:
Project files
Folders
A hidden .git folder (used for version control)

-------------------------------------------------------------------------------------------------

9. Check Git Configuration
Open the .git folder
Locate the config file
Open it using Notepad or WordPad

You should see:

Remote repository URL
Fetch configuration
Origin settings

---------------------------------------------------------------------------------------

10. Open Project in Visual Studio Code
Open Visual Studio Code
Click File → Open Folder
Select the cloned repository
Click Open

--------------------------------------------------------------------------------------

11. Verify in VS Code
Confirm files and folders are visible
Open the Source Control tab
Ensure the repository is initialized and branch (e.g. main) is visible

----------------------------------------------------------------------------------------------

12. Create, Commit, and Push Changes
Create a test file
Create a file (e.g. test.txt)
Add content and save
Commit and push
git add .
git commit -m "Initial commit from local machine"
git push origin main

------------------------------------------------------------------------------------------

13. Verify in Azure DevOps
Go back to Azure DevOps
Navigate to Repos
Confirm your new file appears in the repository

-----------------------------------------------------------------------------------------

14. Slack Integration (Azure DevOps Notifications)
Configure Slack Webhook
Open Slack
Create a workspace and channel (e.g. #devops-alerts)
Go to Slack API → Create New App
Enable Incoming Webhooks
Add webhook to workspace
Copy the webhook URL

-------------------------------------------------------------------------------------------------

15. Configure Azure DevOps Service Hook
Open Azure DevOps project
Go to Project Settings → Service Hooks
Click Create Subscription
Select Slack
Trigger: Code pushed
Select repository and branch
Paste webhook URL
Click Finish

--------------------------------------------------------------------------------------------------

16. Test Slack Integration
git add .
git commit -m "Testing Slack integration"
git push origin main
You should receive a notification in Slack.

---------------------------------------------------------------------------------------

17. Pipeline Setup
Create Pipeline
Go to Pipelines
Click New Pipeline
Select Azure Repos Git
Select your repository
Choose Starter pipeline

-----------------------------------------------------------------------------------------

18. Configure YAML Pipeline
trigger:
- main

pool:
  name: MyCustomPool

steps:
- script: echo "Pipeline running on self-hosted agent"
  displayName: "Run script"

---------------------------------------------------------------------------------------------
  
19. Run Pipeline
Click Save and Run
Commit directly to main
Monitor pipeline execution

--------------------------------------------------------------------------------------

20. Verify Pipeline Execution
Check logs
Ensure agent picks up the job
Confirm successful run (green checkmark)
Self-Hosted Agent Setup (Custom Pool)

------------------------------------------------------------------------------------------------

21. Create Agent Pool
Go to Project Settings → Agent Pools
Click Add Pool
Select Self-hosted
Enter pool name (e.g. MyCustomPool)
Click Create

--------------------------------------------------------------------------------------------------

22. Download Agent
Open your agent pool
Click New Agent
Select Windows
Download the .zip file

-----------------------------------------------------------------------------------------------

23. Install Agent (PowerShell - Admin)
mkdir agent
cd agent
Extract agent (use provided command from Azure DevOps UI).

PS C:\> mkdir agent ; cd agent

 Extract the downloaded agent zip
PS C:\agent> Add-Type -AssemblyName System.IO.Compression.FileSystem
[System.IO.Compression.ZipFile]::ExtractToDirectory(
"$HOME\Downloads\vsts-agent-win-x64-4.270.0.zip", "$PWD")

------------------------------------------------------------------------------------------

24. Configure Agent
.\config.cmd

  Configure the agent (answer the prompts)
PS C:\agent> .\config.cmd

 Run the agent interactively
PS C:\agent> .\run.cmd

Provide:
Server URL
PAT
Pool name
Agent name

Server URL prompt
Enter: https://dev.azure.com/agwumonica100874

Auth type prompt
Enter: PAT

PAT prompt
Paste the token you saved 

Agent pool prompt
Enter: Default

Agent name prompt
Enter: agent3

Work folder prompt
Press Enter to keep default (_work)

 After running .\run.cmd you should see "Listening for Jobs" — agent3 is now online in your Azure DevOps pool.


------------------------------------------------------------------------------------------------------------

25. Run Agent as Service
Choose Y to run as service
Use default system account
Allow auto-start

-------------------------------------------------------------------------------------------

26. Verify Agent
Go to Azure DevOps
Open Agent Pool
Confirm agent status is Online

----------------------------------------------------------------------------------------------

Key Concepts Demonstrated
Version Control using Git
Azure DevOps project and repository management
Secure authentication using PAT
CI/CD pipeline creation using YAML
Self-hosted agent configuration
Slack integration using webhooks
Event-driven DevOps workflow

---------------------------------------------------------------------------------------------------

Conclusion
This project demonstrates a complete DevOps pipeline from code management to automation and notifications, simulating a real-world CI/CD environment.

--------------------------------------------------------------------------------------------------

https://github.com/user-attachments/assets/2bf1f392-4a74-452a-9a44-af235bcdc3f2

-----------------------------------------------------------------------------------------------------

👨‍💻 Author
Monica

*Cloud Engineering & DevOps

https://www.linkedin.com/in/agwumonica/
