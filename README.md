https://github.com/antonputra/tutorials/tree/main/lessons/061/infra

docker pull devopmacaz/mkdocs:v1

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 088789840359.dkr.ecr.us-east-1.amazonaws.com

docker tag devopmacaz/mkdocs:v1 088789840359.dkr.ecr.us-east-1.amazonaws.com/nginx:1.19.8

docker push 088789840359.dkr.ecr.us-east-1.amazonaws.com/nginx:1.19.8


git add -A &&
git commit -m "added kusto and source" &&
git push 

flux reconcile kustomization flux-system --with-source

***This is my original Trust policy for IAM Role: hr-dev-irsa-iam-role***

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::088789840359:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/43950C62DBA1803849E1E2F5F2923DEB"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "oidc.eks.us-east-1.amazonaws.com/id/43950C62DBA1803849E1E2F5F2923DEB:sub": "system:serviceaccount:default:irsa-demo-sa"
                }
            }
        }
    ]
}
```

updated to

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "",
			"Effect": "Allow",
			"Principal": {
				"Federated": "arn:aws:iam::088789840359:oidc-provider/oidc.eks.us-east-1.amazonaws.com/id/43950C62DBA1803849E1E2F5F2923DEB"
			},
			"Action": "sts:AssumeRoleWithWebIdentity",
			"Condition": {
				"StringEquals": {
					"oidc.eks.us-east-1.amazonaws.com/id/43950C62DBA1803849E1E2F5F2923DEB:sub": "system:serviceaccount:flux-system:ecr-credentials-sync"
				}
			}
		}
	]
}
```