# Contributing to 404unfound Repo

Thank you for considering contributing to our repository! We welcome contributions from all members. Please follow the guidelines below to contribute effectively.

## Adding Writeups

1. Fork this repository to your GitHub account.
2. If necessary, create a folder for the CTF in `content/writeups`. 
3. Inside the CTF folder, create a Markdown(.md) file for your writeup using the following template:

```markdown
---
title: "[CTF Name] Challenge Name"
date: YYYY-MM-DDTHH:MM:SS+08:00
author: "Your Name"
tags:
  - ctf category
ctfs:
  -  ctf name
---

# Challenge Name
Your writeup content goes here.

To embed an image, add it to the same directory (folder) and use the following Markdown format:

![Image](image.png)

OR automatically generate the Markdown file using: 
```bash
hugo new content/writeups/.../index.md
```

4. Make sure to replace [CTF Name], Challenge Name, YYYY-MM-DDTHH:MM:SS+08:00, Your Name, and ctf category with the appropriate information.

## Adding Profile Info

1. Fork this repository to your GitHub account.
2. Thank you for being a part of our CTF team! To add your profile information, open the `data/members.yml` file and add in your information following this format:

```yaml
- name: Your Name
  course: Your Course
  pfp: your_profile_picture.jpeg
  roles:
    - CTF Role 1
    - CTF Role 2
  desc: Your description or bio
  linkedin: https://www.linkedin.com/in/yourprofile
  github: https://github.com/yourgithub
  twitter: https://https://twitter.com/yourtwitter
  personal: https://www.yourwebsite.com

```
Note that `pfp`, `linkedin`, `github`, `twitter` and  `personal` are **optional** fields.

3. Ensure that you replace the placeholders with your actual information.

4. If you are adding a profile picture, add it to the `static/profile_pics/` directory.

## Submitting Your Changes

1. Create a new branch for your changes:
```git
git checkout -b your-feature
```
2. Commit your changes with clear and concise messages.
3. Push your changes to your forked repository:
```git
git push origin your-feature
```
4. Create a pull request (PR) to the main branch of the 404unfound repository. Include a descriptive title and provide details about your changes in the PR description.
5. Your PR will be reviewed, and if everything is in order, it will be merged :D

Thank you for contributing to the 404unfound CTF Team repository!

