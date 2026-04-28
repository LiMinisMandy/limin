# Project Transition Setup (GitHub Projects v2)

## 1) Add Repository Variable(s)

Go to `Settings -> Secrets and variables -> Actions -> Variables`:

- `PROJECT_NUMBER`: GitHub Project v2 number, e.g. `3`
- `PROJECT_OWNER`: project owner login, e.g. `my-org` (optional; default is repo owner)
- `PROJECT_TITLE`: project title for disambiguation when an issue belongs to multiple projects (optional)
- `STATUS_FIELD_NAME`: status field name in project, default is `Status` (optional)
- `COMMENT_ON_TRANSITION`: `true` or `false` (optional, default `false`)

`PROJECT_NUMBER` is now optional. If not provided, workflow will auto-pick the project from the issue's existing project items when there is exactly one match.

## 2) Add Secret

Go to `Settings -> Secrets and variables -> Actions -> Secrets`:

- `PROJECT_TOKEN`: a PAT that can update your Project v2 items.

Suggested PAT scopes:

- `project` (required for project item updates)
- `repo` (recommended when repository access is private or comment writing is needed)

## 3) Trigger

Workflow file: `.github/workflows/project-transition.yml`

It triggers on:

- push to `main`
- push to `develop`
- manual run (`workflow_dispatch`)

## 4) Commit Message Format

Supported commands:

- `plan #16`

Current built-in transition in workflow:

- `plan -> 策划完成`

One commit can include multiple commands, e.g.:

`start #12 fix #11`
