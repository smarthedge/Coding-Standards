# Using GitHub with SSH (Secure Shell)

Last Updated : 12 Jul, 2025

**Secure Shell (SSH) Protocol** facilitates the communication among systems in an unsecured network by providing a secure channel over it. It safeguards the connection to remote servers enabling user authentication. Using SSH, you can connect to your **[GitHub](https://www.geeksforgeeks.org/blogs/ultimate-guide-git-github/)** account eliminating the need of giving username and password each time you push changes to the remote repository. The integration process involves setting up [**SSH Keys**](https://www.geeksforgeeks.org/computer-networks/introduction-to-ssh-secure-shell-keys/) within both the local and remote systems.

### Connect to GitHub using SSH

**Note:** If you already have an existing SSH key, you can skip step 1 and go to step 2. You can verify the same by listing all the existing keys using the command:

```
 $ ls -al ~/.ssh 
```

##### Steps to connect GitHub to SSH :

**Step 1:** **Generate SSH Key on Local System**

- Launch Terminal / Git Bash.

- Paste the below command and substitute your GitHub email address:

  ```bash
  $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com" 
  ```

  ```bash
  $ ssh-keygen -t ed25519 -C "your_email@example.com"
  ```

  

- Press Enter when prompted "Enter a file in which to save the key".

- Type a passphrase of your choice.

- Verify creation of SSH Key:

  ```
   $ ls -al ~/.ssh 
  ```

  

**Step 2:** **Add SSH Key to SSH Agent**

- Initiate ssh-agent:

  ```bash
   $ eval "$(ssh-agent -s)" 
  ```

- If your key is generated with a different name, replace id_rsa in the command below:

  ```bash
   $ ssh-add ~/.ssh/id_rsa 
  ```

  ```bash
  
  kenwa@Legion5 MINGW64 /d/Git/Coding-Standards (main)
  $ eval "$(ssh-agent -s)"
  Agent pid 1408
  
  kenwa@Legion5 MINGW64 /d/Git/Coding-Standards (main)
  $ ssh-add /d/users/kenwa/.ssh/kengit
  
  ```

**Step 3:** **Add the SSH Key to your GitHub Account**

- Copy key to the clipboard:

  ```
  WINDOWS
  $ clip < ~/.ssh/id_rsa.pub
  
  LINUX
  $ sudo apt-get install xclip
  $ xclip -sel clip < ~/.ssh/id_rsa.pub
  
  MAC
  $ pbcopy < ~/.ssh/id_rsa.pub
  ```

- Open the GitHub website and log in to your account. Go to the settings page from the menu in top right corner.

- Select "**SSH and GPG keys**" from the sidebar and click on "**New SSH key**" option.![Click to enlarge](https://media.geeksforgeeks.org/wp-content/uploads/20200330201614/Add-SSH-Key-12.png)

- Add relevant title in the "**Title**" field and paste the SSH key in the "**Key field**".

- Now, click on "**Add SSH key**".

**Step 4:** **Test the SSH Connection**

- Launch Terminal / Git Bash.

- Type:

  ```
   $ ssh -T git@github.com 
  ```

  

- Connection is established if you are prompted with the following message:

  >  Hi {username}! You've successfully authenticated, but GitHub does not provide shell access.