# Test Workflow Example

## Build Instructions

```bash
# Build the image
docker build -t quay.io/ambient_code/test-workflow:v1.0.0 .

# Push to registry
docker push quay.io/ambient_code/test-workflow:v1.0.0

# Get digest
docker inspect quay.io/ambient_code/test-workflow:v1.0.0 | jq -r '.[0].RepoDigests[0]'
```

## Register Workflow

```bash
curl -X POST "http://localhost:8080/api/projects/YOUR_PROJECT/workflows" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "test-workflow",
    "imageDigest": "quay.io/ambient_code/test-workflow@sha256:YOUR_DIGEST",
    "graphs": [{"name": "main", "entry": "app.workflow:build_app"}]
  }'
```

## Run Workflow

```bash
curl -X POST "http://localhost:8080/api/projects/YOUR_PROJECT/agentic-sessions" \
  -H "Content-Type: application/json" \
  -d '{
    "workflowRef": {"name": "test-workflow", "graph": "main"},
    "inputs": {"message": "Hello from test!"}
  }'
```

