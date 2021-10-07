# eks-fluxcd-bootstrap

This repository contains non-production [FluxCD](https://eksctl.io/usage/gitops-v2/) manifests used to bootstrap of Amazon EKS clusters with [eksctl](https://eksctl.io/) using the GitOps approach.

## Prerequisites (CLI)

The following Command Line Interfaces (CLIs) are required to use the assets from this repository:

- eksctl, >= 0.68.0
- awscli, >= 2.2.43
- flux, >= 0.17.2

## Setup

1. Fork this repository using your own GitHub account.

2. Create an [GitHub Personal Access Token](https://github.com/settings/tokens/new?scopes=repo&description=eks-fluxcd-demo) to authenticate against the GitHub APIs, and export it to environment variables:

```bash
export GITHUB_TOKEN=<YOUR GH TOKEN>
export GITHUB_USER=<YOUR GH USERNAME>
```

3. Checkout your forked repository, create a `demo` branch and push to the origin:

```bash
git clone https://github.com/<YOUR GH USERNAME>/eks-fluxcd-bootstrap
cd eks-fluxcd-bootstrap/
git checkout -b demo
git push -u origin demo
```

4. Modify the `owner` on `eksctl-config.yaml` file to use your GitHub username:

```yaml
gitops:
  flux:
    gitProvider: github
    flags:
      owner: <YOUR GH USERNAME>
      repository: eks-fluxcd-bootstrap
      branch: demo
      path: ./clusters/sandbox/
      namespace: flux-system
```

5. Commit the changes into the `demo` branch, and push it to the origin:

```bash
git commit -am 'Change GitOps repository information'
git push -u origin demo
```

6. Create the Amazon EKS cluster with `eksctl`, using the configuration file:

```bash
eksctl create cluster -f helpers/eksctl-config.yaml
```

## Cleanup

1. Delete your Amazon EKS cluster:

```bash
eksctl delete cluster --name sandbox
```

2. Delete the remote and local `demo` branch of your forked repository:

```bash
git checkout main
git push --delete origin demo
git branch -D demo
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

