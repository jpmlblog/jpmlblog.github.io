---
title: Azure Functions を使用してコンピューティング インスタンスを自動停止する方法について
date: 2020-09-23 00:00:00
categories:
- Azure Machine Learning
tags:
- Azure Functions
---
Azure Functions のタイマー トリガーを使用して、Azure Machine Learning のコンピューティング インスタンスを自動停止する方法を紹介します。
<!-- more -->
<br>

***
Azure Machine Learning のコンピューティング インスタンスは、一度起動すると停止操作を行わない限り起動し続けます。起動時間に比例して課金が発生しますので、停止忘れてしまった時の損失が思いのほか大きいこともあります。  

本記事では、Azure Functions のタイマー トリガーを使用して、Azure Machine Learning SDK (AzureML SDK) のコードを決まった時刻に実行させることで、コンピューティング インスタンスを自動停止させる方法を紹介します。  

(補足)  
Azure Automation は Azure CLI および Python 3 の実行をサポートしていないため、本記事では触れておりません。  

***
## 事前準備
本記事では、Visual Studio Code を使用して Azure Functions へデプロイを行います。事前に以下を満たしているかご確認ください。  

- 有効な Azure サブスクリプション (Azure Machine Learning を使用している時点で満たしています)
- [Visual Studio Code](https://code.visualstudio.com/) のインストール
- [Azure Functions 拡張機能](https://docs.microsoft.com/ja-jp/azure/azure-functions/functions-develop-vs-code?tabs=csharp#install-the-azure-functions-extension) のインストール

参考:
- [チュートリアル:Visual Studio Code を使用して Python でサーバーレスの Azure Functions を作成してデプロイする](https://docs.microsoft.com/ja-jp/azure/developer/python/tutorial-vs-code-serverless-python-01)

## 設定手順
(1) Azure ポータルより、Azure Functions のリソースを作成します。

<img src="https://jpmlblog.github.io/images/AML_functions-autostop/create-functions-resource-1.png" width=300px>  

(1) Visual Studio Code を起動し、アクティビティ バーの Azure アイコンを選択、 [Functions] 領域で [Create NCreate New Project New Project (新しいプロジェクトの作成)] アイコンを選択します。  

<img src="https://jpmlblog.github.io/images/AML_functions-autostop/create-new-project.png" width=300px>  

(2) 

[Azure Functions のドキュメント](https://docs.microsoft.com/ja-jp/azure/azure-functions/)


Azure Machine Learning SDK (AzureML SDK) の ComputeInstance クラスのメソッドを使用することで、コンピューティング インスタンスの起動および停止を操作することが可能です。  

- [ComputeInstance class](https://docs.microsoft.com/ja-jp/python/api/azureml-core/azureml.core.compute.computeinstance.computeinstance?view=azure-ml-py)



<img src="https://jpmlblog.github.io/images/template.png" width=300px>  

***
## (参考) AzureML CLI を使用する場合
Azure Machine Learning CLI (AzureML CLI) は Azure CLI への拡張機能です。下記サイトに具体的な使用方法が紹介されております。  

- [Azure Machine Learning の CLI 拡張機能のインストールと使用](https://docs.microsoft.com/ja-jp/azure/machine-learning/reference-azure-machine-learning-cli)

関連するコマンドは以下サイトより確認可能です。

- [az ml computetarget computeinstance](https://docs.microsoft.com/ja-jp/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest)

本記事では、コンピューティング インスタンスの起動停止に関連する操作部分のみ抜粋して以下に紹介します。  

```powershell
# Azure サブスクリプションへの CLI の接続  
az login  

# 拡張機能のインストール  
az extension add -n azure-cli-ml  
 
# 拡張機能の更新  
az extension update -n azure-cli-ml  
 
# ワークスペースへの接続  
az ml folder attach -w <ワークスペース名> -g <リソース グル―プ名>  
 
# Compute Instance の起動  
az ml computetarget computeinstance start -n <コンピューティング インスタンス名>  
 
# Compute Instance の停止  
az ml computetarget computeinstance stop -n <コンピューティング インスタンス名>  
```
***
`変更履歴`  
`9999/12/31 created by ******`