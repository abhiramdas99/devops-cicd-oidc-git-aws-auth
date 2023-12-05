# devops-cicd-oidc-git-aws-auth


- Why OIDC (OpenID Connect)?
Due to security reasons, we cannot keep the SSH port open to the public. We can only keep it open for whitelisted IP addresses in the AWS security group. However, the issue arises when the CI/CD cannot connect to the EC2 machine.

To address this problem, we are utilizing the OIDC feature.

![image](https://github.com/abhiramdas99/devops-cicd-oidc-git-aws-auth/assets/62290469/81554280-c5be-488a-893f-d3da5f0f1ebd)

## steps 
- create identity provider - IAM >  Identity providers
- Create identity provider > Configure provider
```git
  provider type : OpenId connect
  Provider url : "https://token.actions.githubusercontent.com"   and click on [Get thumbprint]
  Audience : "sts.amazonaws.com" and click [create]
```
- create a role
```git
Trusted entity type:
  select trusted entity : web identity

Web identity:
  Identy provider : https://token.actions.githubusercontent.com
  Audience : sts.amazonaws.com
[Next]
Add Permissions:
Add admin policy which you want to give access to Github Actions. For now I have added "AdministratorAccess"

Trust relationships:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "arn:aws:iam::**********:oidc-provider/token.actions.githubusercontent.com"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
                },
                "StringLike": {
                    "token.actions.githubusercontent.com:sub": [
                        "repo:Ma*************n/s*******r-backend:*",
                        "repo:Ma*************n/s*******r-frontend:*"
                    ]
                }
            }
        }
    ]
}
```
  
