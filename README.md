# helm-files-get
A project see how helm mounts local file content in a pod.

The chart templates will create a secret with content of a local file `./assets/realm.json`.
It will use that secret to add a pod volumeMount at `/tmp/realm.json`.

The intent of this project is to see if that local file can be overridden by using the chart remotely and without modifying the repo file itself.

# Playing with local chart

## Helm chart validation
```
helm lint ./chart
helm template --debug ./chart
```

## Helm deploy
```
helm install app ./chart
```

## Confirm file is mounted
```
k exec -it myapp -- /bin/bash
cat /tmp/realm.json
```

# Publishing chart to use "remotely"

Package and publish chart
```
helm package ./chart
mv helm-file-get-0.1.0.tgz docs
helm repo index docs --url https://thecodebeneath.github.io/helm-files-get
git add .
git commit -m "New chart index"
git push
```

Add a helm repo and use it
```
helm repo add codebeneath https://thecodebeneath.github.io/helm-files-get
helm install app2 codebeneath/helm-file-get
```
