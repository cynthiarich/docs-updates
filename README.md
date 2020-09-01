# GitHub Action: Docs Updates 

<p align="center">
  <a href="https://github.com/cynthiarich/docs-updates/actions"><img alt="javscript-action status" src="https://github.com/cynthiarich/docs-updates/workflows/units-test/badge.svg"></a>
</p>
 
 Never forget to update your docs again! The Docs Updates action helps you detect changes in the folders and files you designate and opens an issue to remind you to update the documentation. You can assign the issue to a specific writer or team.

## Using this action

To use this action in your project, you'll need to complete 2 steps:

### Step 1: Create the docs-config.yml

The config is used to designate areas of responsibility within the product. When a pull request touches a file in one of the paths, a documentation issue will be opened and assigned to an individual or team as indicated in the YAML. 

**Example `docs-config.yml` assigning individuals**

```yaml
group: 'Info only'
 path: 'README.md'
 assignee: 'aperson'

group: 'App function'
 path: ['index.js', 'app/*']
 assignee: 'anotherperson'
```

**Example `docs-config.yml` using a first responder model**

```yaml
group: 'All'
 path: '*'
 assignee: 'org/docs-first-responder'
```

### Step 2: Create the workflow file

Next, you'll need to add a file to the project repository's `.github/workflows` directory:

```yaml
name: "docs-updates"
on:
 pull_request:
 push:
  branches:
    - main
    - 'releases/*'
  paths-ignore:
    - 'docs/*'

jobs:
 docs:
  runs-on: ubuntu-latest
  steps:
  - uses: cynthiarich/docs-updates@main
    with:
      config: 'docs-config.yml'
```

**Optional** You can allow your developers to skip this workflow if they know documentation is not needed by adding a label to the pull request when it is opened:

```yaml
name: "docs-updates"
on:
 pull_request:
 push:
  branches:
    - main
    - 'releases/*'
  paths-ignore:
    - 'docs/*'

jobs:
 docs:
  if: ${{ !contains(github.event.pull_request.labels.*.name, 'no-docs') }}
  runs-on: ubuntu-latest
  steps:
  - uses: cynthiarich/docs-updates@main
    with:
      config: 'docs-config.yml'
```

**Optional:** You can also add the newly opened issue to a project board:

```yaml
show example here
```

See the [actions tab](https://github.com/cynthiarich/docs-updates/actions) for runs of this action! :rocket:
