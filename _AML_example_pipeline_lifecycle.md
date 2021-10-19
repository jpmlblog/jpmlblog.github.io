---
title: Azure Machine Learning パイプラインを使用した自動機械学習ライフサイクルの例
date: 9999-12-31 00:00:00
categories:
- Azure Machine Learning
tags:
- パイプライン
- ライフサイクル
---
Azure Machine Learning のライフサイクルを実現する Azure Machine Learning パイプラインの利用例について紹介させていただきます。今回紹介するライフサイクルは次の通りです。  

- ストレージ アカウント上の csv ファイル読み込み  
- AutoML を実行
- ベスト モデルを選択して ACI にデプロイ

<!-- more -->
<br>

***
## パイプラインの作成

パイプラインのサンプル ノートブックと、実験に使用するデータを以下からダウンロードします。

- [aml-pipeline-sample_1.ipynb](https://jpmlblog.github.io/files/AML_example_pipeline_lifecycle/aml-pipeline-sample_1.ipynb "aml-pipeline-sample_1.ipynb")
- [machineData.csv](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/machineData.csv "machineData.csv")

machineData.csv ファイルは、Azure Machine Learning ワークスペースの既定のストレージ アカウントの azureml-blobstore-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (xxx 部分はランダムな英数字) という名前のコンテナー配下に "Datasets" というフォルダーを作成し、アップロードします。  

<img src="https://jpmlblog.github.io/images/template.png" width=300px align="left" border="1"><br clear="left">

***
`変更履歴`  
`9999/12/31 created by ******`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  