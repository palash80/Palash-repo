# Branch Merge Notification Setup

---

| Author         | Created on | Version | Last updated by  | Last edited on |
|:----------------:|:------------:|:---------:|:------------------:|:----------------:|
| Palash Kamble  | 13-04-2024 | 1.0     | Palash Kamble    | 13-04-2024     |


---

## Table of Content


## Table of Content

| **Serial No.** | **Content**                                |
|:--------------:|--------------------------------------------|
| 1              | [Introduction](#introduction)             |
| 2              | [Purpose](#purpose)                       |
| 3              | [Why merging is important?](#why-merging-is-important-) |
| 4              | [Why Git?](#why-git-)                     |
| 5              | [Working flow](#working-flow)             |
| 6              | [Advantages](#advantages)                 |
| 7              | [Setup](#setup)                           |
| 8              | [Scenario](#scenario)                     |
| 9              | [Conclusion](#conclusion)                 |
| 10             | [Contact](#contact)                       |
| 11             | [References](#references)                 |


---


## Introduction


In version control systems, merging branches is essential for combining separate lines of development. It enables collaboration by integrating changes from one branch into another, facilitating the incorporation of new features or fixes. This process ensures that multiple developers can work independently without disrupting the main codebase. By merging branches, developers can consolidate their work into a unified codebase, promoting seamless collaboration and code integration.


---

## Purpose

The purpose of merging branches in version control systems (VCS) is to integrate changes made in separate branches back into the main codebase or designated integration branch. This process allows multiple developers to work on different features or fixes simultaneously without interfering with each other's work.

---

## Why merging is important ?

| **Purpose**                                            | **Description**                                                                                                   |
|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Experimentation and Exploration**                   | Data scientists often need to experiment with different models, hyperparameters, and data preprocessing techniques. By creating separate branches for each experiment, they can easily compare and track the results of different approaches. |
| **Collaboration and Teamwork**                        | Large ML projects often involve multiple data scientists and developers working on different parts of the codebase. Branching allows each team member to work on their own tasks or features without interfering with each other's work. |
| **Release Management**                                | When a new version of a model is ready for deployment, it may need to be tested and validated before being merged into the main codebase. By creating a release branch, developers can ensure that only tested and validated code is included in the final release. |
| **Risk Management and Disaster Recovery**             | In the event of a major bug or issue in the codebase, it may be necessary to roll back to a previous version. By creating backups and separate branches, developers can quickly revert to a previous state without losing data or progress. |


---


## Why Git ?


| **Key Reasons to Choose Git over Other VCS**          | **Description**                                                                                                   |
|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Distributed Version Control**                       | Git is a distributed version control system, allowing every developer to have a complete copy of the repository with its full history. This enables offline work and faster operations. |
| **Branching and Merging**                             | Git provides powerful branching and merging capabilities, allowing developers to work on new features or experiments without affecting the main codebase. Merging branches is also straightforward. |
| **Speed and Performance**                             | Git is known for its speed and performance, making operations like committing changes, branching, and merging efficient even for large projects. |
| **Open Source and Community Support**                  | Git is an open-source project maintained by a large community of developers, ensuring plenty of resources, documentation, and community support for users. |
| **Flexibility and Customizability**                   | Git offers flexibility and customizability, allowing developers to tailor their workflows, configurations, and integrations to suit their specific needs. |


---

## Working flow


![image](https://github.com/palash80/Palash-repo/assets/153359214/80b12324-4199-4726-b194-82d9b9b7890e)

- In our case, we have two branches one is the default branch called “main” and the other branch named “dev” and this is how our git repo looks before merging. Here git finds the common base, creates a new merge commit, and merged them.

---




![image](https://github.com/palash80/Palash-repo/assets/153359214/62e7919e-4268-4f01-a317-d70c54f903f2)

- When we perform merging, git always merges with the current branch from where we are performing the operation(in our case it is “main”). By this, the branch being merged is not affected.

---


## Advantages


1. Merge is simple, straightforward, and familiar
2. Merge preserves the entire history in chronological order
3. Merge maintains the branch’s context
4. Merge creates a complete log that helps developers understand the big picture, showing when and how each merge occurred
5. Merge makes it easy for users to find mistakes and correct them

---

## Setup


**Step-1**

- I have create a code in which we have to set the mail of sender & receptent for get the notification 

- Path = .github/workflows/main.yml


<pre><code>
name: Branch Merge Notification
on:
  push:
    branches:
      - main
  pull_request:
    types: [closed]
jobs:
  notify_on_merge:
    runs-on: ubuntu-latest
    steps:
      - name: Send Email Notification
        if: github.event_name == 'push' && github.ref == 'refs/heads/main' || github.event_name == 'pull_request' && github.event.action == 'closed' && github.event.pull_request.merged && github.event.pull_request.base.ref == 'main'
        uses: dawidd6/action-send-mail@v3
        with:
          server_address: smtp.gmail.com
          server_port: 587
          username: palash.kamble@opstree.com
          password: ${{ secrets.GMAIL_APP_PASSWORD }}
          to: palash.kamble@opstree.com
          from: palash.kamble@opstree.com
          subject: "Branch Merged into Main"
          body: |
            A branch has been merged into the main branch.
            - Repository: ${{ github.repository }}</code></pre>

![image](https://github.com/palash80/Palash-repo/assets/153359214/7d23159d-b0c1-4ddb-8c1a-5dca4d9d666a)

---


**Step-2**

- We have to generate an app password from google account ,of that email which we are using .

![image](https://github.com/palash80/Palash-repo/assets/153359214/7db7fabb-7ff5-4a8a-af29-e16044a1f962)

---

**Step-3**

- Store your app password in secrets as the steps are mentioned in screenshot

![image](https://github.com/palash80/Palash-repo/assets/153359214/253ca3bc-53cb-4d3e-8ccf-d0de56946023)

---  



## Scnerio

**Step-1**

- We have a two brances for this demo activity mention in below screenshot.

---

![image](https://github.com/palash80/Palash-repo/assets/153359214/846742bf-8f49-459e-aee4-d3114f84b541)

---

**Step-2**

- We have create a file [file55] in feature branch and added some code in it , mention below in screenshot


![image](https://github.com/palash80/Palash-repo/assets/153359214/5b2dbdd8-5a37-41bd-8925-1581891c5f71)

---




**Step-3**

- Then I checkout to the main branch after comminting the code from feature.

![image](https://github.com/palash80/Palash-repo/assets/153359214/70a96331-35bb-4ad7-a8aa-cf58fadccfcf)

---

**Step-4**

- Click on pull request

![image](https://github.com/palash80/Palash-repo/assets/153359214/34b995f7-a3a5-40a9-9e00-4c07c6f4a242)
 

---

**Step-5**

- Fill the detail according to your requirenment & create the pull request


![image](https://github.com/palash80/Palash-repo/assets/153359214/8741964f-156a-449b-b394-a3de1970fa54)

---

**Step-5**

- Fill the detail according to requirenments & merge pull request

![image](https://github.com/palash80/Palash-repo/assets/153359214/de0a2258-3c63-45c2-b6a3-af42498a80d5)


---

**Output**

![image](https://github.com/palash80/Palash-repo/assets/153359214/d96bb3a9-53d4-449b-bb76-db8bb39dc25d)


## Conclusion


"The branch merge notification setup in Git streamlines collaboration and ensures code integrity by integrating changes from separate branches into the main codebase. This facilitates experimentation, teamwork, release management, and disaster recovery, promoting efficient development workflows.


## Contact

Name           | Email Address
---------------|----------------------
Palash Kamble  | palash.kamble@opstree.com


---



## References


| **Links**                                                     | **Description**                                                            |
|---------------------------------------------------------------|----------------------------------------------------------------------------|
| [Branching and Merging - Almabetter](https://www.almabetter.com/bytes/tutorials/mlops/branching-and-merging)            | A tutorial on branching and merging in the context of MLOps.                 |
| [Git Merge - GeeksforGeeks](https://www.geeksforgeeks.org/git-merge/)                                                   | An article explaining the Git merge operation and its usage.                 |
| [Git Rebase vs Merge - Simplilearn](https://www.simplilearn.com/git-rebase-vs-merge-article)                             | An article comparing Git rebase and merge operations, highlighting their differences and use cases. |


---
