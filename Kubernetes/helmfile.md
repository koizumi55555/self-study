# Helm の概要

- Kubernetes 設定ファイルを 1 つの再利用可能なパッケージにまとめて、Kubernetes アプリケーションの作成、パッケージ化、構成、デプロイを自動化するツール。

- Kubernetesを使うと、複数のマイクロサービスを1つのデプロイにまとめて、プロセスを簡素化できるが、開発ライフサイクルを通じて Kubernetes アプリケーションを管理する際にも、バージョン管理、リソース割り当て、更新、ロールバックなど、特有の課題がいくつも発生する。これらの問題の解決策がHelmであり、Helm を使うと、デプロイの一貫性、再現性、信頼性を高めることが可能である。

## Helmを使用したKubernetes管理の簡素化
- Kubernetes のデプロイには、YAML 設定ファイルを使用するが、デプロイが複雑で頻繁に更新が行われる場合、これらのファイルのバージョンすべてを追跡するのは困難である。
- Helmは、デプロイ用YAMLファイル1つとバージョン情報を管理する便利なツールであり、このファイルを使うと、複雑なKubernetesクラスタを少しのコマンドでセットアップおよび管理できる。

### chart
- アプリケーションを Kubernetes クラスタにデプロイするのに必要なリソースをすべて含んだパッケージである。
- パッケージには、デプロイ、サービス、シークレット、および所定のアプリケーションの状態を定義した構成マップの各種 YAML 設定ファイルが含まれる。
- chartはこれらの YAML ファイルとテンプレートをパッケージ化したものであり、これらを使うことで、パラメーター化した値に基づいて追加の設定ファイルを生成可能である。
- 各chartは個別にバージョン設定と管理ができるため、設定の異なる複数バージョンのアプリケーションを簡単に管理可能である。

### config ファイル
- config ファイルにはアプリケーションの設定が含まれ、通常は YAML ファイルに保存される。
- Kubernetes クラスタのリソースは、この値に基づいてデプロイされる。

### リリース
- チャートを実行するインスタンスのことを、リリースと呼ぶ。
- `helm install` コマンドを実行すると、config ファイルとチャートファイルが取得され、すべての Kubernetes リソースがデプロイされる。

### アーキテクチャ
#### Helm クライアント
- ローカルでのチャート開発やリポジトリとリリースの管理を行うためのエンドユーザー用コマンドラインユーティリティである
#### Helm ライブラリ
- メインの処理をすべて担う。このライブラリには、Helm コマンドで指定された処理を実行するための実際のコードが含まれている。Helm ライブラリによって、config とチャートファイルの組み合わせが処理され、リリースが作成される。

## Helm のしくみ
- チャートを使用して Kubernetes アプリケーションの定義、作成、インストール、アップグレードを行います。
- Helm チャートを使用すると、クラスタを制御するために Kubernetes コマンドラインインターフェース (CLI) を使う必要も、複雑な Kubernetes コマンドを思い出す必要もなく、Kubernetes マニフェストを管理できる。

#### Helm リポジトリ
- Helm リポジトリは、Helm チャートをアップロードする場所です。
- プライベートリポジトリを作成して、組織内でチャートを共有することもできます。
- Artifact Hub という、さまざまな用途のチャートを検索してインストールできる機能を備えたグローバル Helm リポジトリもあります。


## Helm と CI/CD
- Helm を活用すると、CI/CDパイプラインでの Kubernetes アプリケーションのビルドとテストが大幅に簡単になる。
- アプリケーションを任意の環境に自動でデプロイして、ビルド時間を短縮できる。


## Helm を使うメリットとデメリット
### Helm が適しているケース
多数のマイクロサービスを含む複雑なアプリケーションを Kubernetes で実行するプロジェクトに有効である。
- アプリケーションのデプロイと管理を簡単に自動化できる。
- アプリケーションのデプロイと管理を簡単に自動化して、手作業を減らし、システムの信頼性と安定性を改善できる。
- インストールおよびアップグレードしやすいモジュール型のチャートにアプリケーションのコンポーネントを整理すると、アプリケーションコンポーネントの管理プロセスを簡素化できる。
- 複雑なシステムを手動で管理する際に発生しがちなエラーや不統一を回避できる。

### Helm が適していないケース
サーバーにデプロイする必要があるコンテナが 1 つだけのプロジェクトには向いていない。
- コンテナのデプロイの管理を Helm で行う必要はなく、Helm を使うとかえってプロセスが複雑になる可能性がある。
- 少数の Kubernetes アプリケーションを、パッケージマネージャーを使わずに手動で管理できている場合もメリットは大きくない。


## コマンド

#### `helm search`: チャートを見つける
- helm search hub は、 Helm Hub を検索。 公開されているチャートを見つけることができる。
- helm search repo は、(helm repo add で) ローカルの helm クライアントに追加したリポジトリを検索。 この検索はローカルデータ上で行われ、 パブリックネットワーク接続は必要

#### `helm install`: パッケージのインストール

新しいパッケージをインストールするには、helm install コマンドを使用。選択するリリース名と、インストールするチャートの名前を引数に取る。

#### `helm upgrade`: リリースのアップグレード

チャートの新しいバージョンがリリースされたとき、またはリリースの構成を変更したいときは、 helm upgrade コマンドを使用できる。

#### `helm rollback`: 障害時の回復

リリース中に何かが計画どおりに進まなかった場合、 helm rollback [RELEASE] [REVISION] を使用して前のリリースに簡単にロールバックできる。

#### `helm get values XXX`: クラスター内のリリースを確認

#### `helm repo`: リポジトリの操作

リポジトリを追加、一覧表示、削除するコマンド


## 具体的な実装方法等
helm create <チャート名>というコマンドを実行すると以下のようなディレクトリ構造が生成される。
``` 
sample_chart/               // ディレクトリ名がチャート名
    ┣ charts/               // サブチャートが入るディレクトリ。
    ┣ templates/            // 本体。生のk8sで動くマニフェストファイルをここに突っ込んだらとりあえずは動くはず。
        ┣ deployment.yaml
        ┣ service.yaml
        ...
    ┣ .helmignore           // .gitignore のような役割のファイル
    ┣ Chart.yaml            // README のような役割のファイル。チャート配布するときは書くほうがいいらしい。
    ┣ values.yaml           // 変数を書く。環境毎に差が出る部分は env/ の中で書く。
sample_chart-0.1.0.tgz      // **helm packageで生成される** これを使ってデプロイをする。 .gitignore に入れておこう
┣ env/                      // **自分で作る** 環境毎に異なる値の入る変数はこの中のファイルに記述
    ┣ dev.yaml
    ┣ prd.yaml
```

#### 導入前ににやるべきこと
- シンタックスエラーと区別がつかないのでHelmのtemplate構文に対応したlinterを入れておく。
- VSCodeの場合、Microsoftが出しているKubernetes[4]という拡張機能をインストールして、setting.jsonに記載する。
```
{
    "files.associations": {
        "*helmfile*.yaml": "helm"
    }
}
```


#### value.yaml: 変数の説明

values.yamlには変数を書きます。下記、記載時のポイント。

- 基本的にリソースkind毎に変数群をまとめる
  - 後述のtemplate化するときに、ややこしいことを考えなくていいのでミスしにくい
  - リソースkindのネスト内でも、実際のマニフェストのブロックに合わせたネストをしておくほうが視覚的にわかりやすい
- namespaceのようなリソースkindを超越して使うような値は、リソースkindのネストの外に置いてグローバル変数のように使うと良い
- カスタムマニフェストなんかに登場するnode.attr.zoneのようなkeyは.がエラー扱いになるので代わりの名前を考える必要がある
```
environment: dev

serviceAccountName: sample-sa

namespace: sample

deployment:
  labels: 
    app: sample
  image: nginx
  imageTag: latest
  containerPort : 8080
  env: 
    name: PORT
    value: 8080
  volumeMounts:
    mountPath: /vol
    name: sample_vol
service: 
  selector: 
    app: sample
  ports: 
    port: 80
    protocol: TCP
    targetPort: 8080
backendconfig:
  iap: 
    enabled: true
    secretName: iap-secret
  healthCheck: 
    type: HTTP
    port: 8080
    requestPath: /api/v1/health
```

#### テンプレート化
- templates/以下のマニフェストファイルはtemplate化を行うことで、前述のvalues.yamlに記述した変数を代入して使用できる。

values.yamlが以下のようになっているとき
```
deployment:
  image: nginx
```

templates/以下のマニフェストファイルでの呼び出し方は以下のようになる。
```
apiVersion: apps/v1
kind: Deployment
...
spec:
  ...
  template:
    spec:
      containers:
        - name: sample
          image: {{ .Values.deployment.image }}
```

### Helm を展開
- マニフェストファイルのtemplate化が済んだらデプロイをする。
- デプロイはチャート単位で行う。なお、チャートのデプロイの方法には新規デプロイで使うインストールと、既存のデプロイを更新するアップグレードがある。

#### 圧縮チャートファイルの作成
チャート名はhelm createの際に名付けた名前です
- `helm package <チャート名>`

#### インストールチャート
圧縮ファイルが<チャート名>-0.1.0.tgzのファイルを使ってデプロイを行う。
- `helm package <チャート名>`

#### アップグレードチャート
既存のreleaseで使用したチャートに加えた修正を環境に反映したい場合、アップグレードというデプロイを行う。
- `helm upgrade <release名> <Helmチャートの圧縮ファイル名>`

#### 導入されたリリースの確認
- `helm list -A`

#### デプロイされたリソースの削除
- `helm uninstall <release名>`
## 参考
- https://helm.sh/
- https://zenn.dev/cloud_ace/articles/highway-to-helm#fn-e9c8-1
- https://circleci.com/ja/blog/what-is-helm/