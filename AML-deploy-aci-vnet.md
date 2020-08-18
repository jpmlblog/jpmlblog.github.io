---
title: モデルを仮想ネットワーク上の Azure Container Instances (ACI) にデプロイする方法について
date: 2020-08-18 12:00:00
categories:
- Azure Machine Learning
tags:
- Azure Container Instances
- 仮想ネットワーク
---
モデルを仮想ネットワーク上の ACI にデプロイする方法について紹介します。  
<!-- more -->
<br>

***
Azure Machine Learning を使用して Azure Container Instances (ACI) にモデルを Web サービスとしてデプロイする方法は、以下の公開情報が参考になります。  

- [Azure Container Instances にモデルをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-azure-container-instance)  

仮想ネットワーク上の ACI にモデルをデプロイする方法は、以下の公開情報に記載されておりますが、手順が分かりづらいため、後述に具体的な手順を紹介させていただきます。

- [Azure Container Instances (ACI) を使用する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-enable-virtual-network#use-azure-container-instances-aci)  

---
## 1. 仮想ネットワークの作成
仮想ネットワークにサブネットを追加、[サブネットの委任] を設定します。具体的な手順は以下を参照ください。  

### 仮想ネットワークの作成例
「[サブネットの委任を追加または削除する - 仮想ネットワークの作成](https://docs.microsoft.com/ja-jp/azure/virtual-network/manage-subnet-delegation#create-the-virtual-network)」 の手順に従って進めます。  
[基本] タブの設定例です。仮想ネットワークはワークスペースと同じリソースグループに作成します。同じリソースグル―プの既存の仮想ネットワークでも使用可能です。名前、地域は任意です。  

<img src="https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-vnet1.png" width=100%>  

[IP アドレス] タブの設定例です。仮想ネットワークの IP アドレス帯は任意で変更可能です。サブネットはそのままで進めます。  

<img src="https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-vnet2.png" width=100%>  

[セキュリティ]、[タグ] タブも既定のまま進め、リソースを作成します。  

<img src="https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-vnet3.png" width=100%>  

### サブネットの作成例
「[サブネットの委任を追加または削除する - サブネットを Azure サービスに委任する](https://docs.microsoft.com/ja-jp/azure/virtual-network/manage-subnet-delegation#delegate-a-subnet-to-an-azure-service)」 の手順に従って進めます。  
作成した仮想ネットワークでサブネットを作成します。名前、アドレス範囲は任意です。サブネットの委任に `Microsoft.ContainerInstance/containerGroups` を指定します。  

<img src="https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-subnet1.png" width=100%>  

<img src="https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-subnet2.png" width=100%> 

---
## 2. 仮想ネットワークへのモデル デプロイ
[こちらのサイト](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-deploy-azure-container-instance#using-the-sdk) のコードに vnet_name、subnet_name を追加して実行します。なお、location を指定しない場合 westus が設定されるため、仮想ネットワークが westus 以外の場合には location を指定する必要があります。  

### ACI へのデプロイ例
上記公開情報では [model]、inference_config が事前に設定されていることが前提となっています。ここではモデル model.pkl、Conda 依存関係ファイル myenv.yml、エントリ スクリプト ファイル score.py を使用して作成する方法を紹介します。  

```Python
from azureml.core import Workspace
from azureml.core import Environment
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

ws = Workspace.from_config()
model = Model.register(model_path="model.pkl",
                       model_name="mymodel",
                       workspace=ws)
env = Environment.from_conda_specification(name='myenv', file_path='myenv.yml')
inference_config = InferenceConfig(entry_script="score.py", environment=env)
```

```Python
from azureml.core.webservice import AciWebservice, Webservice
from azureml.core.model import Model

deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1,
    location = "japaneast", vnet_name = "amlvnet1", subnet_name = "amlsubnet1")
service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
```

作成された REST エンドポイントは、Azure Machine Learning ポータルの [エンドポイント] から確認できます。  

![AML-create-aci.png](https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-aci.png)  

### Web サービスの動作確認例
Web サービスと同じ仮想ネットワーク上にあるコンピュート インスタンスから Web リクエストを実行する方法を紹介します。まず、[こちらのサイト](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-enable-virtual-network#compute-clusters--instances) を参照し、ネットワーク セキュリティ グループ (NSG) を作成します。下記画像ではリージョンの指定を省略しています。  

![AML-create-nsg.png](https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-nsg.png)  

作成した NSG を amlvnet1 の default サブネットに設定します。  

![AML-change-nsg.png](https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-change-nsg.png)  

amlvnet1 の default サブネットに Compute Instance を作成します。  

![AML-create-compute.png](https://jpmlblog.github.io/images/AML-deploy-aci-vnet/AML-create-compute.png)  

作成したコンピュート インスタンス上で以下のコードを実行します。[こちら](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-consume-web-service#call-the-service-python) のサイトの手順が参考になります。  

```Python
import requests
import json

scoring_uri = 'http://10.0.1.4/score'

data = {"data":
       [[ *** モデルに併せてデータを設定ください *** ]]
       }

input_data  = json.dumps(rawdata)

headers = {'Content-Type': 'application/json'}
resp = requests.post(scoring_uri, input_data , headers=headers)
print(resp.text)
```

***
`変更履歴`  
`2020/08/18 created by Mochizuki`  