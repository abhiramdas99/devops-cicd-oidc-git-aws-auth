# devops-cicd-oidc-git-aws-auth


- Why OIDC (OpenID Connect)?
Due to security reasons, we cannot keep the SSH port open to the public. We can only keep it open for whitelisted IP addresses in the AWS security group. However, the issue arises when the CI/CD cannot connect to the EC2 machine.

To address this problem, we are utilizing the OIDC feature.

![image](https://github.com/abhiramdas99/devops-cicd-oidc-git-aws-auth/assets/62290469/81554280-c5be-488a-893f-d3da5f0f1ebd)

## steps 
- create identity provider - IAM >  Identity providers
- Create identity provider > Configure provider
  provider type : OpenId connect
  Provider url : "https://token.actions.githubusercontent.com"   and click on [Get thumbprint]
  Audience : "sts.amazonaws.com" and click [create]

