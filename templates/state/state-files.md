# State file templates

All state files hold references only — never code or long text.

`current-session.json`
```json
{ "startedAt": "", "command": "", "artifact": "", "stage": "detect|memory|discover|context|spec|plan|implement|test|review|improve|evolve" }
```

`active-context.json`
```json
{ "artifact": "", "contextFile": "", "files": [], "domains": [] }
```

`current-spec.json`
```json
{ "artifact": "", "spec": "prompt-spec.md", "blockingQuestions": false }
```

`current-plan.json`
```json
{
  "artifact": "",
  "strategy": "",
  "milestones": [],
  "tasks": [{ "id": "T1", "title": "", "status": "pending|in_progress|done|blocked", "dependsOn": [], "files": [], "requiredSkills": [] }],
  "nextTask": ""
}
```

`progress.json`
```json
{ "artifact": "", "tasksDone": 0, "tasksTotal": 0, "lastUpdated": "" }
```

`changed-files.json`
```json
{ "artifact": "", "files": [{ "path": "", "change": "added|modified|deleted", "task": "T1" }] }
```

`selected-domains.json` — verbatim output of f67-domain-detector.

`execution-history.json`
```json
{ "lastSyncCommit": "", "workflows": [{ "artifact": "", "completedAt": "", "memorySynced": true }] }
```
