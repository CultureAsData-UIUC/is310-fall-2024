---
title: "GitHub Style Guide"
permalink: /assessments/05-github-style-guide/
toc: true
toc_sticky: true
---

Welcome to the GitHub Style Guide! This guide is designed to help you understand the expectations for your GitHub repositories and how to structure your work for the semester. This guide will be updated throughout the semester as we learn more about GitHub and how to use it effectively for our projects. You will be graded on your use of GitHub and the quality of your repositories, so please make sure to follow these guidelines carefully, though we will also discuss them in class and provide feedback on your work.

<div class="notice--info">⚡️ This style guide will be updated as we progress through the course and cover new topics, so please refer back to this. The instructors will let you know when there is additional guidelines.</div>

## Repositories

These guidelines for your `is310-coding-assignments` repository and your group repository available on the `CultureAsData-UIUC` organization. We will cover all of these in class but this guide is here for your reference.

### General Guidelines

- [ ] Create a `.gitignore` file and ensure that you are not committing any private files or keys. More information [here](#gitignore-and-no-private-files-or-keys).
- [ ] Include a `README.md` file in your repository that provides an overview of the contents. More information [here](#readmemd-file).
- [ ] Make sure that your commits are associated with your GitHub account. More information [here](#commit-user-configuration).
- [ ] Ensure that your repository has a clear and organized folder structure. More information [here](#folder-structure).

### Specific Guidelines

#### .gitignore and No Private Files or Keys

Please make sure that you have a `.gitignore` file in your repository. This file should include any files that you do not want to be tracked by Git. This includes any files that contain private information or keys. If you are using an API key or any other sensitive information, please make sure that you are not committing this information to your repository.

Also ensure that sensitive or unnecessary files (such as temporary files, IDE config files, and large data files) are not pushed to the repository. Some common files to include in `.gitignore` are:

- IDE files (e.g., .vscode/, .idea/)
- OS-specific files (e.g., .DS_Store for macOS)
- Log files (e.g., *.log)
- Data or configuration files with sensitive information (e.g., *.env, *.json with API keys)

Example content for a `.gitignore` file:

```plaintext
# IDE files
.vscode/
.idea/

# OS-specific files
.DS_Store

# Log files
*.log

# Sensitive data files
*.env
*.json
```

#### README.md File

Your repository should have a `README.md` file that provides an overview of the contents of the repository. This file should include the following information:

- A brief description of the repository and its purpose.
- Instructions on how to run any code or scripts in the repository.
- Any dependencies that are required to run the code.
- A brief description of the files and directories in the repository.
- Any other relevant information that would help someone understand the contents of the repository.
- Contributors to the repository.

Here is an example of a `README.md` file:

```markdown
# IS310 Coding Assignments

This repository contains the coding assignments for IS310: Computing in the Humanities.

## Repository Structure

Each folder in this repository corresponds to a different assignment. You can find a list of assignments on the course website.

## Running the Code

To run the code in this repository, you will need to have Python installed on your machine. You can install all dependencies by running the following command:


`pip install -r requirements.txt`


### Contributors

- @student1

```

#### Commit User Configuration

Please make sure that your commits are associated with your GitHub account. This will help us track your contributions to the repository and ensure that you receive credit for your work.

If you are seeing a different username or email address in your commits, you can update your Git configuration with the following commands:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

You can check your current Git configuration with the following command:

```bash
git config --list
```

#### Folder Structure

Please make sure that your repository has a clear and organized folder structure. This will help you and others navigate the repository and find the files they need. You should create separate folders for different types of files or different assignments. For example, in your `is310-coding-assignments` repository, you need to have a folder for each assignment that corresponds to the correct name from the assignment. 

For your project repository, you might have folders for data, code, documentation, and other relevant files. Make sure that the folder structure is logical and easy to understand. You can also include a `README.md` file in each folder to provide more information about the contents of that folder, as well as document the structure in your main `README.md` file at the root of the repository.

For example, your `is310-coding-assignments` repository might look like this:

```plaintext
is310-coding-assignments/
├── .gitignore
├── README.md
├── init-is310-homework/
│   ├── images/
│   └── README.md
└── command-line-maze/
    ├── command_line.zip
    └── README.md
```

And so your `README.md` file might look like this:

```markdown
# IS310 Coding Assignments

This repository contains the coding assignments for IS310: Computing in the Humanities.

## Repository Structure

- `init-is310-homework`: Initial homework assignment.
- `command-line-maze`: Command line maze assignment.

```

## Issues & Projects

These guidelines are primarily for your group repository, but you can also use them for your individual repositories. We will cover these in class but this guide is here for your reference.

### General Guidelines

- [ ] Use GitHub Issues to track tasks, bugs, and other work items. More information [here](#using-github-issues).
- [ ] Use GitHub Projects to organize and track the progress of your work. More information [here](#using-github-projects).
- [ ] Make sure that your Issues and Projects are up-to-date and reflect the current state of your work. More information [here](#keeping-issues-and-projects-up-to-date).

### Specific Guidelines

#### Using GitHub Issues for Project Management 

In each of the group project repositories, you will now find a `project_manager_weekly_report.md` file in an `ISSUE_TEMPLATE` folder. This file is a template for your weekly project management report. You will need to fill out this template each week to provide an update on your progress and plans for the upcoming week. You will need to merge in the pull request that contains this file to your repository.

Once you have it merged, you can start using GitHub issues to document your project management. The first step is to go to the `Issues` tab in your repository.

<figure>
    <a href="{{site.baseurl}}/assets/images/new_github_issue.png">
        <img src="{{site.baseurl}}/assets/images/new_github_issue.png" alt="New GitHub Issue" class="image-popup">
    </a>
</figure>

In that tab, you should see an empty page that says "Welcome to issues!" You can click on the green "New issue" button to create a new issue.

<figure>
    <a href="{{site.baseurl}}/assets/images/issue_template.png">
        <img src="{{site.baseurl}}/assets/images/issue_template.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

That should take you to the issue template page. You'll notice after you merge in the pull request, that now you have a `Project Manger Weekly Report` template. You can use this template to create a new issue for your weekly report.

Alternatively, you can see below it says `Don't see your issue here? Open a blank issue.` You can click on that to open a blank issue, which we will cover in a moment.

##### Project Manager Weekly Report

To create a new issue using the `Project Manager Weekly Report` template, click on the green "Get started" button next to the template. This will open a new issue form with the template pre-filled.

The `Project Manager Weekly Report` template is a template for your weekly project management report. You will need to fill out this template each week to provide an update on your progress and plans for the upcoming week. The assigned project manager should create one of these issues each week and fill it out with the relevant information prior to class on Tuesday.

<figure>
    <a href="{{site.baseurl}}/assets/images/weekly_report_template.png">
        <img src="{{site.baseurl}}/assets/images/weekly_report_template.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

You'll notice that the template is written in Markdown, so you can use the Markdown styling we have learned to help you complete the form. It has a checklist of items that you need to fill out, as well as sections for you to include notes on your progress and plans for the upcoming week.

<figure>
    <a href="{{site.baseurl}}/assets/images/filled_example_report.png">
        <img src="{{site.baseurl}}/assets/images/filled_example_report.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

You can see a sample filled example above in this image, where the title has been updated and the checklist has been filled out. You can also click the `Preview` tab to see how your issue will look once it is created.

<figure>
    <a href="{{site.baseurl}}/assets/images/preview_report.png">
        <img src="{{site.baseurl}}/assets/images/preview_report.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

Finally once you are ready, you can click the green `Submit new issue` button to create the issue.

<figure>
    <a href="{{site.baseurl}}/assets/images/created_report.png">
        <img src="{{site.baseurl}}/assets/images/created_report.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

It should then show up under your `Issues` tab in your repository as a new issue under the `Open` tab.

Once you have completed all your the tasks are completed, you can close the issue by clicking the `Close issue` button. You can add a comment to explain why you are closing the issue, and then click the `Close issue` button to confirm.

<figure>
    <a href="{{site.baseurl}}/assets/images/issue_completed_comment.png">
        <img src="{{site.baseurl}}/assets/images/issue_completed_comment.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

Once the issue is closed, you will see your comment in the issue thread, and the issue will be marked as `completed`.

<figure>
    <a href="{{site.baseurl}}/assets/images/issue_completed.png">
        <img src="{{site.baseurl}}/assets/images/issue_completed.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

Then the issue will be moved to the `Closed` tab in your repository.

<figure>
    <a href="{{site.baseurl}}/assets/images/issue_closed.png">
        <img src="{{site.baseurl}}/assets/images/issue_closed.png" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

If you need to reopen the issue, you can click the `Reopen issue` button to reopen the issue and move it back to the `Open` tab. Also if you run to any issues, you can always ask the instructors for help.

##### Blank Issues

Besides the `Project Manager Weekly Report` template, you can also create blank issues to track tasks, bugs, and other work items. To create a new issue without a template, click on the green "New issue" button on the `Issues` tab. You can read more about issues here [https://docs.github.com/en/issues/tracking-your-work-with-issues/quickstart](https://docs.github.com/en/issues/tracking-your-work-with-issues/quickstart).

Issues are useful for a number of reasons:

1. **They help you divide tasks and assign them to group members.**

<figure>
    <a href="https://docs.github.com/assets/cb-19901/mw-1440/images/help/issues/assignee-menu.webp">
        <img src="https://docs.github.com/assets/cb-19901/mw-1440/images/help/issues/assignee-menu.webp" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

2. **They can be a place to discuss and document decisions.**

You can add checklists to issues to track progress on tasks.

<figure>
    <a href="https://docs.github.com/assets/cb-80075/mw-1440/images/help/issues/issue-task-list-raw.webp">
        <img src="https://docs.github.com/assets/cb-80075/mw-1440/images/help/issues/issue-task-list-raw.webp" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

You can also add labels to issues to categorize them.

<figure>
    <a href="https://docs.github.com/assets/cb-120194/mw-1440/images/help/issues/issue-with-label.webp">
        <img src="https://docs.github.com/assets/cb-120194/mw-1440/images/help/issues/issue-with-label.webp" alt="New GitHub Issue Form" class="image-popup">
    </a>
</figure>

3. **They also allow you to work with GitHub's project interface**, which we will cover next.


#### Using GitHub Projects for Project Management

Part of the project management process is to use GitHub Projects to organize and track the progress of your work. You can create a new project by clicking on the `Projects` tab in your repository.