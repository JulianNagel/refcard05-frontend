# TODO – Frontend (für Mitwirkende)

Dieses Repo ist ein **Template**. Pipeline, Dockerfile, nginx-Config und Doku-Gerüst sind fertig – folgendes fehlt noch. Bitte erledigte Punkte abhaken (`[x]`) und mitcommitten.

Docker Compose für das Gesamtsystem und die AWS-Doku (RDS, S3, Learner-Lab-Secrets, OIDC-Vergleich) liegen im Backend-Repo.

## 1. Code einbringen

- [ ] React-+-Vite-Projekt einfügen (Vorgabe: [refcard-03-fe-react-vite v1-start](https://github.com/bbwlc/refcard-03-fe-react-vite/tree/v1-start))
- [ ] `package.json` + `package-lock.json` liegen im Repo-Root (sonst überspringt die Pipeline den Build; `npm ci` braucht das Lockfile)
- [ ] Bestehende `Dockerfile`, `nginx.conf`, `.gitignore` **nicht überschreiben**
- [ ] Build-Output landet in `dist/` (Vite-Standard – das Dockerfile kopiert von dort)

## 2. Backend-URL

- [ ] Alle API-Aufrufe nutzen `import.meta.env.VITE_API_BASE_URL` (keine hartcodierten URLs)
- [ ] Lokal: `cp .env.example .env`

## 3. Tests

- [ ] Mindestens ein Test, `npm test` läuft ohne Watch-Modus durch (z. B. Vitest: `"test": "vitest run"`)
- [ ] Optional `npm run lint` – wird ausgeführt, falls vorhanden

## 4. AWS einrichten (Learner Lab, `us-east-1`)

- [ ] ECR-Repository `refcard05-frontend`
- [ ] Task-Definition aus `aws/task-definition.example.json` registrieren (alle `REPLACE_*` ersetzen)
- [ ] ECS-Service `refcard05-frontend-service` im gleichen Cluster wie das Backend (Port 80 offen)

## 5. GitHub konfigurieren (Settings → Secrets and variables → Actions)

Secrets (Learner Lab → *AWS Details* → *AWS CLI*; laufen pro Session ab!):
- [ ] `AWS_ACCESS_KEY_ID`
- [ ] `AWS_SECRET_ACCESS_KEY`
- [ ] `AWS_SESSION_TOKEN`

Variables:
- [ ] `VITE_API_BASE_URL` = URL des Backend-Service (ohne gesetzte Variable bricht der Image-Build bewusst ab)
- [ ] `AWS_REGION` = `us-east-1`
- [ ] `ECR_REPOSITORY` = `refcard05-frontend`
- [ ] `ECS_CLUSTER`
- [ ] `ECS_SERVICE` (solange leer, wird nicht deployt)
- [ ] `ECS_TASK_FAMILY` = `refcard05-frontend`
- [ ] `CONTAINER_NAME` = `frontend`

## 6. Release & Redeploy prüfen

- [ ] Push auf `main` → Workflow grün, Image `latest` + `sha-…` in ECR
- [ ] Ersten Release taggen: `git tag v1.0.0 && git push origin v1.0.0` → Image `1.0.0` in ECR
- [ ] ECS startet automatisch einen neuen Task, `/health` antwortet mit `ok`
- [ ] Frontend im Browser über die öffentliche IP/URL erreichbar und lädt Daten vom Backend

## 7. Dokumentation (README.md)

- [ ] Screenshots unter `docs/screenshots/` ablegen und im README verlinken: Commits/Tags, Workflow-Lauf, ECR, ECS
- [ ] Checkliste "Nachweise" im README abhaken

## Regeln

- Keine Zugangsdaten oder `.env`-Dateien committen
- Kleine, aussagekräftige Commits (zählt für "Nachvollziehbarkeit")
- Versionen nur über Git-Tags `vMAJOR.MINOR.PATCH`, gleiches Schema wie im Backend
