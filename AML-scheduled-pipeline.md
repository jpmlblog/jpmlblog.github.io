---
title: Azure Functions を使用してパイプラインを定期的に実行する方法について
date: 2020-11-30 12:00:00
categories:
- Azure Machine Learning
tags:
- Azure Functions
---
Azure Functions のタイマー トリガーを使用して、Azure Machine Learning で公開したパイプラインを定期的に実行する方法を紹介します。
<!-- more -->
<br>

***
前述

(参考情報) 
- [Azure でタイマーによってトリガーされる関数を作成する](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-create-scheduled-function) 

***
## 事前準備
本記事では、Visual Studio Code を使用して Azure Functions へデプロイを行います。事前に以下を満たしているかご確認ください。  

- Azure Machine Learning [ワークスペース リソースの作成](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-workspace#create-a-workspace)
- Azure Machine Learning ワークスペース リソースへの[サービス プリンシパル認証設定](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-setup-authentication#service-principal-authentication)  
  (手順参考) 「[Authentication in Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/manage-azureml-service/authentication-in-azureml/authentication-in-azureml.ipynb)」 の Service Principal Authentication セクション  
- [Visual Studio Code](https://code.visualstudio.com/) のインストール
- [Azure Functions 拡張機能](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-develop-vs-code?tabs=csharp#install-the-azure-functions-extension) のインストール

***
## 設定手順
### Azure Functions のリソースの作成
Azure ポータルより Function App リソースを作成します。設定項目は、後述の画像を参照ください。

(参考) [Azure Portal で初めての関数を作成する](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-create-first-azure-function)  
(参考) [クイック スタート:Visual Studio Code を使用して Azure で関数を作成する](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-python)  

- 「基本」情報を設定します。
&emsp;設定例:
&emsp;&emsp;リソース グループ：任意
&emsp;&emsp;関数アプリ名：任意
&emsp;&emsp;公開：コード
&emsp;&emsp;ランタイム スタック：Python
&emsp;&emsp;バージョン：3.8
&emsp;&emsp;地域：任意

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-resource-01.png" width=600px>  

- 「ホスティング」情報を設定します。
&emsp;設定例:
&emsp;&emsp;ストレージ アカウント：既定値
&emsp;&emsp;オペレーティング システム：Linux
&emsp;&emsp;プランの種類：既定値

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-resource-02.png" width=600px>  


### Azure Functions プロジェクトの作成  
Visual Studio Code を起動し、新しいプロジェクトを作成します。

- 「Create New Project」ボタンをクリックします。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-01.png" width=600px>

- 作業用のローカル フォルダーを選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-02.png" width=600px>

- プログラミング言語を選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-03.png" width=600px>

- 言語のバージョンを選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-04.png" width=600px>

- 「Timer trigger」を選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-05.png" width=600px>

- Function App の名前を付けます。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-06.png" width=600px>

- 実行間隔を表す NCRONTAB 式※の値を入力します。ここでは、 5 分ごとに 1 回トリガーする、という意味になります。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-07.png" width=600px>

※ NCRONTAL 式の入力規則については下記ドキュメントをご参照ください。
- [NCRONTAB 式](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-bindings-timer?tabs=csharp#ncrontab-expressions) 

- 「Add to workspace」を選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-08.png" width=600px>

- 画面が更新されるのを待って、サンプルコードを「\_\_init\_\_.py」ファイルにコピーして、保存します。なお、下記の箇所の値を置き換える必要があります。
&lt;Client_Secret_Value&gt;：シート「サービス プリンシパル 認証設定」のステップ 8 で取得した「クライアント シークレット」の値
&lt;Tenant_Id&gt;：シート「サービス プリンシパル 認証設定」のステップ 10 で取得した「ディレクトリ (テナント) ID」の値
&lt;Application_Id&gt;：シート「サービス プリンシパル 認証設定」のステップ 10 で取得した「アプリケーション (クライアント) ID」の値
&lt;Subscription_Id&gt;：Azure サブスクリプション ID
&lt;Pipeline_Id&gt;：実行したいパイプラインの ID ※
&lt;Experiment_Name&gt;：実験の名前

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-09.png" width=600px>

サンプルコード
```python
import datetime
import logging

import azure.functions as func

from azureml.core.authentication import ServicePrincipalAuthentication
from azureml.pipeline.core import PublishedPipeline
from azureml.core import Workspace
import requests

SVC_PR_PWD = "<Client_Secret_Value>"

def main(mytimer: func.TimerRequest) -> None:
    utc_timestamp = datetime.datetime.utcnow().replace(
        tzinfo=datetime.timezone.utc).isoformat()

    if mytimer.past_due:
        logging.info('The timer is past due!')

    logging.info('Python timer trigger function ran at %s', utc_timestamp)
    
    logging.info('Service Principal Authentication')
    svc_pr = ServicePrincipalAuthentication(
        tenant_id="<Tenant_Id>",
        service_principal_id="<Application_Id>",
        service_principal_password=SVC_PR_PWD)
   
    logging.info('Get Workspace')
    ws = Workspace(subscription_id="<Subscription_Id>",
                    resource_group="Azure-Machine-Learning-RG",
                    workspace_name="machine-learing",
                    auth=svc_pr)
    
    logging.info('Get Published Pipeline')
    pipeline = PublishedPipeline.get(ws, id='<Pipeline_Id>')
    
    logging.info('Run Published Pipeline')
    pipeline_run = pipeline.submit(ws, '<Experiment_Name>')

    logging.info('Waiting result')
    pipeline_run.wait_for_completion(show_output=True)
  ```

- 以下のライブラリー名を「requirements.txt」ファイルに追加して、保存します。
```python
azureml-core
requests
azureml-pipeline-core
```

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-10.png" width=600px>

- Azure アイコンをクリックして、Function App を右クリックして、「Deploy to Function App...」を選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-11.png" width=600px>

- サブスクリプションを選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-12.png" width=600px>

- デプロイ先の Function App リソースを選択します。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-13.png" width=600px>

- 「Deploy」ボタンをクリックします。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-14.png" width=600px>

- デプロイが完了したら、画面右下に下記のメッセージが表示されます。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-15.png" width=600px>


***
## 動作確認
Azure ポータルより、作成したタイマー トリガーが存在し、有効になっていることを確認します。  

- Function App の動作を確認します。デプロイ先の Function App リソースに移動して、「関数」ボタンをクリックします。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-16.png" width=600px>

- 作成した Function の名前をクリックします。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-17.png" width=600px>

- 「モニター」ボタンをクリックして、呼び出しのトレースを確認できます。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-18.png" width=600px>

- 上記の「日付」項目をクリックしますと、ログを確認できます。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-19.png" width=600px>

- Machine Learning リソースの実験画面で、Function App の Timer Trigger にトリガーされたパイプラインの実行を確認できます。

<img src="https://jpmlblog.github.io/images\AML-scheduled-pipeline/create-function-project-20.png" width=600px>


***
`変更履歴`  
`2020/12/01 created by Chao`