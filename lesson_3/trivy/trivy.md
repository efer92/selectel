docker run --rm -v $(pwd):/project aquasec/trivy config /project/Dockerfile
docker run --rm aquasec/trivy image <имя_образа>
