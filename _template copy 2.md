---
title: Azure Machine Learning の専用ポータルで
date: 2020-08-25 12:00:00
categories:
- Azure Machine Learning
tags:
- デプロイ
- 推論
---
テンプレート
<!-- more -->
<br>

***
Azure Machine Learning を使用して、モデルの推論を実行する方法について紹介します。  




---
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