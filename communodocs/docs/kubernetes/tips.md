### Lister tous les objets dans un namespace (exhaustif)

Le `kubectl get all` ne liste pas tous les objets dans un namespace, il manque entre autres les ConfigMaps, Secrets, PersistentVolumeClaims, etc.

Du coup itérer sur tous les types de ressources disponibles dans le cluster permet d'avoir une liste exhaustive des objets dans un namespace donné :

```bash
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --ignore-not-found --show-kind
```