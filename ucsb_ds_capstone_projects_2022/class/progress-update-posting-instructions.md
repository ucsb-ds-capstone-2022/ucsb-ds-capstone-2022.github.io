# How to post project updates

## Step 1a. Fork class page repository

You need to fork the class page GitHub repository to your own GitHub. **You only need to do this once before your initial post -- you do not need to repeat this step every time you post an update**. This will create a copy of the repository associated with your personal GitHub account that remains linked with the original repository (so you can continue to fetch changes made to the class page). To fork class github repo:

1. Go to [class page github repository](https://github.com/ucsb-ds-capstone-2022/ucsb-ds-capstone-2022.github.io). At the top right corner click Fork.
2. Choose to fork to your own github.
3. You should see the repository appear in your personal account.

## Step 1b. Fetch upstream changes

**If this is your initial post, skip this step.** If this is not your initial post, you simply need to update your forked copy of the class page repository so that it is current with any changes to the class page made since you created the fork (or since your last update).

1. Navigate to your forked copy of the class page repository.
2. Just above the repository directory select 'Fetch upstream'.
3. Select 'Fetch and merge'.
4. You should see a message indicating that your current branch is up to date with the original repository.


## Step 2. Enable Actions
**Actions** will allow the github pages to automatically rebuild each time you commit an update.
To enable Actions:
1. On your own repo of class website choose tab **Actions**.
2. Click the green button to enable **Actions**.
3. You should see the workflows of this repo have an action called **deploy**.

## Step 3. Publish GitHub Pages
This step will allow you to view and test updates that you made on your fork of the repository before you submit your update to the class repository. To publish GitHub pages:
1. Go to Settings. Find the 'Pages' item in the navigation pane to the left.
2. Choose the **gh-pages** branch as the source. Click save.
3. Wait and refresh the webpage until you see a green tick with the message "your site is published".
4. Follow the website link to confirm that the page is published. You should see a duplicate of the class webpage.

## Step 4. Update team project
Now you have your own copy of the webpage to test run updates that you make for your teams. To update your project:
1. Clone the repository fork to your local directory. 
2. Navigate to your team's directory: `ucsb_ds_capstone_projects_2022/projects/["your team name"]`
3. Choose the file that you want to update or create a new markdown (.md) file for a new update. You can create directories for additional files such as images if you wish.
4. Update the file and commit changes. (You may find [this Jupyter book MyST Markdown guide](https://jupyterbook.org/content/myst.html) helpful.) If you are not modifying an existing file, find `ucsb_ds_capstone_projects_2022/_toc.yml`, find the 'chapter' for your group, and create a new 'section' by adding a line of the form `-file: projects/[YOUR TEAM NAME]/[YOUR NEW MARKDOWN FILE NAME]`.
5. Return to the GitHub repository in your web browser and navigate to **Actions**. Wait until you see the green tick indicating your page is rebuilt.
6. View your changes on your copy of the website; check for anything aesthetic or rendering issues, typos, and the like. Continue to update, commit changes, and preview as needed.
7. When you are ready, navigate to the **Code** section of the repository fork. Choose 'Contribute > Open pull request' and add any comments you would like to be reviewed along with your changes.

After you create a pull request, course staff will check your request for conflicts. If your request is accepted to the main repository, your update will be seen on the class website.
