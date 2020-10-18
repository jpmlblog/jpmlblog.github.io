---
title: データセット名に UTF-8 の 1 バイト コード文字以外を使用すると参照できなくなる
date: 2020-10-19 12:00:00
categories:
- Azure Machine Learning
tags:
- 名前付け規則
- Dataset
---
データセットの名前に漢字などの全角文字を使用した場合、登録後に Azure Machine Learning スタジオから参照できなくなる事象が報告されています。具体的な事象について紹介いたします。  
<!-- more -->
<br>

***
## 名前付け規則について
Azure Machine Learning で使用するリソースの名前付け規則として、下記公開情報をのとおり、ワークスペースのリソースと、これを作成するリソース グループに名前付けの規則がございます。  

- [Azure リソースの名前付け規則と制限事項 #Microsoft.MachineLearningServices](https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/management/resource-name-rules#microsoftmachinelearningservices)
  > |Entity|Scope|長さ|有効な文字|
  > |:-----|:----|:---|:--------|
  > |workspaces|resource group|3-33|英数字とハイフン|
  > |workspaces / computes|ワークスペース|2-16|英数字とハイフン|  

<br>

また、Azure Machine Learning スタジオ上でコンピューティング リソースを作成する場合には、下記画像のように有効な文字の情報が表示され、名前の検証が行われますので、これに従い名前を指定下さい。  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/naming-rule-of-compute-instance.png">  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/naming-error1-compute-instance.png">  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/naming-error2-compute-instance.png">  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/naming-error3-compute-instance.png">  

***
## データセットの名前の場合
データセットを登録する際に指定する名前には、上述のような公開情報や、機械的な名前検証が行われません。しかしながら、UTF-8 の 1 バイト コード以外の文字 (漢字などの全角文字や異なる言語の文字) を使用した場合、登録後に Azure Machine Learning スタジオで参照できなくなるといった現象が報告されています。  

例えば、下記の通り全角の "Ｄａｔａｓｅｔ" という名前で登録するとします。  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/name-of-dataset.png">  

これをクリックすると、以下の通り HTTP 404 エラーが表示されます。  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/name-of-dataset-error.png">  

<img src="https://jpmlblog.github.io/images/AML_dataset-name/name-of-dataset-error-detail.png">  

なお、このデータセットを Azure Machine Learning スタジオ上で登録解除しようとしても、正常に処理が進まず失敗します。回避策として、Azure Machine Learning の CLI 拡張機能をすることでこれを登録解除することが出来ます。下記公開情報に従い Azure Machine Learning 

[Azure Machine Learning の CLI 拡張機能のインストールと使用](https://docs.microsoft.com/ja-jp/azure/machine-learning/reference-azure-machine-learning-cli)  

Azure CLI がインストール済みの環境では以下の実行します。
```PowerShell:拡張機能のインストールおよび更新
az extension add -n azure-cli-ml
az extension update -n azure-cli-ml
```

初回アクセス時には、以下のコマンドでワークスペースにアタッチします。
```PowerShell:初回接続時
az ml folder attach -w "ワークスペース名" -g "リソース グル―プ名"
```

以下のコマンドでデータセットの登録を解除します。
```PowerShell:データセットの登録解除
 az ml dataset unregister -n "データセット名"
```

***
`変更履歴`  
`2020/10/19 created by Mochiszuki`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  