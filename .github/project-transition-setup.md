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

- push to `master`
- push to `main`
- push to `develop`
- issue closed (`issues.closed`)
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

## 5) Parent Requirement Auto-Close (Sub-issues)

When a sub-issue (bug) status changes by commit command:

- parent issue project status follows child status for:
  - `策划完成`
  - `研发完成`
  - `需求验收完成`
  - `测试中`
  - `已拒绝`
- for `已实现`: only when **all** sub-issues are already in `已实现`, parent status moves to `已实现`.

When a bug issue is closed and it is a sub-issue of a requirement issue:

- if parent still has open sub-issues: do nothing
- if all sub-issues are closed:
  - parent issue will be auto-closed
  - parent project status will be updated to `已实现`

One commit can include multiple commands, e.g.:

`start #12 fix #11`

## 6) Test Steps

1. Create one requirement issue (parent), e.g. `#100`.
2. Create two bug issues, e.g. `#101` and `#102`.
3. In issue `#100`, add `#101` and `#102` as sub-issues.
4. Ensure issue `#100` is added into your GitHub Project v2.
5. Close `#101`: parent should remain open.
6. Close `#102`: parent `#100` should auto-close and project status should become `已实现`.
