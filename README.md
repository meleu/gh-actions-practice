# gh-actions-practice

A repo created to learn &amp; practice GitHub Actions

The code here was taken from the [GitHub Actions - The Complete Guide](https://udemy.com/course/github-actions-the-complete-guide) udemy course and is used just for learning GitHub Actions.

## Core concepts

Three main building blocks:

1. workflows
2. jobs
3. steps

Relationship between them:

- **Workflows**: define events + jobs
    - **Jobs**: define runner + steps
        - **Steps**: do the actual work

A little more detailed:

- workflows
    - attached to a git repository
    - triggered upon **events**
    - contain multiple **jobs**
- jobs
    - define a **runner** (execution environment)
    - contain multiple **steps**
    - run in parallel (default) or sequential
    - can be conditional
- steps
    - execute **shell scripts** (`run`) or **actions** (`uses`)
    - executed in order
    - can be conditional


## Workflow basic structure

```yaml
name: <workflow-name>
on: <event>
jobs:
  <job-name>:
    runs-on: ubuntu-latest
    steps:
      - name: <step-name>
        uses: <action-name>  # can be an action
      - name: <step-name>
        run: <shell-command> # can be a shell command
```

---

## Examples

---

### Hello World

- [example](./.github/workflows/hello.yml)
- [logs](https://github.com/meleu/gh-actions-practice/actions/workflows/hello.yml)

Main concepts:

Only trigger the workflow manually
```yaml
# ...
on: dispatch_workflow # <- trigger workflow manually
# ...
```

Defining a runner and executing a shell command:
```yaml
jobs:
  hello-world:
    runs-on: ubuntu-latest
    steps:
      - name: Print greeting
        run: echo "Hello World"
```


---

### Test on push

- [example](./.github/workflows/test.yml)
- [logs](https://github.com/meleu/gh-actions-practice/actions/workflows/test.yml)

Main concepts:
```yaml
on: push # <- run this workflow right after pushing a change

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: get code
    steps:
      - name: get code
        # using the "checkout" action to get the code from the repo
        uses: actions/checkout@v4
      - name: install dependencies
        run: npm ci
      - name: run tests
        run: npm run test
```

We can define two different types of `steps`:

- `run`: to execute actual shell commands
- `uses`: to run a "GitHub Action" (a custom application that performs a, typically complex, frequently repeated task).
    - in this case we are using an action to get the source code from the repo.

---

### Build after test

- [example](https://github.com/meleu/gh-actions-practice/commit/523eb24a2386d27aedd33f2e2367f4f470907863#diff-5c3fa597431eda03ac3339ae6bf7f05e1a50d6fc7333679ec38e21b337cb6721)
- [logs](https://github.com/meleu/gh-actions-practice/actions/workflows/build.yml)

Main concepts:

Triggering the workflow on multiple events

```yaml
on:
  - push
  - dispatch_workflow
```


Defining that a job only runs after another one finishes with success

```yaml
jobs:
  test:
    # ...

  build: # this job runs only after the 'test' job finishes with success
    needs: test
    # ...
```

---

### Only build when push on master

- [example](./.github/workflows/build.yml)
- [logs](https://github.com/meleu/gh-actions-practice/actions/workflows/build.yml)

This is related to **Filters**. Check the [Workflow syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions) doc.

```yaml
# ...
on:
  push:
    branches:
      - master
# ...
```

