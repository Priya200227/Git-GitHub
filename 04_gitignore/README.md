# `.gitignore` and Clean Repositories

## What is `.gitignore`?

`.gitignore` tells Git which files or directories should not be tracked.

Example:

```gitignore
.venv/
__pycache__/
.env
*.csv
```

---

## Why This Matters

I should not accidentally push:

* API keys
* Passwords
* Environment variables
* Large datasets
* Temporary files
* Virtual environments
* Generated files

For Python projects, a typical `.gitignore` might include:

```gitignore
.venv/
__pycache__/
*.pyc
.env
.ipynb_checkpoints/
```

For data projects, I may also ignore:

```gitignore
data/raw/
data/processed/
*.csv
*.xlsx
```

The exact choice depends on whether the data is supposed to be version-controlled.

---

## `.gitkeep`

Git does not normally track an empty directory.

A `.gitkeep` file is commonly used as a placeholder:

```text
data/
└── raw/
    └── .gitkeep
```

`.gitkeep` is a convention rather than a special Git command or feature.

---

## Important Rule

`.gitignore` is **not a security mechanism**.

If a secret has already been committed and pushed, simply adding the file to `.gitignore` does not remove the secret from Git history.

Secrets should never be committed in the first place.

