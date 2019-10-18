# flux-get-started

We published a step-by-step run-through on how to use Flux and Helm Operator [over
here](https://github.com/fluxcd/flux/blob/master/docs/tutorials/get-started-helm.md).

###

K8s cluster setup on Docker Desktop for Mac

```
# References
# https://docs.fluxcd.io/en/latest/tutorials/get-started.html
# https://github.com/fluxcd/helm-operator/blob/master/chart/helm-operator/README.md

kubectl -n kube-system create sa tiller

kubectl create clusterrolebinding tiller-cluster-rule \
    --clusterrole=cluster-admin \
    --serviceaccount=kube-system:tiller

helm init --skip-refresh --upgrade --service-account tiller --history-max 10 --wait

helm repo add fluxcd https://charts.fluxcd.io

helm repo update

kubectl apply -f https://raw.githubusercontent.com/fluxcd/flux/helm-0.10.1/deploy-helm/flux-helm-release-crd.yaml

kubectl create ns fluxcd

helm install --name flux --namespace fluxcd --wait fluxcd/flux \
  --set git.email="ashevtsov@monetate.com" \
  --set git.url=git@github.com:ashevtsov-mt/flux-get-started \
  --set git.user="Flux" \
  --set helmOperator.create=true \
  --set helmOperator.createCRD=false

kubectl -n fluxcd rollout status deployment/flux

fluxctl identity --k8s-fwd-ns fluxcd
```

### Helm releases

Elasticsearch
* Source: Helm repository (elastic.co)
* Kubernetes deployment
* automated image updates

Kibana
* Source: Helm repository (elastic.co)
* Kubernetes deployment
* automated image updates

Fluentd
* Source: Helm repository (stable)
* Kubernetes deployment
* automated image updates

## <a name="help"></a>Getting Help

If you have any questions about, feedback for or problems with `flux-get-started`:

- Invite yourself to the <a href="https://slack.cncf.io" target="_blank">CNCF community</a>
  slack and ask a question on the [#flux](https://cloud-native.slack.com/messages/flux/)
  channel.
- To be part of the conversation about Flux's development, join the
  [flux-dev mailing list](https://lists.cncf.io/g/cncf-flux-dev).
- [File an issue.](https://github.com/fluxcd/flux/issues/new)

Your feedback is always welcome!
