# RefCard05 – Frontend (React + Vite)

Frontend der RefCard05 Full-Stack-Anwendung auf AWS: React + Vite, ausgeliefert als nginx-Container über Amazon ECR und Amazon ECS (Fargate).

Backend-Repo: [refcard05-backend](../../../refcard05-backend) – dort liegen auch Docker Compose für die lokale Entwicklung und die Doku zu RDS/S3.

> **Template:** Der React-Code (Vorgabe: [refcard-03-fe-react-vite v1-start](https://github.com/bbwlc/refcard-03-fe-react-vite/tree/v1-start)) wird noch hinzugefügt. Solange kein `package.json` existiert, überspringt die Pipeline Build und Deploy.

## Struktur

| Pfad | Zweck |
|---|---|
| `.github/workflows/ci-cd.yml` | Pipeline: build → test → Image nach ECR → ECS-Redeploy |
| `Dockerfile` | Multi-Stage: Vite-Build → nginx |
| `nginx.conf` | SPA-Fallback, Asset-Caching, `/health`-Endpoint |
| `aws/task-definition.example.json` | Beispiel-Task-Definition (Fargate, Healthcheck) |
| `.env.example` | `VITE_API_BASE_URL` für lokale Entwicklung |

## CI/CD-Pipeline

1. **check** – überspringt alles, solange noch kein Code im Repo ist
2. **build-test** – `npm ci`, Lint, Tests, `npm run build` (npm-Cache)
3. **image** – bricht mit klarer Meldung ab, falls `VITE_API_BASE_URL` fehlt; Build & Push nach ECR
4. **deploy** – aktuelle Task-Definition holen, neues Image eintragen, Service updaten, auf Stabilität warten (→ automatisches Redeploy)

Pull Requests laufen nur bis *build-test*.

### GitHub Secrets

| Secret | Quelle |
|---|---|
| `AWS_ACCESS_KEY_ID` | Learner Lab → *AWS Details* → *AWS CLI* |
| `AWS_SECRET_ACCESS_KEY` | dito |
| `AWS_SESSION_TOKEN` | dito |

### GitHub Variables

| Variable | Beispiel |
|---|---|
| `VITE_API_BASE_URL` | URL des Backend-Service, z. B. `http://<backend-ip-oder-alb>:8080` |
| `AWS_REGION` | `us-east-1` |
| `ECR_REPOSITORY` | `refcard05-frontend` |
| `ECS_CLUSTER` | `refcard05-cluster` |
| `ECS_SERVICE` | `refcard05-frontend-service` (leer → kein Deploy) |
| `ECS_TASK_FAMILY` | `refcard05-frontend` |
| `CONTAINER_NAME` | `frontend` |

`VITE_API_BASE_URL` wird beim Build ins Bundle eingebaut. Ändert sich die Backend-URL, Variable anpassen und Workflow neu starten.

## Versionierung (Git-Tags → ECR)

```bash
git tag v1.0.0 && git push origin v1.0.0
```

| Ereignis | Image-Tags in ECR |
|---|---|
| Push auf `main` | `latest`, `sha-<7 Zeichen>` |
| Tag `v1.2.3` | `1.2.3`, `1.2`, `sha-<7 Zeichen>` |

Gleiches Schema wie im Backend-Repo.

## ECS-Deployment

1. ECR-Repository `refcard05-frontend` erstellen
2. Task-Definition aus `aws/task-definition.example.json` registrieren (Platzhalter ersetzen)
3. Service im gleichen Cluster wie das Backend erstellen (1 Task, Port 80)
4. Ab dann deployt jeder Push auf `main` / jeder Tag automatisch

## Learner-Lab-Zugangsdaten

Die Zugangsdaten laufen mit der Lab-Session ab. Bei jeder neuen Session die drei Secrets in **beiden** Repos aktualisieren (Details und OIDC-Vergleich im Backend-README).

## Lokale Entwicklung

Nur Frontend:

```bash
cp .env.example .env
npm install
npm run dev
```

Gesamtes System (Frontend + Backend + DB): siehe `compose/` im Backend-Repo.

## Nachweise (Screenshots)

<!-- Screenshots unter docs/screenshots/ ablegen und hier verlinken -->

- [ ] Commits / Git-Tags
- [ ] Erfolgreicher Workflow-Lauf
- [ ] ECR-Repository mit getaggten Images
- [ ] ECS-Service (laufender Task, Deployments)
