# Kubernetes 完全ガイド

## 4.5.3
- create よりも apply を使う
    - apply はリソースの更新を行うコマンドだが、リソースが存在しない場合は作成を行う

```
$ kubectl apply -f sample-pod.yaml
```

## 4.5.5
- マニフェストの指定は、ファイルだけでなくディレクトリでも可能
    - オプション `-R` で再帰的に適用される
    - 適用順序はディレクトリ名も含めたファイル名の順

```
$ kubectl apply -f . -R
```
```
# 適用される順序
1/pod.yaml
pod.yaml
z/pod.yaml
```
