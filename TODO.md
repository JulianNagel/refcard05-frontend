# TODO – Frontend

Offene Punkte für Setup & Konfiguration (Code nicht aufgeführt).

## AWS (Learner Lab, `us-east-1`)

- [ ] ECR-Repository `refcard05-frontend`
- [ ] Task-Definition aus `aws/task-definition.example.json` registrieren (`REPLACE_*` ersetzen)
- [ ] ECS-Service `refcard05-frontend-service` im Backend-Cluster, Port 80 offen

## GitHub Secrets

Aus Learner Lab → *AWS Details* → *AWS CLI*. **Laufen pro Session ab – bei jeder neuen Session aktualisieren.**

- [ ] `AWS_ACCESS_KEY_ID`
- [ ] `AWS_SECRET_ACCESS_KEY`
- [ ] `AWS_SESSION_TOKEN`

## GitHub Variables

- [ ] `VITE_API_BASE_URL` = URL des Backend-Service (fehlt sie, bricht der Image-Build ab)
- [ ] `AWS_REGION` = `us-east-1`
- [ ] `ECR_REPOSITORY` = `refcard05-frontend`
- [ ] `ECS_CLUSTER`
- [ ] `ECS_SERVICE` (leer → kein Deploy)
- [ ] `ECS_TASK_FAMILY` = `refcard05-frontend`
- [ ] `CONTAINER_NAME` = `frontend`

## Template erwartet

- API-Aufrufe über `import.meta.env.VITE_API_BASE_URL`
- Build-Output in `dist/`, `package-lock.json` vorhanden

## Abschluss

- [ ] Release taggen (`v1.0.0`) → Image in ECR, ECS-Task neu gestartet
- [ ] Screenshots (Commits, Workflow, ECR, ECS) ins README
