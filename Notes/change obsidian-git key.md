# How to change the GitHub token if expired in Obsidian?
Obsidian-git is an Obsidian community that allows us to sync our vault with git. I had a token that is expired and I want to make some pushes. However I did not see some docs in the documentation website. What is good is that Obsidian works well with file system and I can access the repo using the terminal. With this access, we can set the URL.

We need to follow the steps:
1. Generate a [token-authentication](https://github.blog/security/application-security/token-authentication-requirements-for-git-operations/)
2. Set the URL as follows using a terminal
   ```sh
   git remote set-url origin https://your_token@github.com/your_name/your_repo.git
   ```
3. We can start push on Obsidian UI.