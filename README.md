# k8s-minikube-deploytraining
https://developer.mamezou-tech.com/containers/k8s/tutorial/app/minikube/
を参考にminikubeをつかったローカル開発環境の構築手順とデプロイを練習

# 実行環境
ローカルPC：Windows11

DockerDesktop 4.18.0

 WSLターミナルよりminikube起動
```
minikube start
```

# minikubeにデプロイ
## dockerコマンドがminikubeの仮想環境で実行されるようにする
WSLターミナルより以下実行
```
eval $(minikube docker-env)
```
## コンテナイメージを作成する
```
docker build -t sample-app .
```
実行結果を確認
```
docker images | grep sample-app
```
実行結果はこんな感じ
```
watyanabe164@DESKTOP-80PP3CS:/mnt/c/Users/user/Documents/work/k8s-minikube-deploytraining$ docker images | grep sample-app
sample-app                                           latest    b10558bb12d2   50 seconds ago   6.12MB
watyanabe164@DESKTOP-80PP3CS:/mnt/c/Users/user/Documents/work/k8s-minikube-deploytraining$
```

## minikube上でpodをデプロイ
```
kubectl apply -f app.yml
```
結果を確認
```
kubectl get pod
```
実行結果はこんな感じ
```
watyanabe164@DESKTOP-80PP3CS:/mnt/c/Users/user/Documents/work/k8s-minikube-deploytraining$ kubectl get pod
NAME                         READY   STATUS    RESTARTS   AGE
sample-app-9b785cb58-j6c7w   1/1     Running   0          9s
```
## ブラウザでデプロイ結果を確認
```
minikube service sample-app --url
```
出力結果はこちら
```
watyanabe164@DESKTOP-80PP3CS:/mnt/c/Users/user/Documents/work/k8s-minikube-deploytraining$ minikube service sample-app --url
😿  service default/sample-app has no node port
http://127.0.0.1:42773
❗  Because you are using a Docker driver on linux, the terminal needs to be open to run it.
```
↑↑で出力されてる、↓↓のURLをローカルＰＣのWebブラウザで実行すれば画面表示される
http://127.0.0.1:42773

※参考サイトでは、ingressアドオンを追加したりdnsアドオンを使ってブラウザからアクセスする方法を説明してたけど…
いろいろとやってうまくいかなかったので割愛。

# (オマケ)minikubeの実行環境のサイズ変更
```
# minikubeを起動するDriver。HyperKit以外のドライバーの場合は設定値を変更してください
minikube config set driver hyperkit
# minikubeに割り当てるCPU。デフォルトは2。マシンスペックに応じて変更してください
minikube config set cpus 4
# minikubeに割り当てるメモリ。デフォルトは2G。マシンスペックに応じて変更してください
minikube config set memory 8Gi
```

