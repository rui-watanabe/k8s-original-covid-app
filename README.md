# Description
「COVID LIVE DASHBORD」
海外と日本のコロナ感染状況をリアルタイムで取得し、データをグラフ化表示したWebアプリケーション。

# DEMO
![demo](https://gyazo.com/764f9a5fd630c302992a0d80d757f331/raw)

# URL
https://original-covid-app.infra-structure.work/

# Kubernetes
- Kubernetesの概要<br>
コンテナ環境でマイクロサービスを実現するためにKubernetesを採用<br>
マスターノード(CentOS7)とワーカーノード(CentOS7)をkubeadmで1台ずつセットし、Kubernetesクラスター環境を作成。

- Deployment<br>
Podを管理するDeploymentで、Pod数の指定やコンテナレジストリーへのアクセスを定義している。

- Service<br>
L4ロードバランサーのServiceで、各マイクロサービスのPodをService名で取得できるようしている。

- Ingress<br>
L7ロードバランサーのingressで外部公開を行い、パスに振り分けられたServiceに接続できるようにしている。

- <br>
![demo](https://gyazo.com/7b4ef626d00a809c10fe54f8a8518334/raw)

# Micro Frontendsとバックエンド
- Micro Frontendsの概要<br>
フロントエンド側もマイクロサービス化させるMicro Frontendsを採用<br>
JavaScriptとCSSをWebpackでモジュールバンドルすることで、各Componentの表示をAPIとして提供している。

- Container<br>
「World Component」と「Japan Prefecture Component」を読み込みフロントエンド（React,TypeScript）<br>
「Container」で表示を行なっているのは遷移ボタンのみで、各コンポーネントを読み込むことによって他の表示を行なっている。<br>
「Container」のルーティング設定では、下記のように行なっている<br>
<br>
wp
|Container React Path|Ingress Path|Description|
|:---|:---|:---|
|/|/api/frontend/world|「World Component」を提供するAPIを取得|
|/prefecutre|/api/frontend/japan|「Japan Prefecture Component」を提供するAPIを取得|
|上記以外|| 404ページを表示 |

- World Component<br>
「海外のコロナ感染状況外部API」の情報から、海外のコロナ感染状況をグラフ化し表示するフロントエンド（React,TypeScript）

- Japan Prefecture Component<br>
「Japan Prefecture Backend」の情報から、日本のコロナ感染状況をグラフ化し表示するフロントエンド（React,TypeScript）

- Japan Prefecture Backend<br>
「日本のコロナ感染状況外部API」の情報を成形し、Japan Prefecture Component側に渡すバックエンド（Golang）

![demo](https://gyazo.com/7bdebe12cff1ab5a45f440e45f064030/raw)

# GitHub ActionsでのCI
Container,World Component, Japan Prefecture Component,Japan Prefecture Backendでは、GitHub Actionsで、プルリクエスト時による自動テストとコンテナレジストリーの更新を行なっている。

# ArgoCD
マニフェストリポジトリでの更新があった際の自動デプロイを、ArgoCDで行っている。

![demo](https://gyazo.com/ed8b886b2ea2a260c7fcf36ba9486dc3/raw)


# Web UIダッシュボードでのリソース情報の収集/監視
Kubernetes環境のリソース情報の収集や監視を行うため、Web UIダッシュボードを導入。
![demo](https://gyazo.com/094e039de479c0de5a706205ccb9f748/raw)

# Repository
Container：<br>
- https://github.com/rui-watanabe/original-covid-app-container

World Component：<br>
- https://github.com/rui-watanabe/original-covid-app-world

Japan Prefecture Component：<br>
- https://github.com/rui-watanabe/original-covid-app-japan-prefecture

Japan Prefecture Backend：<br>
- https://github.com/rui-watanabe/original-covid-app-japan-prefecture-backend

# Future
- 日本のコロナ感染状況の病院名を表示し、受け入れ可能な病院を探せるような機能を作る

# License
 [MIT license](https://en.wikipedia.org/wiki/MIT_License).
