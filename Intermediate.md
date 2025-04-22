# 🔥 **Intermediate Guide**

This guide is packed with everything you need to elevate your Git experience, from resolving complex merge conflicts to understanding Git internals and enhancing performance. By the end, you'll not only be a master of GitHub, but also a Git Ninja!!🥷

---

## **Table of Contents**

1. [**Advanced Branching Models**](#1-advanced-branching-models)
2. [**Git Rebase Mastery**](#2-git-rebase-mastery)
3. [**Git Submodules and Subtrees**](#3-git-submodules-and-subtrees)
4. [**Advanced Merge Conflict Resolution**](#4-advanced-merge-conflict-resolution)
5. [**Git Hooks for Automation**](#5-git-hooks-for-automation)
6. [**Handling Large Repositories & Git LFS**](#6-handling-large-repositories--git-lfs)
7. [**Advanced GitHub Collaboration Strategies**](#7-advanced-github-collaboration-strategies)
8. [**Debugging and Git Internals**](#8-debugging-and-git-internals)
9. [**Optimizing Performance in Git**](#9-optimizing-performance-in-git)
10. [**Best Practices & Workflow Refinements**](#10-best-practices--workflow-refinements)
11. [**FAQ and Troubleshooting Guide**](#11-faq-and-troubleshooting-guide)

---

## 1. **Advanced Branching Models**

### **A. GitFlow vs GitHub Flow vs Trunk-Based Development**
In this section, we dive deeper into Git branching strategies and when to use each.

- **GitFlow**: Suitable for large projects with multiple release stages. Use for projects where releases and hotfixes are done on dedicated branches.
- **GitHub Flow**: Ideal for Continuous Deployment and small teams. GitHub Flow focuses on the `main` branch, with feature branches created and merged directly through Pull Requests (PRs).
- **Trunk-Based Development**: Emphasizes keeping the `main` branch always deployable. Developers work on very short-lived feature branches, continuously pushing to the `main` branch with no long-lived feature branches.

### **B. Hybrid Strategies**
Some organizations choose hybrid workflows:
- For example, mixing GitFlow and GitHub Flow for managing **features** with **release stages**.
- Another example: **Release branches** (GitFlow) + **Feature Branches** (GitHub Flow).

---

## 2. **Git Rebase Mastery**

### **A. Interactive Rebase with Multi-Commit Editing**
Take full control of your commit history by editing multiple commits, including:
1. **Rewording** commit messages.
2. **Squashing** commits (combining them into one).
3. **Reordering** commits.

#### Example: Rebase the last 5 commits
```bash
git rebase -i HEAD~5
```
In the editor, you can reorder, squash, or fixup the commits as needed.

**Common Interactive Rebase Actions**:
- `pick` — Keep the commit as is.
- `reword` — Change the commit message.
- `edit` — Modify the commit content.
- `squash` — Combine the commit with the previous one.
- `fixup` — Similar to squash but discards the commit message.

---

### **B. Rebase and Merge Strategies**
Git’s rebase and merge strategies have a significant impact on the **history** and **collaboration** of a project.

- **Rebase Before Merge**: Rebase your feature branch before merging into `main` to avoid a cluttered commit history with merge commits.
    ```bash
    git rebase main
    git push origin feature-branch --force
    ```

- **Preserving History with Merge Commit**: Use a `merge` to preserve the context of multiple feature branches in your project. This is helpful when debugging or maintaining complex histories.
    ```bash
    git checkout main
    git merge --no-ff feature-branch
    ```

---

## 3. **Git Submodules and Subtrees**

### **A. Working with Submodules**
Git submodules are an excellent way to work with multiple, independent repositories within a single repository.

#### **Steps**:
1. **Add Submodule**:
    ```bash
    git submodule add <repository-url> <path>
    ```
2. **Update Submodule**:
    ```bash
    git submodule update --remote
    ```

3. **Commit Submodule Change**:
    ```bash
    git add <submodule-path>
    git commit -m "Update submodule"
    git push origin main
    ```

#### **Pros and Cons** of Submodules:
- **Pros**:
  - Independent versioning.
  - Allows reusability.
  - Great for integrating with third-party projects.

- **Cons**:
  - Requires extra care during collaboration.
  - Can be complex to manage across large teams.

### **B. Git Subtrees**
Git subtree is another way to manage dependencies or other repositories inside your project without the complexity of submodules.

#### **Commands**:
- **Add a subtree**:
    ```bash
    git subtree add --prefix=subdir <repository-url> <branch> --squash
    ```
- **Pull updates from the subtree**:
    ```bash
    git subtree pull --prefix=subdir <repository-url> <branch> --squash
    ```

---

## 4. **Advanced Merge Conflict Resolution**

Handling complex merge conflicts can be a challenge when working with large teams. 

### **A. Resolving Conflicts Using Git’s Conflict Markers**
Git marks conflicts in files like this:
```bash
<<<<<<< HEAD
Changes in the current branch
=======
Changes in the incoming branch
>>>>>>> feature/xyz
```

Resolve the conflict by manually editing the file, then use:
```bash
git add <resolved-file>
git commit -m "Resolved merge conflict in <file>"
```

### **B. Using `git rerere` for Reusing Conflict Resolution**
The `git rerere` (reuse recorded resolution) feature can be helpful when resolving conflicts that arise repeatedly.

1. Enable it:
   ```bash
   git config --global rerere.enabled true
   ```

2. Once the conflict is resolved, Git will record the resolution, and you can reuse it for future similar conflicts.

---

## 5. **Git Hooks for Automation**

Automating tasks with **Git Hooks** allows you to streamline your development process.

### **A. Pre-commit Hook for Code Linting**
Prevent committing broken code by using the pre-commit hook to run tests or linters before code is committed.

```bash
#!/bin/sh
# Run tests
npm test || exit 1
```

### **B. Post-commit Hook for Notifications**
You can send Slack notifications or trigger automated deployment scripts after a commit.

```bash
#!/bin/sh
curl -X POST -H 'Content-type: application/json' --data '{"text":"New commit made!"}' YOUR_SLACK_WEBHOOK_URL
```

---

## 6. **Handling Large Repositories & Git LFS**

### **A. Git LFS (Large File Storage)**
Git LFS is ideal for storing large files (like images, videos, etc.) that shouldn't be stored in the main Git repository.

**Steps**:
1. **Install Git LFS**:
   ```bash
   git lfs install
   ```

2. **Track large files**:
   ```bash
   git lfs track "*.mp4"
   ```

3. **Push large files**:
   ```bash
   git add <large-file>
   git commit -m "Add large media files"
   git push origin main
   ```

**Considerations**:
- Git LFS keeps a pointer to the file, and the actual file is stored outside the Git repository.
- Git LFS can be a bit slower with large file operations. It’s important to track only the necessary files.

---

## 7. **Advanced GitHub Collaboration Strategies**

### **A. Code Reviews & Pull Request Strategies**
Effective code reviews are crucial in maintaining code quality across teams.

1. **Draft Pull Requests**: Use draft PRs to signal work in progress (WIP) and get early feedback.
2. **Squash and Merge**: Use this to keep commit history clean and avoid "merge commit noise".
3. **Auto-merge on CI Success**: Configure GitHub Actions to auto-merge a PR once it passes all CI checks.

---

## 8. **Debugging and Git Internals**

### **A. The Power of Git Reflog for Debugging**
Git's **reflog** is an essential tool for recovering lost commits and tracking branch updates.

#### Example:
```bash
git reflog
git checkout <commit-hash>
```

This shows the history of all branch updates and lets you undo operations like `reset`, `checkout`, and more.

---

## 9. **Optimizing Performance in Git**

### **A. Shallow Clones for Faster Clones**
For large repositories or projects with a lot of history, shallow cloning can significantly improve performance.

```bash
git clone --depth 1 <repository-url>
```

This will clone only the latest commit, improving speed.

### **B. Git GC (Garbage Collection)**
Use `git gc` to clean up your repository, optimize performance, and remove unnecessary files.

```bash
git gc --aggressive
```

This cleans up the repository by optimizing and packing objects to reduce size and improve performance.

---

## 10. **Best Practices & Workflow Refinements**

### **A. Keep Branches Short-Lived**
The longer you work on a branch, the higher the risk of merge conflicts. Keep branches short and feature-focused.

### **B. Automate with CI/CD**
Integrate your workflow with **CI/CD tools** like GitHub Actions, CircleCI, or Jenkins to automatically build, test, and deploy your code.

### **C. Use Descriptive Commit Messages**
Follow a commit message convention, like **Conventional Commits**, to keep commit history clean, readable, and easy to navigate.

---

## 11. **FAQ and Troubleshooting Guide**

### **Q: How do I undo a commit in Git?**
- **Undo the last commit**:
   ```bash
   git reset --soft HEAD~1  # Keep changes, uncommit
   ```
- **Undo the commit and changes**:
   ```bash
   git reset --hard HEAD~1  # Remove changes
   ```

### **Q: How do I squash multiple commits?**
```bash
git rebase -i HEAD~n
# Change pick to squash on all but the first commit
git push origin feature-branch --force
```

### **Q: How do I remove sensitive data from history?**
```bash
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch <file>' --prune-empty --tag-name-filter cat -- --all
```

---

### **Conclusion**

This **Intermediate Guide** has covered branching strategies, advanced Git techniques (like interactive rebase), powerful features (like submodules and Git hooks), and optimized workflows for large-scale teams and projects. The knowledge you’ve gained here will empower you to work smarter, handle conflicts efficiently, and scale your Git repositories in a professional development environment.

With this guide, you're now equipped with the right tools to push the boundaries of your Git knowledge. Go ahead and revolutionize your workflow! 🚀

## God speed mate