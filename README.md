# Git Submodules Guide for ALX Airbnb Clone 

## Problem
You have multiple ALX-mandated repos (e.g., `alx-airbnb-database`, `alx-airbnb-docs`) but want to organize them under **one main repo** (`airbnb-clone-project`) without breaking the ALX checker.

## Solution
Use **Git Submodules**! Here's how:

## Step 1: Understand the Structure
* **Main Repo**: `airbnb-clone-project` (Your primary project, holds the main `README.md`).
* **Submodules**:
   * `alx-airbnb-database` → Linked to `airbnb-clone-project/database/`
   * `alx-airbnb-docs` → Linked to `airbnb-clone-project/docs/`

The ALX checker **still sees** `alx-airbnb-database` and `alx-airbnb-docs` as standalone repos (no issues).

## Step 2: Set Up Submodules

### 1. Clone Your Main Repo
```bash
git clone git@github.com:yourusername/airbnb-clone-project.git
cd airbnb-clone-project
```

### 2. Add Submodules
```bash
git submodule add git@github.com:yourusername/alx-airbnb-database.git database
git submodule add git@github.com:yourusername/alx-airbnb-docs.git docs
```

* This creates:
   * `database/` (linked to `alx-airbnb-database`)
   * `docs/` (linked to `alx-airbnb-docs`)

### 3. Commit & Push the Main Repo
```bash
git add .gitmodules database/ docs/
git commit -m "Add ALX submodules (database & docs)"
git push origin main
```

## Step 3: Daily Workflow

### A. Working on ALX Tasks (Inside Submodules)
**Example: Updating** `database/ERD/requirements.md`

1. **Enter the submodule**:
```bash
cd database/
```

2. **Make changes**:
```bash
mkdir -p ERD && echo "New ERD specs" > ERD/requirements.md
```

3. **Commit & push (to** `alx-airbnb-database`):
```bash
git add .
git commit -m "Update ERD for ALX task"
git push origin main
```

4. **Return to main repo**:
```bash
cd ..
```

5. **(Optional) Update the main repo's submodule reference**:
```bash
git add database
git commit -m "Bump database submodule"
git push origin main
```

### B. Working on Main Project (Outside Submodules)
* Edit files **directly in** `airbnb-clone-project/` (e.g., `README.md`).
* Commit normally:
```bash
git add README.md
git commit -m "Update main README"
git push origin main
```

## Step 4: Cloning the Project Later
To clone **with submodules**:
```bash
git clone --recurse-submodules git@github.com:yourusername/airbnb-clone-project.git
```

*(Or, after cloning, run* `git submodule update --init` *to fetch submodules.)*

## Key Rules
1. **ALX Checker Needs**:
   * Always `git push` from **inside the submodule** (e.g., `database/`).
   * The checker **only cares about the standalone repos** (`alx-airbnb-database`, `alx-airbnb-docs`).
2. **Main Repo's Role**:
   * Just **references submodules** (like shortcuts).
   * You **don't need to push submodule changes from here** (but you can update their commit pointers).
3. **Fix "Dirty Submodule" Errors**: If Git complains:
```bash
cd database/  
git stash      # Save uncommitted changes  
cd ..
git add database
```

## Visual Structure
```
airbnb-clone-project/  
├── README.md          (Main project file)  
├── database/          (Submodule → alx-airbnb-database)  
│   └── ERD/requirements.md  (ALX task file)  
└── docs/              (Submodule → alx-airbnb-docs)  
    └── features-and-functionalities/README.md  
```

## Troubleshooting
* **Submodule not updating?** Run:
```bash
git submodule update --remote
```

* **Checker fails?** Double-check:
   * Submodule repo names **match ALX's requirements exactly**.
   * You **pushed changes from inside the submodule**.

## Final Notes
* **ALX repos stay independent** (checker-safe).
* **You get a clean, organized main repo**.
* **No more 10+ repos cluttering your GitHub!**