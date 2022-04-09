# Developer Guidelines

Test

Some rules and advice for developers who want to contribute to this project and keep it 
human friendly.

## GIT
[Git](https://git-scm.com/) is used as the version control system. Each developer must be 
familiar with the basic functionality of git before contributing to the project. If 
necessary, look at this [guide](https://githowto.com/).

### Commit messages
[**Conventional Commits** specification](https://www.conventionalcommits.org/en/v1.0.0-beta.2/) 
shall be used for commit messages. Read the specification carefully and use
[this compact list of conventional commit types](https://github.com/commitizen/conventional-commit-types/blob/master/index.json)
to help you. You'll quickly get used to it!

Common types of commit messages:

| Type      | Meaning                                                    |
|-----------|------------------------------------------------------------|
| docs:     | Documentation only changes.                                |
| feat:     | A new feature.                                             |
| fix:      | A bug fix.                                                 |
| perf:     | A code change that improves performance.                   |
| refactor: | A code change that neither fixes a bug nor adds a feature. |
| style:    | Changes that do not affect the meaning of the code.        |

Basic rules:

 - Try to keep the first line under 72 characters.
 - A blank line must separate the subject from the description.
 - Use the **imperative** voice.


### Branching rules

In the project, the following branch types are recommended:

| Type            | Branch   | Meaning                                          |
|-----------------|----------|--------------------------------------------------|
| Stable          | stable   | Accepts merges from Working and Hotfixes         |
| Working         | master   | Accepts merges from Features/Issues and Hotfixes |
| Features/Issues | topic/*	 | Always branch of HEAD of Working                 |
| Hotfix          | hotfix/* | Always branch of Stable                          |

`feat`, `bug` and `docs` can be used as a `topic`. Each commit to the `stable` branch 
must contain a tag with the following format: `<version>-stable`. See the
[article](https://gist.github.com/digitaljhelms/4287848) for detailed information.

> **Warning:** branch naming rules from the article are NOT used in this project – only 
branching rules.


### Updating the Stable branch
It might be a little tricky to update the Stable branch.  
It should be updated at the end of each shape with the latest stable features.

1. Create a new branch from the Master (or from the latest stable commit on it, if there are already new features in development inside the master branch). Let's call it **"chore/update-stable-branch"**.
2. Run `git merge -s ours origin/stable`
3. Push the new branch to the cloud with `git push`
3. Create a PR on Azure DevOps from `chore/update-stable-branch` to `stable`
4. Check for any conflicts
5. Complete the PR
6. Done! :)

### Hints
  - **Do not** forget to remove the branch after a successful Pull Request to the main branch 
  to prevent contamination of the repository.
  - **Do not** make additional changes in a branch that is already merged with the main 
  one.
  - If the changes are small and there is no danger of conflicts, 
  consider making a **squash merge** to keep the git history more compact and readable.
  - To enrich your changes, include at least one colleague as a reviewer for your 
  PR. On the other hand, if you're a reviewer, make comments and give tips on how to 
  improve it.

### Pre-commit, using black and flake8 linting when pushing new changes
If you want to have linting before you push new code, you have to `pip install pre-commit` (enable in requirements-dev.txt).
After that, just run `pre-commit install`, with or without `--allow-missing-config`, in terminal in the main directory.
Terminal should return `pre-commit installed at .git/hooks/pre-commit`.
Optionally, you can run against all files by using `pre-commit run --all-files`.
You will get a 'Failed' or 'Passed' status.
Black can be run manually by using `python -m black .`.

If there are appearing weird errors, you can try to `pre-commit autoupdate`.

**Note:** You need to configure a `.pre-commit-config.yaml` before you can use pre-commit. Here you can define all tests and linters you would like to use. For example:

```
repos:
-   repo: https://github.com/psf/black
    rev: 21.9b0
    hooks:
    -   id: black
-   repo: https://gitlab.com/pycqa/flake8
    rev: 3.9.2
    hooks:
    - id: flake8
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.0.1
    hooks:
    -   id: check-added-large-files
    -   id: end-of-file-fixer
    -   id: trailing-whitespace
```

## VERSIONING
For versioning of the packages [Semantic Versioning](https://semver.org/) (SemVer) is
used. Read the specification carefully and take a look at the
[how-to guide](https://github.com/dbrock/semver-howto) to SemVer.

## CODE

### Imports
We tend to follow the [PEP8](https://pep8.org/#imports). In summary:

- Import order: Built-ins, Third-parties, Locals. First the `import ...` then the
  `from ... import ...`

- Prioritize Absolute Imports over Relative Imports (e.g., `import tsaf.utils`)

- Use `as` to reduce the verbosity (e.g., `import tsaf.forecasting.baseline as
  baseline`)

- Prioritize `import ... as ...` for modules and packages

- Avoid `from ... import ...`. But if needed, use it for importing specific functions/classes from a
  module/package

- Leave `__init__.py` empty unless needed (e.g., making a function from a module
  accessible from the package. Even on these cases, prioritize absolute imports)

### Style
[flake8](https://flake8.pycqa.org/en/latest/) is used for style guide checking. Usually,
CI build of the projects runs linting with flake8 and the build will not be succeeded
without corresponding styling. Before committing the code, don't forget to check styling
locally. Various python IDEs also support the integration of flake8. Standard flake8
configuration for this project is as follows:

```
[flake8]
# Matching the black line length (default 88)
max-line-length = 88
max-complexity = 10
extend-ignore =
    # See https://github.com/PyCQA/pycodestyle/issues/373
    E203,
    F811,
exclude =
    .flake8,
    .git,
    .venv*,
    .env*,
    .bkp*,
    __pycache__,
    # Add your folders to ignore here
    build/,
    output/,
    bin/,
    dist/,
```

### Docstrings
In this project reStructuredText (reST) Docstring Format is used, according to proposal 
in [PEP 287](https://www.python.org/dev/peps/pep-0287/). See the example of such 
formatting [here](http://queirozf.com/entries/python-docstrings-reference-examples). **A 
docstring for each function and class are recommended.**

### Readme
Each repository **must** have a README with **at least**:

* Short blurb about the service and what it does
* Describe how to install all development dependencies
* Instructions about how to run the service as well as the tests

Take a look at the READMEs of other services if you need some inspiration!

### Contributing
Each repository **can** have a CONTRIBUTING with the necessary information for new
developers to be able to contribute with the Software.
