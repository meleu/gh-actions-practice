# gh-actions-practice

A repo created to learn &amp; practice GitHub Actions

The code here was taken from the [GitHub Actions - The Complete Guide](https://udemy.com/course/github-actions-the-complete-guide) udemy course and is used just for learning GitHub Actions.

## GitHub Workflow basic structure

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

## Hello World

The [hello.yml](./.github/workflows/hello.yml) is a workflow that is only triggered manually.

Check it [here in the Actions tab](https://github.com/meleu/gh-actions-practice/actions/workflows/hello.yml).

Main concept:
```yaml
# ...
on: dispatch_workflow # <- trigger workflow manually
# ...
```


## Test on push

The [test.yml](./.github/workflows/test.yml) is an example of workflow created
to run `npm run test` when a new push happens.

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


