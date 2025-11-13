# AI-Prognosis-System
This organizational repository is intended to act as version control between all six members of our AI team..

# **INSTRUCTIONS ON HOW TO GET YOUR REPOSITORY RUNNING**
#I shared the folder that consists of dataset.csv and dataset.db. Everyone has to individually log into their google drive account and do the following CRITICAL in order to make the code work.  
Go to your Google Drive homepage.
On the left-hand menu, click "Shared with me".
Find the Project_Data folder that you just shared with them.
Right-click the Project_Data folder.
Select "Add shortcut to Drive" (it may also be under an "Organize" sub-menu).
A small pop-up will ask where to put the shortcut. They must select "My Drive" as the location (the main root folder).

Once you do this, the Project_Data folder will appear inside your "My Drive" just as it does in mine. The file path in Colab will now be identical for everyone.

Here is a step-by-step guide with images that's required to be synced with our shared Github Repository.
This process has two parts:
Part 1: Creating the Personal Access Token (PAT) on your GitHub account.
Part 2: Adding that token to the Google Colab "Secrets" manager.

Part 1: How to Create Your GitHub Token
 
A Personal Access Token (PAT) is a secure password for your code. You must create one yourself.
 
Step 1: Go to Your Settings
 
Log in to GitHub. In the top-right corner, click your profile picture, then click "Settings".

Step 2: Go to Developer Settings
 
In the settings menu, scroll all the way down to the bottom of the left-hand sidebar. Click "Developer settings".

Step 3: Select "Tokens (classic)"
 
On the left-hand menu, click "Personal access tokens", then select "Tokens (classic)".

Step 4: Generate a New Token
 
Click the "Generate new token" button, and then select "Generate new token (classic)".

Step 5: Configure Your Token
 
This is the most important step. You need to give your token a name and the correct permissions.
Note (Name): Give your token a clear name, like colab-project-access.
Expiration: Set an expiration. 90 days is a good, secure choice.
Scope: Check the box labeled repo. This one checkbox is all you need. It will automatically check all the sub-options. This repo scope is what allows you to clone, pull, and push to the project.

Step 6: Generate and Copy Your Token
 
Scroll to the very bottom and click the green "Generate token" button.
You will now see a new screen with your token. It will look like ghp_....
CRITICAL WARNING:
This is the only time you will ever see this token. Once you leave this page, it is hidden forever.
Click the "copy" icon next to the token and paste it somewhere safe temporarily (like a text file).

Part 2: How to Add Your Token to Google Colab
 
Now, we add that token to Colab's secure "Secrets" manager.
 
Step 1: Open Your Colab Notebook
 
Go to your project notebook in Google Colab.
 
Step 2: Open the "Secrets" Manager
 
In the left-hand sidebar, click the "Key" icon (🔑). This will open the "Secrets" panel.

Step 3: Add the New Secret
 
Click the "Add a new secret" button.
Name: Type GITHUB_TOKEN (This must be exactly this name, all uppercase, with an underscore).
Value: Paste your new token that you just copied from GitHub (the ghp_... string).
Allow notebook access: Click the toggle switch to turn this ON.
You're done! You can now close the "Secrets" panel. Your notebook now has secure access to the GitHub repository, and you are ready to run the setup scripts.

After you've done this you'll be ready to clone the repository within your local colab environment.   To do this run the following script in a cell ONLY ONE TIME.
 
1. Script #1: One-Time Setup (Run This Only Once)
 
This script will mount your Google Drive, clone the "AI-Prognosis-System" repository, and set up your Git username.
Instructions:
Open a new, blank Colab notebook.
Make sure you have your GitHub token saved as GITHUB_TOKEN in the Colab Secrets (🔑 icon).
Copy and paste the code below into a cell.
Make the changes to the script mentioned above and then run the code blow in a colab cell.
Change the user.name and user.email to your own.
Run the cell.

from google.colab import drive, userdata
drive.mount('/content/drive')
 
# 1. Navigate to your main 'My Drive' folder
%cd /content/drive/MyDrive/
 
# 2. Get your GITHUB_TOKEN from Colab Secrets
try:
    GITHUB_TOKEN = userdata.get('GITHUB_TOKEN')
except userdata.SecretNotFoundError:
    raise Exception("ERROR: 'GITHUB_TOKEN' not found in Colab Secrets (the 🔑 icon).")
 
# 3. Clone the repository
print("Cloning repository...")
!git clone https://{GITHUB_TOKEN}@github.com/UST-MSAI-Neural-Prognosis/AI-Prognosis-System.git
print("...Cloning complete!")

# 4. Move into the new folder
%cd AI-Prognosis-System
 
# 5. Set up your Git identity
#    !!!! CHANGE THESE TO YOUR GITHUB USERNAME AND EMAIL !!!!
!git config --global user.name "YourGitHubUsername"
!git config --global user.email "your-email@example.com"
 
print("\n🎉 Setup complete! You are ready to start work.")
print("You can close this notebook now.")

2. Script #2: "Start of Work" (Run This Every Day)
 
Run this script every time you start working. It gets the latest main branch from and merges it into your personal branch to keep you up-to-date.
Instructions:
You can run this in a new notebook or create a "Git Control" notebook.
Change  (your_first_name)_branch is your personal branch name.  I have already created a branch for everyone.  
Make the changes to the script mentioned above and then run the code blow in a colab cell.

# --- "Start of Work" Script ---
from google.colab import drive
drive.mount('/content/drive')
 
# 1. Go to the repo folder
%cd /content/drive/MyDrive/AI-Prognosis-System/
 
# 2. Go to YOUR personal branch
#    !!!! CHANGE 'david_branch' THIS IS MY BRANCH NAME YOU UST CHANGE TO YOUR BRANCH NAME !!!!
!git checkout david_branch 
 
# 3. Pull all new updates from the server
!git fetch origin
 
# 4. This is the MOST IMPORTANT command.
#    It merges the latest 'main' into your branch.
print("Updating your branch with the latest 'main'...")
!git pull origin main
 
print("\n✅ You are up-to-date and ready to work!")
print("You can now go to Google Drive and open the notebook.")

3. 🌙 Script #3: "End of Work" (Run This When You're Done)
 
After you've worked on the SEIS_764_Final_Project_Master.ipynb notebook and saved your changes (File > Save), run this script. It will commit and push your work to your branch.
Instructions:
CHANGE david_branch to your personal branch name!!!!!!!!!!!!
CHANGE the commit message to be descriptive.
Make the changes to the script mentioned above and then run the code blow in a colab cell.  

# --- "End of Work" Script ---
from google.colab import drive
drive.mount('/content/drive')
 
# 1. Go to the repo folder
%cd /content/drive/MyDrive/AI-Prognosis-System/
 
# 2. (Make sure you saved your notebook in the other tab!)
#    Add the notebook file
!git add SEIS_764_Final_Project_Master.ipynb
 
# 3. Commit your changes
#    !!!! CHANGE THIS MESSAGE to be descriptive !!!!
!git commit -m "Added my new data analysis"
 
# 4. Push your changes to your branch
#    !!!! CHANGE 'david_scott_branch' to your branch name!!!!
!git push origin david_scott_branch
 
print("\n🚀 Success! Your changes are on GitHub.")
print("You can now go to GitHub.com to create your Pull Request.")

If you would like to keep an eye on commits to branches or to the main (master) branch then you can go to the repository in github and click on the "Watch" tab.  This will send you a notification everytime a push has been made to the repository.  
repo URL: https://github.com/UST-MSAI-Neural-Prognosis/AI-Prognosis-System
