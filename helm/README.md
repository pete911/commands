# helm

## repos

 - add repo `helm repo add <repo-name> <repo-url` e.g. `helm repo add fluxcd https://charts.fluxcd.io`
 - update repos `helm repo update`
 - list repos `helm repo list`
 - search in repo `helm search repo <repo-name>`

## charts

Values can be overriden with `--values <values-file>.yaml` or `--set <key>=<value>` flag.
 
 - download chart `helm fetch <repo-name>/<chart> --version <chart-version> --untar`
 - validate chart `helm lint <chart>`
 - render chart with default values `helm tempalte <chart> > rendered.yaml`
 - render chart with override values `helm tempalte <chart> --values <values-file>.yaml > rendered.yaml`
 - debug chart (dry-run) `helm install --dry-run --debug --values <values>.yaml <chart-name> <chart-location|<repo-name/chart>>`

## view/rollback/delete releases

 - list all releases in all namespaces `helm list -A`
 - release history `helm history -n <namespace> <release-name>`
 - rollback release `helm rollback -n <namespace> <release-name> [revision]`
 - delete release `helm delete -n <namespace> <release-name>` (can be run with `--dry-run` flag as well)
