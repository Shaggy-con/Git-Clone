# Custom Git Implementation in Python

This project is a simplified, custom implementation of Git written in Python. It's designed to help you understand the core concepts behind Git's functionality by replicating its basic commands. This guide will walk you through using `gitimpl.py`.

---

## 🚀 Getting Started(NOTE=Project Done by self,AI used for README)

To begin, save the entire Python code for this custom Git implementation into a file named `gitimpl.py`.

---

## 📌 Usage

Here are the step-by-step instructions for using the various commands.

### 1. Initialize a Repository

This command sets up the necessary `.git` directory and its internal structure.

* **Command:**
    ```bash
    python gitimpl.py init
    ```
* **What it does:**
    * Creates a `.git` directory.
    * Initializes the `objects` and `refs` subdirectories.
    * Creates a `HEAD` file pointing to the `main` branch.
* **Expected Output:**
    ```
    Initialized git directory
    ```

### 2. Add a File to the Object Store

This command hashes a file's content and stores it in Git's object database.

* First, create a sample file:
    ```bash
    echo "Hello, this is a test file" > test.txt
    ```
* **Command:**
    ```bash
    python gitimpl.py hash-object -w test.txt
    ```
* **What it does:**
    * Reads and compresses the file's content.
    * Computes the SHA-1 hash of the content.
    * Saves the compressed content in the `.git/objects` directory.
* **Expected Output** (the hash will vary based on content):
    ```
    a8a940627d132695a9769df883f85992f0ff4a43
    ```

### 3. Read Object Content

You can inspect the content of any Git object using its SHA-1 hash.

* **Command** (use the hash from the previous step):
    ```bash
    python gitimpl.py cat-file -p a8a940627d132695a9769df883f85992f0ff4a43
    ```
* **What it does:**
    * Finds the object by its hash in `.git/objects`.
    * Decompresses and displays the object's content.
* **Expected Output:**
    ```
    Hello, this is a test file
    ```

### 4. Create a Tree Object

This command captures the state of the current directory as a tree object.

* **Command:**
    ```bash
    python gitimpl.py write-tree
    ```
* **What it does:**
    * Scans the current directory (excluding `.git`).
    * Creates "blob" objects for files and "tree" objects for subdirectories.
    * Returns the SHA-1 hash of the root tree.
* **Expected Output** (hash will vary):
    ```
    d8329fc1cc938780ffdd9f94e0d364e0ea74f579
    ```

### 5. List Tree Contents

View the files and directories contained within a specific tree object.

* **Command** (use the hash from `write-tree`):
    ```bash
    python gitimpl.py ls-tree --name-only d8329fc1cc938780ffdd9f94e0d364e0ea74f579
    ```
* **What it does:**
    * Reads the specified tree object.
    * Displays the names of the entries within that tree.
* **Expected Output:**
    ```
    test.txt
    ```

### 6. Create a Commit

This command creates a commit object, which is a snapshot of the repository at a specific point in time.

* **Command** (use your tree hash; for the first commit, the parent is typically a placeholder):
    ```bash
    python gitimpl.py commit-tree d8329fc1cc938780ffdd9f94e0d364e0ea74f579 -p 00000001 -m "Initial commit"
    ```
* **What it does:**
    * Creates a commit object that points to the specified tree.
    * Includes metadata like author, committer, and a commit message.
    * Returns the SHA-1 hash of the new commit object.
* **Expected Output** (hash will vary):
    ```
    7bd63a1ef27465aa2e8e5cf7b60da123cd8adef2
    ```

### 7. Read a Commit

Inspect the details of a commit object.

* **Command** (use your commit hash):
    ```bash
    python gitimpl.py cat-file -p 7bd63a1ef27465aa2e8e5cf7b60da123cd8adef2
    ```
* **Expected Output:**
    ```
    tree d8329fc1cc938780ffdd9f94e0d364e0ea74f579
    parent 0000000000000000000000000000000000000000
    author Your Name <your.email@example.com> 1714599041 -0600
    committer Your Name <your.email@example.com> 1714599041 -0600

    Initial commit
    ```

### 8. Clone a Remote Repository

This command allows you to download a repository from a remote URL.

* **Command:**
    ```bash
    python gitimpl.py clone https://github.com/some-user/some-repo.git local-repo
    ```
* **What it does:**
    * Initializes a new local repository.
    * Fetches references (branches, tags) from the remote.
    * Downloads the necessary Git objects.
    * Checks out the files from the `HEAD` reference.

**Note:** Cloning requires the remote repository to support the Git smart HTTP protocol.

---

## 📋 Workflow Example: Making a New Commit

Here’s a quick example of how to make a second commit after modifying your project.

1.  **Create or modify a file:**
    ```bash
    echo "Adding more content" > more.txt
    ```
2.  **Hash the new file** (optional, as `write-tree` does this implicitly):
    ```bash
    python gitimpl.py hash-object -w more.txt
    ```
3.  **Create a new tree** that reflects the updated directory content. Note the new tree hash.
    ```bash
    python gitimpl.py write-tree
    ```
4.  **Create a new commit**, referencing the *previous commit hash* as the parent.
    ```bash
    python gitimpl.py commit-tree <new-tree-hash> -p <previous-commit-hash> -m "Add more.txt file"
    ```

---

## ⚠️ Limitations

This is a simplified implementation for educational purposes and lacks many of the advanced features of the official Git, including:

* **No index/staging area:** Changes are committed directly from the working directory.
* **Limited branch management:** No commands for creating, switching, or listing branches.
* **No remote tracking:** Does not track remote branches.
* **No merge functionality:** Cannot merge branches or resolve conflicts.
* **Basic clone only:** Supports only the HTTP protocol for cloning and does not handle complex authentication.
