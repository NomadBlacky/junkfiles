# PredictionIO - ビズリーチ

+ PredictionIOは何が出来るのか
+ アーキテクチャ
+ 動かし方
+ EngineTemplateの実装方法


## 機械学習の話題

+ AlphaGo
+ Google翻訳

## 深層学習の流行

+ TensorFlow
+ Chaniner
+ dsstne - amazon
+ torch 
+ theano

## SalesForce

機械学習関連の企業を買収してる

+ BeyondCore
+ Coolan
+ EdgeSpring
+ Implisit
+ ... PrdictionIO

## PredictionIO とは

+ 最先端のOSSを組み合わせて作られた機械学習サーバー
+ テンプレートから予測エンジンを作ってWebサービスとしてデプロイ出来る
+ リアルタイムにクエリへ結果を返すことが出来るよ
+ 誤差の調整や、評価の仕組みもあるよ
+ ばっちでも、リアルタイムでもいろんなプラットフォームからのデータをまとめてつっこめるよ

+ SDK
  + Ruby
  + PHP
  + など

+ 仕組み化されたエンジンテンプレートがあるから機械学習のモデル作成がらく
+ SparkMLLいbやOpenNLPなど機械学習、データ処理ライブラリを使う
+ 自分の学習モデルを実装して組み込める

## 主要開発メンバ

+ *Pat Ferrell - ActionML*
+ Tamas Jambor - Channel4
+ Justin Yip - independent
+ Xusen Yin - USC
+ Lee Moon Soo - NFLabs ←ApacheZeppelinの作者
+ *Donald Szeto - Salseforce*

## Event Server

+ データ収集用にHTTPベースのEventAPI

## pio コマンド

+ $ pio status

## 3分クッキング 

+ ./make-distribution.sh
+ PATHを通す
+ pio
  + pio-start-all
	+ 必要なインスタンスが立ち上がる
  + pio-stop-all
+ localhost:7070
  + イベントサーバー
+ テンプレートを落としてくる
  + 公式ページ「TEMPLATES」
  + とりあえず動かすだけならこれでおk
+ pio app list
  + アプリケーションの一覧
  + データを複数のアプリケーションからとってくる
  + Token - 認証
+ pio app new hoge
  + アプリケーションの新規作成
+ localhost:7070/events.json?accesskey=$ACCESS_KEY
  + データを投げる

## 学習する

+ engine.json
  + 機械学習アルゴリズムの設定
+ pio train
  + 学習を行う
+ pio deploy
  + 結果を返すサーバーを立てる
+ curl localhost:8080/queries.json
  + 実際に予測を行う

## Versions

+ 最後のリリース v0.9.6

## PIO CLI

+ status
  + インスタンスの起動状況など
+ version
  + バージョンの表示
+ build
  + 
+ train
  + 
+ deploy
+ accesskey
+ export
+ run
+ eval
+ dashboard

+ コマンドを定義しているScalaのコードを見たほうがはやい
  + tools.console.Console
  + scopt でコマンドを定義されてる
  
## 設定

+ conf/pio-env.sh
  + 使用するストレージの設定など

## Templates

+ Recomendation

## D-A-S-E

+ D
  + Data Source and Data Preparator
+ A
  + Algorithm
+ S
  + Serving
+ E
  + Evaluation Metrics

## 機械学習？

+ Extracting
  + 特徴抽出など
  + TF-IDF
  + Word2Vec
  + Count Vectorizer
+ Transforming
  + データを加工
  + Tokenizer 携帯要素解析
  + StopWordsRemover
  + n-gram 文字分析
+ Classification
  + 分類問題　教師あり
  + Logistic Reggression
  + Decision tree
+ Regression
  + 回帰分析
  + Linear regression
+ Clustering
  + 教師なしクラスタ
  + K-means
  + トピック抽出
+ Collaborative filtering
  + 
  

  
## アーキテクチャ

1代でも動くよ

+ Hadoop 2.7.2
+ HBase 1.2.4
+ Spark 1.6.3
  + not 2.x
+ Elasticsearch 1.7.5
  + not 2.x
  
## HBase

+ 列思考DB
+ 分散データベース
+ Google BigTableがモデル
+ Java
+ Hadoop

+ Spark
  + 分散型全文検索エンジン
  + モデルのバージョン
  + エンジンのバージョン
  + アクセスキーとAppidのマッピング
  + 学習結果のモデルなどメタデータの管理
  
+ HDFS
  + 全文検索エンジン
  + データセットの読み込み・モデルの書き出し
  
+ Hadoop MapReduce
  + 処理時間がかかる
  + オーバーヘッドが大きい
  + 機械学習の繰り返し処理では性能が出ない
  + そこでSpark
  
+ Spark
  + キャッシュ
	+ メモリに保持
	+ 載り切らないならデイスク
  + Rdd
	+ Scalaのコレクション
  + Sparkは機械学習向き！
  
+ Hadoopはいらないの？
  + YARNの上に色々乗っけられるよ
  
## TemplateSample

+ PEvents
  + 学習時にSparkから呼び出す
  + データストアにHaddop経由でアクセス
  + RDD[Event]を返す
+ LEvents
  + EventServerコール時のデータストアへのアクセス
  + Future[Event]を返す
  
