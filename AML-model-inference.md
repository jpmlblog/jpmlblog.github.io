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

<img src="https://jpmlblog.github.io/images/AML-test-inference/AML-algorithm-list.png" width=800px>  

「詳細」 タブより実行 ID を確認します。  

<img src="https://jpmlblog.github.io/images/AML-test-inference/AML-algorithm-details.png" width=400px>  

Notebooks メニューより任意のフォルダーに新しいノートブックを作成し、以下のコードを入力、実行します。

```Python
# 実行 ID より Run オブジェクトを作成します。
from azureml.core import Experiment, Workspace
from azureml.core.experiment import Experiment
from azureml.core.run import Run

workspace = Workspace.from_config()
experiment_name = "my-1st-automl-experiment"
experiment = Experiment(workspace, experiment_name)
run = Run(experiment=experiment, run_id='AutoML_66d0eb73-e09c-435d-ae80-da060d204b09_69')

# モデル等の情報を download フォルダー配下に格納します
run.download_files(output_directory='download')
```

```Python
# ダウンロードしたモデルをローカルにロードします。
from sklearn.externals import joblib
from azureml.core.model import Model

model_path = 'download/outputs/model.pkl'
model = joblib.load(model_path)
```

```Python
# トレーニングで使用したデータをそのままテスト用データとして使用します。
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
```



## Web サービスとしてデプロイされたモデルを使用する
作成したモデルを ACI または AKS を使用して Web サービスとしてデプロイした場合、要求データを REST エンドポイントに対して送信することで推論結果を得られます。  

モデルのデプロイ方法は下記公開情報が参考になります。  

- [Azure Machine Learning を使用してモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-and-where?tabs=azcli)

AzureML SDK ベースの ACI または AKS へのデプロイ方法は、下記公開情報が参考になります。  

- [Azure Container Instances にモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-azure-container-instance)
- [Azure Kubernetes Service クラスターにモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-azure-kubernetes-service)

ACI または AKS へのデプロイは [Azure Machine Learning 専用ポータル](https://ml.azure.com/) のユーザー インターフェイスからも実行できます。

<img src="https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-nsg.png" width=800px>  

自動 ML によって



各 Web サービスのエンドポイントは、[Azure Machine Learning](https://ml.azure.com/) の [アセット] - [エンドポイント] に表示されます。  





Azure Machine Learning を使用してモデルをデプロイする
https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-and-where?tabs=azcli


Web サービスとしてデプロイされた Azure Machine Learning モデルを使用する
https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-consume-web-service

    model_1_path = Model.get_model_path(model_name='my_first_model')
    model_2_path = Model.get_model_path(model_name='my_second_model')
    
    model_1 = joblib.load(model_1_path)
    model_2 = joblib.load(model_2_path)

![Template](https://jpmlblog.github.io/images/template.png "ファイルの説明")
***