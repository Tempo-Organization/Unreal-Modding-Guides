# How to Contribute

You need [`git`](https://git-scm.com/) and [`uv`](https://github.com/astral-sh/uv) installed.

---

## Setup

Clone the repo, then `cd` into it, and run the following commands:

```bash
uv venv
uv run pre-commit install
uv run pre-commit install --hook-type commit-msg
uv run pre-commit install --hook-type pre-push
```

---

## Working with Docs

- Run the docs locally for live editing:

  ```bash
  uv run mkdocs serve
  ```

- Build the offline docs:

  ```bash
  uv run mkdocs build
  ```

---

## Committing Changes

- Use `git add` / `git rm` to stage file changes.
- Use the commitizen CLI:

  ```bash
  uv run cz commit
  ```

  Or retry a previous commit:

  ```bash
  uv run cz commit --retry
  ```

- Push your changes:

  ```bash
  git push
  ```

---

## Repo Structure

- Place images in:

  ```
  assets/images/
  ```

- Place videos in:

  ```
  assets/videos/
  ```

- Configure page structure and order in:

  ```
  mkdocs.yml
  ```

- Custom site styles are in:

  ```
  docs/stylesheets/extra.css
  ```
