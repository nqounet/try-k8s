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

## 4.5.12
- 少なくともローカル環境では top コマンドを実行するとエラーが発生する
- 解消するには metrics-server を使う必要があるようだ
    - `kubectl top --help` を実行すると、以下のようなコメントが出る
    - `This command requires Metrics Server to be correctly configured and working on the server.`
    - [kubernetes-sigs/metrics-server: Cluster-wide aggregator of resource usage data.](https://github.com/kubernetes-sigs/metrics-server)
        - 残念ながら、そのままでは動作しない
        - SSL証明書やIPアドレスを解決する必要がありそうだが、今の所断念
    - 後で調べてみる
    - [Kubernetesクラスタでmetrics-serverを導入してkubectl topやHPA(Horizontal Pod Autoscaler)を有効にする - Qiita](https://qiita.com/chataro0/items/28f8744e2781f730a0e6)

## 動作環境
### 結論
- `Docker Desktop` を `enable Kubernetes` して使用することにした
    - Dashboard は便利
    - メニューから Kubernetes の context を切り替えることができる

### 試行錯誤のログ
- macos に microk8s をインストールして使用したかったがイマイチだった（dockerの影響かも？）
- 一旦アンインストールし multipass を使用することにした
    - [Multipass orchestrates virtual Ubuntu instances](https://multipass.run/)
- multipass で起動した仮想マシンを docker コマンドでも使用できるように設定したかったが、マウントが解決できなかった
    - [multipassでつくるmacOS向けのDockerd & K8s環境 - Qiita](https://qiita.com/mumoshu/items/6ff56badcfabe5ab1f49)
- multipass で起動したマシンにログインして、各種コマンドを実行することにした
    - primary は ~/Home にホストの ~ をマウントする
    - path が異なるため（？）か、 `git status` が非常に遅くなる
    - zsh で情報を取得していることもあり、ホストとゲストを行ったり来たりすると耐え難い遅さ
- 紆余曲折はしたが、最終的には `Docker Desktop` を使用することにした
    - 動作の遅さを解決する場合は、仮想マシン内で操作を完結する必要がありそう
        - この場合、手軽に仮想マシン自体を作ったり削除したりする機能は不要になる
    - 仮想マシン内で完結させる場合、 ssh コマンドによるログインが中心になる
        - ターミナルからは `multipass shell` が使用できるが、エディタからは使用できない
