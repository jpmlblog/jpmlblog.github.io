---
title: Azure Machine Learning で作成したモデルの推論を実行する方法について
date: 2021-01-13 12:00:00
categories:
- Azure Machine Learning
tags:
- モデル デプロイ
- 推論
---
本記事では、Azure Machne Learning で作成したモデルを SDK を使用して推論実行する方法を紹介します。  
試験的に作成したモデルの評価を行う場合には、都度 Web サービスにデプロイする方法は効率的ではないため、ローカルにロードして推論実行する手順を紹介します。また、Web サービスにデプロイして推論を実行する方法についても併せて紹介します。  
<!-- more -->
<br>

***
## ローカルにモデルをロードして評価を行う場合
実験によって作成されたモデルのテストを行う場合、ローカル環境にロードして実行する方法をお勧めいたします。以下に、自動機械学習のチュートリアルの実行によって作成されたモデルを例にご紹介いたします。  

- [チュートリアル:Azure Machine Learning の自動 ML で分類モデルを作成する](https://docs.microsoft.com/ja-jp/azure/machine-learning/tutorial-first-experiment-automated-ml)  

自動機械学習では、複数のアルゴリズムと前処理の組み合わせを試し、それぞれの実行でモデルを作成します。上記チュートリアルのとおりに実行すると、「my-1st-automl-experiment」 という名前の実験の中で、複数のモデルが作成されます。このうちひとつのモデルをローカルにロードして、推論実行する方法を紹介します。

モデルは、VotingEnsemble を選択します。  

<img src="https://jpmlblog.github.io/images/AML-model-inference/AML-algorithm-list.png" width=800px align="left" border="1"><br clear="left">  

「詳細」 タブより実行 ID を確認します。  

<img src="https://jpmlblog.github.io/images/AML-model-inference/AML-algorithm-details.png" width=500px align="left" border="1"><br clear="left">  

Notebooks メニューより任意のフォルダーに新しいノートブックを作成し、以下のコードを入力、実行します。experiment_name や run_id は、ご利用環境に合わせて適宜変更ください。  

```Python
# 実行 ID より Run オブジェクトを作成します。
from azureml.core import Experiment, Workspace
from azureml.core.experiment import Experiment
from azureml.core.run import Run

workspace = Workspace.from_config()
experiment_name = "my-1st-automl-experiment"
experiment = Experiment(workspace, experiment_name)
run = Run(experiment=experiment, run_id='AutoML_66d0eb73-e09c-435d-ae80-da060d204b09_69')

# モデル等の情報を download フォルダー配下に格納します。
run.download_files(output_directory='download')
```

```Python
# ダウンロードしたモデルをローカルにロードします。
from sklearn.externals import joblib
from azureml.core.model import Model

model_path = 'download/outputs/model.pkl'
model = joblib.load(model_path)
```

推論のテストに使用するデータは、トレーニング データに含まれていないものをご用意頂く必要があります。今回は既に用意されているテスト データを使用しますが、一般的にはトレーニング用に収集したデータの 1 割程度をテスト用に分割しておくことをお勧めします。

```Python
# チュートリアルのテスト用データをデータセットとして読み込みます。
from azureml.core.dataset import Dataset

test_data = "https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_test.csv"
test_dataset = Dataset.Tabular.from_delimited_files(test_data)
```

```Python
# y 列を削除して pandas のデータフレーム形式に変換します。
test = test_dataset.drop_columns(columns=['y'])
test_df = test.to_pandas_dataframe()

# predict メソッドを使用して推論を実行します。
pred  = model.predict(test_df)
pred.tolist()
```

以下の通り推論結果を表示できたかと思います。  

<img src="https://jpmlblog.github.io/images/AML-model-inference/AML-inference-result.png" width=700px align="left" border="1"><br clear="left">  

これらのコードは下記サンプル ノートブック [auto-ml-classification-bank-marketing-all-features.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) を参考にしておりますので、併せて参照ください。 

>**注意点**  
>時系列予測モデルの ForecastingParameters として target_rolling_window_size パラメーターを指定していると、predict の実行が失敗することが確認できています。これは predict メソッドが target_rolling_window_size パラメーターを使用したモデルでの実行をサポートしていないためです。このような場合、以下サンプル ノートブック [auto-ml-forecasting-orange-juice-sales.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb) 、にありますとおり、predict メソッドではなく forecast メソッドの利用をご検討ください。  
><img src="https://jpmlblog.github.io/images/AML-model-inference/AML-forecasting-sample.png" width=800px align="left" border="1"><br clear="left">  

***
## Web サービスとしてデプロイされたモデルを使用する
作成したモデルをローカル、ACI、AKS のいずれかに Web サービスとしてデプロイした場合、要求データを REST エンドポイントに対して送信することで推論結果を得られます。  

モデルのデプロイ方法は下記公開情報に纏められています。  

- [Azure Machine Learning を使用してモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-and-where?tabs=azcli)

SDK ベースになりますが、ローカル、ACI、AKS それぞれの公開情報を抜粋します。  

- [Azure Machine Learning コンピューティング インスタンスへのモデルのデプロイ](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-local-container-notebook-vm)  
- [Azure Container Instances にモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-azure-container-instance)
- [Azure Kubernetes Service クラスターにモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-azure-kubernetes-service?tabs=python)

デプロイした Web サービスの呼び出し方は、下記公開情報が参考になります。  

- [Web サービスとしてデプロイされた Azure Machine Learning モデルを使用する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-consume-web-service?tabs=python)  

<br>
※ 現在更新中
<br>


***
`変更履歴`  
`2021/01/13 created by Mochizuki`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  