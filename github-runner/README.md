# Install ARC Github Runner

Follow the documentation here: https://github.com/actions/actions-runner-controller/blob/master/docs/quickstart.md

## Requirements

- Cert manager configured

## HELM install

1. Add the HELM repo

```bash
helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller
```

2. Update your helm 

```bash
helm repo update
```

3. Install the Github ARC repo

***Note***: You will need a PAT github token with the "Repo" permission checked. For Organization and more, refer to documentation.

```bash
helm upgrade --install --namespace actions-runner-system --create-namespace\
  --set=authSecret.create=true\
  --set=replicaCount=1 \
  --set=authSecret.github_token="REPLACE_YOUR_TOKEN_HERE"\
  --wait actions-runner-controller actions-runner-controller/actions-runner-controller
```

### Tweaking the github controller settings

Find settings here: https://github.com/actions/actions-runner-controller/blob/master/charts/actions-runner-controller/README.md

### Required Private Access Token permissions

https://github.com/actions/actions-runner-controller/blob/master/docs/authenticating-to-the-github-api.md
