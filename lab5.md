## STARTER_KIT AND AUTOMATIONS

---

## 1. `starter_kit.sh` Script

```bash
#!/bin/bash

# Create project directory structure
mkdir -p project/scripts project/docs project/data

# Add placeholder README.md in each folder
echo "# Project Root" > project/README.md
echo "# Scripts Directory" > project/scripts/README.md
echo "# Docs Directory" > project/docs/README.md
echo "# Data Directory" > project/data/README.md

# Print completion message
echo "Starter Kit Ready!"
```

Make executable:

```bash
chmod +x starter_kit.sh
```

---

## 2. `LAB_extra.md`

# LAB Extra – Starter Kit Automation

## Purpose of the Script

* The `starter_kit.sh` script automates the setup of a basic project environment by:

- Creating a root folder named `project` with three subfolders: `scripts`, `docs`, and `data`.
- Adding a placeholder `README.md` file in each folder to help developers understand the folder’s purpose.
- Printing a message to indicate the setup is complete.

This saves time and ensures consistent project structure across teams.

---

## Example Run

```bash
$ ./starter_kit.sh
Starter Kit Ready!
````
![image](<Screenshot (15).png>)

Directory structure created:

```
project/
├── README.md
├── scripts/
│   └── README.md
├── docs/
│   └── README.md
└── data/
    └── README.md
```

---

## Extra Questions

### 1. What does `mkdir -p` do?

The `mkdir -p` command creates the specified directory and any necessary parent directories that don't already exist. If the directories already exist, it won’t throw an error.

---

### 2. Why is automation useful in DevOps?

Automation in DevOps:

* **Saves time** by performing repetitive tasks without manual effort.
* **Reduces errors** caused by manual intervention.
* **Ensures consistency** in deployment, testing, and setup.
* **Enables faster delivery** of software by automating workflows.
* **Improves scalability** by automating infrastructure provisioning and configuration.

---


## Summary for your submission:

- `starter_kit.sh` — Creates the folder structure and README files.
- `LAB_extra.md` — Documentation and answers to extra questions.


---

