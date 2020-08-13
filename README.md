# Kubernetes 完全ガイド

## 4.5.3
- create よりも apply を使う
    - apply はリソースの更新を行うコマンドだが、リソースが存在しない場合は作成を行う

```
$ kubectl apply -f sample-pod.yaml
```

