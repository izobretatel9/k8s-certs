# k8s-certs


1)  Update requires new certificate files with extension ``.crt и .key``

2)  Run the command (the machine must have kubernetes installed, for example minikube):

```
kubectl create --dry-run='client' -o=yaml secret tls myname --cert=<Path to file .crt> --key=<Path to file .key>
```

3) Run commands (werf must be installed):
```
export WERF_SECRET_KEY=
```
4) Downloading the repository. Let's go and do it
```
werf helm secret values edit .helm/secret-values.yaml
```
5) Inserting values ``.crt и .key`` and save
6) git commit and push