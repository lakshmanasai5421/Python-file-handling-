### Basic GitHub Commands for Pushing Code

**1. Initialize a Git Repository**
Creates a new Git repository in your project folder.

**Syntax:**

```bash
git init
```

---

**2. Add Files to the Staging Area**
Stages all project files for the next commit.

**Syntax:**

```bash
git add .
```

---

**3. Commit Changes**
Saves the staged changes with a message describing the update.

**Syntax:**

```bash
git commit -m "commit message"
```

**Example:**

```bash
git commit -m "Added login feature"
```

---

**4. Connect to a GitHub Repository**
Links your local repository to a remote GitHub repository.

**Syntax:**

```bash
git remote add origin <repository_url>
```

**Example:**

```bash
git remote add origin https://github.com/user/project.git
```

---

**5. Push Code to GitHub**
Uploads your local commits to the GitHub repository.

**Syntax:**

```bash
git push -u origin main
```

---

### Quick Workflow

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin <repository_url>
git push -u origin main
```

These commands are the most commonly used steps for uploading a project from your local computer to GitHub.
