# Project Transition Setup (GitHub Projects v2)

## 1) Add Repository Variable(s)

Go to `Settings -> Secrets and variables -> Actions -> Variables`:

- `PROJECT_NUMBER`: GitHub Project v2 number, e.g. `1` (optional; default is `1`)
- `PROJECT_OWNER`: project owner login, e.g. `my-org` (optional; default is repo owner)
- `STATUS_FIELD_NAME`: status field name in project, default is `Status` (optional)
- `COMMENT_ON_TRANSITION`: `true` or `false` (optional, default `false`)

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
- `rd_done #16`
- `req_accept_done #16`
- `testing #16`
- `implemented #16`
- `rejected #16`

Current built-in transition in workflow:

- `plan -> 策划完成`
- `rd_done -> 研发完成`
- `req_accept_done -> 需求验收完成`
- `testing -> 测试中`
- `implemented -> 已实现`
- `rejected -> 已拒绝`

One commit can include multiple commands, e.g.:

`start #12 fix #11`
