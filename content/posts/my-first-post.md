+++
title = 'Creating Github Pages'
date = 2023-09-12T15:13:25+08:00
draft = false
+++


Creating a Page in GitHub Pages
GitHub Pages allows you to easily host your static websites or documentation directly from your GitHub repository. In this guide, we'll walk you through the steps to create a GitHub Pages site.

Prerequisites
Before you start, make sure you have the following:

GitHub Account: You need a GitHub account. If you don't have one, you can sign up at GitHub.com.

GitHub Repository: Create a repository on GitHub where you will host your website. You can name it <username>.github.io to make it your personal site.

Steps
Follow these steps to create a page in GitHub Pages:

1. Create a Repository
Log in to your GitHub account.

Click the "+" button in the top right corner of the GitHub dashboard and select "New Repository."

Enter a repository name in the format <username>.github.io if you want to create a personal site. Replace <username> with your actual GitHub username.

Choose the repository visibility (public or private) according to your preference. For GitHub Pages to work, it's common to use a public repository.

Click the "Create repository" button.

2. Add Content
Clone your repository to your local machine using Git. Replace <username> with your GitHub username:

bash
Copy code
git clone https://github.com/<username>/<username>.github.io.git
Add your website content to the local repository. You can create HTML, Markdown, or other supported files for your site.

Commit and push your changes to GitHub:

bash
Copy code
git add .
git commit -m "Initial commit"
git push origin main
3. Configure GitHub Pages
Go to your repository on GitHub.

Click on the "Settings" tab.

Scroll down to the "GitHub Pages" section.

Under "Source," choose the branch you want to use for GitHub Pages. Typically, you'll use the "main" branch.

Click the "Save" button.

4. Access Your Site
Once configured, your GitHub Pages site should be live at https://<username>.github.io. It may take a few minutes for changes to reflect.

Custom Domain
If you want to use a custom domain for your GitHub Pages site, you can configure that in the "Custom domain" section of your repository's settings. Follow GitHub's documentation for setting up custom domains.

Congratulations! You've successfully created a GitHub Pages site to host your web content.

For more advanced configurations and additional features, refer to the GitHub Pages documentation.