https://github.com/antonputra/tutorials/tree/main/lessons/061/infra

docker pull devopmacaz/mkdocs:v1

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 088789840359.dkr.ecr.us-east-1.amazonaws.com

docker tag devopmacaz/mkdocs:v1 088789840359.dkr.ecr.us-east-1.amazonaws.com/nginx:1.19.8

docker push 088789840359.dkr.ecr.us-east-1.amazonaws.com/nginx:1.19.8


git add -A &&
git commit -m "added kusto and source" &&
git push 

flux reconcile kustomization flux-system --with-source
