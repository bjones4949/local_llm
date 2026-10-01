# local-llm

vLLM (inference) + AnythingLLM (UI/RAG) + OneDrive sync, via Docker Compose.

## Host prerequisites
- Docker Engine + Compose v2
- NVIDIA driver + NVIDIA Container Toolkit (`docker run --rm --gpus all nvidia/cuda:12.4.0-base-ubuntu22.04 nvidia-smi` should work)

## First deploy
```bash
git clone <repo-url> local-llm && cd local-llm
cp .env.example .env && nano .env          # set secrets, model, paths

# OneDrive data dir must exist and be owned by PUID:PGID
sudo mkdir -p /srv/onedrive && sudo chown 1000:1000 /srv/onedrive

# One-time OneDrive auth (interactive: open the URL, paste the response URI back)
docker compose run --rm -it onedrive

docker compose pull
docker compose up -d
docker compose logs -f vllm               # watch the model load
```
AnythingLLM: http://<host>:3001

## Update
```bash
git pull
docker compose pull
docker compose up -d
```

## Rollback
Tag known-good states (`git tag v1.0 && git push --tags`), then on the host:
```bash
git fetch --tags && git checkout v1.0 && docker compose up -d
```

## Notes
- Secrets live only in `.env` on the host. Model weights, AnythingLLM data and the OneDrive token live in Docker volumes.
- Pin image tags in `.env` once things are stable so `docker compose pull` doesn't surprise you.
- OneDrive files are mounted read-only into AnythingLLM at `/app/onedrive`. AnythingLLM does not auto-ingest that folder; add docs via the UI or the AnythingLLM API.