# Kubernetes 完全ガイド

## 動作環境
- macos に microk8s をインストールして使用したかったがイマイチだった（dockerの影響かも？）
- 一旦アンインストールし multipass を使用することにした
    - [Multipass orchestrates virtual Ubuntu instances](https://multipass.run/)
- multipass で起動した仮想マシンを docker コマンドでも使用できるように設定したかったが、マウントが解決できなかった
    - [multipassでつくるmacOS向けのDockerd & K8s環境 - Qiita](https://qiita.com/mumoshu/items/6ff56badcfabe5ab1f49)
- multipass で起動したマシンにログインして、各種コマンドを実行することにした
    - primary は ~/Home にホストの ~ をマウントする
    - path が異なるため（？）か、 `git status` が非常に遅くなる
    - zsh で情報を取得していることもあり、ホストとゲストを行ったり来たりすると耐え難い遅さ

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
