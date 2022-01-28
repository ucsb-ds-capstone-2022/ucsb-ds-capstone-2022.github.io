# Progress update guidelines

The sections below provide instructions for preparing and posting each progress update that your team will write for the class. Sections will be added as updates are due, so please refer back to this page in advance of submitting your posts. 

## General guidelines

Each update will differ in content, but in general the **purpose** of preparing progress updates is the same in each instance:

- step back, organize, and reflect on your project work to date;
- practice communicating your work to a broader audience in writing;
- draft materials that could be used in a report, manuscript, or slides.

These updates create excellent opporutnities to appreciate and acknowledge your efforts, discuss and affirm a shared view of the project with your team, recalibrate your perspective on what you're currently doing, set goals for future work, and -- since posts will be public -- ask for feedback from classmates and anyone else whose perspective you value.

Progress updates should be written for a varied **audience** (listed from most specialized to least):

- your peers in the capstone class;
- your sponsor's colleagues (researchers, specialists, management, etc.);
- prospective and future data science capstone students.

You can assume all groups share an interest in data science, but you should also assume that your audience has diverse technical expertise and that most readers will lack the domain knowledge relevant to your project. (Students, for example, may know more about statistical methods than your sponsor's colleagues.) You can, of course, include some details that may only be meaningful for technical specialists or domain experts, but overall your writing should be accessible to the broader audience described above. To that end, here are some **suggestions**:

- avoid overly technical jargon wherever possible by interpreting technical aspects of your updates in the context of your project;
- when needed, define any domain-specific terminology, technical language, or acronyms;
- label plots, tables, and figures clearly and provide descriptive captions;
- avoid showing codes, computer output, and equations unless they are absolutely essential to communicating or illustrating your point;
- use descriptive headers and subheaders (*e.g.,* 'Customer-agent conversation data' instaed of 'Datasets', 'Predicting arrests using machine learning' rather than 'Introduction') to organize your post;
- use links and references to point readers to background resources.


## Initial post (due Friday 2/4)

Your initial post should introduce your project.

The **aim** should be to *orient the reader to the project domain, datasets, objectives, and motivations*. Sufficiently so that one can easily answer the following questions after reading your post:

1. *Who is the project sponsor?* Are you working with a company or in a research lab? What does the company / lab do? 
1. *What is the subject of the team's project?* This is often easiest to characterize through the specific combination of domain or industry application and technical area/task that the project pertains to; for instance, distinguishing terrestrial habitats from satellite images (application) using image classification (technical task). How does the subject relate to the broader activities of the sponsor's lab / company?
2. *What data is the team working with?* This should be answerable at a certain level of detail -- the reader should be able to describe the main features/variables, observations, and how data were collected. Often, together with verbal explanation, a few example rows or a table of variable names and descriptions is a helpful way to present this information; of course, for large datasets, or for teams who are in the process of obtaining data, purely qualitative description may have to suffice.
3. *What is the team hoping to accomplish (or working on right now)?* The reader should have some sense of your goals or questions of interest. If you don't have much clarity about your objectives presently, that's okay -- you can instead try to articulate the question(s) that are guiding your first steps.

The **content** can be kept basic:

- domain **background** and project **motivation**;
- available **dataset(s)**;
- project **objectives**.

If it helps you to begin with some structure, create an outline with one header for each item above (but give them good titles -- see general suggestions). Review and critique [last year's project updates](https://ucsb-ds-capstone-2021.github.io/welcome.html) for further ideas about post organization. You do not need to stick to this particular outline and should do what suits your project best.

Your post should meet the following **criteria**:

- domain-specific background relevant to the project is clearly and concisely explained;
- for at least one primary dataset, the method of data collection, data quantity, and primary attributes (observations and variables) are clearly described;
- at least one figure is included; 
- at least one question of interest or project aim is clearly stated.

Don't worry about getting every last thing right for the initial submission. You'll do a peer review with another team shortly after submission and will be encouraged to post revisions before recieving feedback from your project mentor. More precisely, here are the **logistics** around post submission:

1. Pull requests to the website repo should be submitted by Friday February 4 at 5:00 pm PST.
2. Your team will review another team's post over the weekend (assignments determined in advance).
3. We will do a peer review in class on Monday February 7.
4. Revisions should be submitted (via pull request) by the end of the week.
5. Mentors will provide feedback. Further revisions optional (but encouraged).

## Posting instructions

### Step 1a. Fork class page repository

You need to fork the class page GitHub repository to your own GitHub. **You only need to do this once before your initial post -- you do not need to repeat this step every time you post an update**. This will create a copy of the repository associated with your personal GitHub account that remains linked with the original repository (so you can continue to fetch changes made to the class page). To fork class github repo:

1. Go to [class page github repository](https://github.com/ucsb-ds-capstone-2022/ucsb-ds-capstone-2022.github.io). At the top right corner click Fork.
2. Choose to fork to your own github.
3. You should see the repository appear in your personal account.

### Step 1b. Fetch upstream changes

**If this is your initial post, skip this step.** If this is not your initial post, you simply need to update your forked copy of the class page repository so that it is current with any changes to the class page made since you created the fork (or since your last update).

1. Navigate to your forked copy of the class page repository.
2. Just above the repository directory select 'Fetch upstream'.
3. Select 'Fetch and merge'.
4. You should see a message indicating that your current branch is up to date with the original repository.


### Step 2. Enable Actions
**Actions** will allow the github pages to automatically rebuild each time you commit an update.
To enable Actions:
1. On your own repo of class website choose tab **Actions**.
2. Click the green button to enable **Actions**.
3. You should see the workflows of this repo have an action called **deploy**.

### Step 3. Publish GitHub Pages
This step will allow you to view and test updates that you made on your fork of the repository before you submit your update to the class repository. To publish GitHub pages:
1. Go to Settings. Find the 'Pages' item in the navigation pane to the left.
2. Choose the **gh-pages** branch as the source. Click save.
3. Wait and refresh the webpage until you see a green tick with the message "your site is published".
4. Follow the website link to confirm that the page is published. You should see a duplicate of the class webpage.

### Step 4. Update team project
Now you have your own copy of the webpage to test run updates that you make for your teams. To update your project:
1. Clone the repository fork to your local directory. 
2. Navigate to your team's directory: `ucsb_ds_capstone_projects_2022/projects/["your team name"]`
3. Choose the file that you want to update or create a new markdown (.md) file for a new update. You can create directories for additional files such as images if you wish.
4. Update the file and commit changes. (You may find [this Jupyter book MyST Markdown guide](https://jupyterbook.org/content/myst.html) helpful.) If you are not modifying an existing file, find `ucsb_ds_capstone_projects_2022/_toc.yml`, find the 'chapter' for your group, and create a new 'section' by adding a line of the form `-file: projects/[YOUR TEAM NAME]/[YOUR NEW MARKDOWN FILE NAME]`.
5. Return to the GitHub repository in your web browser and navigate to **Actions**. Wait until you see the green tick indicating your page is rebuilt.
6. View your changes on your copy of the website; check for anything aesthetic or rendering issues, typos, and the like. Continue to update, commit changes, and preview as needed.
7. When you are ready, navigate to the **Code** section of the repository fork. Choose 'Contribute > Open pull request' and add any comments you would like to be reviewed along with your changes.

After you create a pull request, course staff will check your request for conflicts. If your request is accepted to the main repository, your update will be seen on the class website.
