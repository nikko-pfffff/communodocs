Afin de faire des tests, on peut directement récupérer les credentials AWS d'un rôle via la CLI AWS et les exporter dans des variables d'environnement.

```bash
export AWS_PROFILE=<mon_profile_aws>
export $(printf "AWS_ACCESS_KEY_ID=%s AWS_SECRET_ACCESS_KEY=%s AWS_SESSION_TOKEN=%s" $(aws --profile <mon_profile_source> sts assume-role --role-arn arn:aws:sts::<account_id>:role/<mon_role> --role-session-name <ma_session> --query "Credentials.[AccessKeyId,SecretAccessKey,SessionToken]" --output text))

env|grep AWS
```

Listing des inférences profiles
```bash
aws --region <ma_region>--profile <mon_profile_aws> bedrock list-inference-profiles --type-equals APPLICATION
```

<br>
Dans le code Python

[...]