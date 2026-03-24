Task 1: Prepare
🔹 Minimal Dockerfile (if you don’t have one)
# Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html

 Place it in your repo root.

 Task 2 & 3: Build + Push Image

Create:

.github/workflows/docker-publish.yml
name: Docker Build & Push

on:
  push:
    branches:
      - main
      - feature/*   # allow testing builds on feature branches

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set short SHA
        id: vars
        run: echo "SHA_SHORT=$(echo $GITHUB_SHA | cut -c1-7)" >> $GITHUB_ENV

      - name: Build Docker image
        run: |
          docker build -t ${{ secrets.DOCKER_USERNAME }}/pr-demo:latest .

      - name: Tag image with SHA
        run: |
          docker tag ${{ secrets.DOCKER_USERNAME }}/pr-demo:latest \
          ${{ secrets.DOCKER_USERNAME }}/pr-demo:sha-${{ env.SHA_SHORT }}

      - name: Login to Docker Hub
        if: github.ref == 'refs/heads/main'
        run: echo "${{ secrets.DOCKER_TOKEN }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Push latest
        if: github.ref == 'refs/heads/main'
        run: docker push ${{ secrets.DOCKER_USERNAME }}/pr-demo:latest

      - name: Push SHA tag
        if: github.ref == 'refs/heads/main'
        run: docker push ${{ secrets.DOCKER_USERNAME }}/pr-demo:sha-${{ env.SHA_SHORT }}
 Verify
🔹 Build logs
Go to Actions tab
Check:
Successfully built ...
🔹 Docker Hub

Go to your repo:

 https://hub.docker.com/r/<your-username>/pr-demo

You should see:

latest
sha-xxxxx
 Task 4: Only Push on Main

Already handled here 

if: github.ref == 'refs/heads/main'
 Test It
Push to feature branch:
git checkout -b feature/test-docker
git push origin feature/test-docker

 Result:

Image builds
 Image NOT pushed
 Task 5: Add Status Badge
🔹 Get Badge
Go to Actions tab
Open workflow → click ... → Create status badge
🔹 Add to README.md
![Docker Build](https://github.com/<username>/<repo>/actions/workflows/docker-publish.yml/badge.svg)

 Commit & push → badge turns green 

Task 6: Pull & Run
🔹 Pull Image
docker pull <your-username>/pr-demo:latest
🔹 Run Container
docker run -d -p 8080:80 <your-username>/pr-demo:latest
🔹 Verify

Open browser:

http://localhost:8080

👉 Your app should load ✅
