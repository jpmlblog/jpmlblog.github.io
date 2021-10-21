---
title: 名前に日本語を含むリソースグループを使用する場合の注意点について
date: 2021-10-16 00:00:00
categories:
- Azure Machine Learning
tags:
- 日本語
- リソースグループ
- コンピューティング インスタンス
---
リソースグループ名に日本語 (例: 機械学習用リソースグループ) など 2 バイト文字が含まれる場合、当該リソースグループに Azure Machine Learning ワークスペースを作るといくつかの管理操作がエラーとなる場合がございます。これらの事例と、推奨する設定をお纏めいたします。 
<!-- more -->
<br>

***
## 確認できている事象について
日本語を含んだ名前のリソースグループを使用する場合、コントロール プレーン操作 (管理操作) において行われる通信がエラーとなることが確認できております。これは、操作の実行時に行われる認証処理において、日本語を含むリソース ID を正しく認識できないことに起因いたします。  

現在 (2021/10/16)、確認できている事象は以下の通りです。

- コンピューティング インスタンス起動 / 停止が失敗する
- コンピューティング インスタンスのスケジュール起動 / 停止が動作しない
- 自動機械学習の DNN オプションを使用できない

また、ノートブックで Python コードを実行する際、こうしたリソース ID が使用されるような処理においても失敗する可能性がございます。オープンソースのモジュールで日本語など 2 バイト文字に対応していない場合、弊社より修正および回避策を提示することができません。

---
## 推奨する設定について
Azure Machine Learning サービスに閉じた機能であれば修正リクエストをいただくことで対処を検討させていただくことは可能ですが、完了まで時間がかかる場合があり、また別のサービスとの連携を含む機能では修正が困難となる場合もございます。  
上記を踏まえまして、リソースグループ名は **「英数字およびハイフンのみ」** としていただくことを強く推奨いたします。  

※ ワークスペースおよびコンピューティング リソースに関する名前付け制限は以下をご参照ください。

[Azure リソースの名前付け規則と制限事項 #Microsoft.MachineLearningServices](https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/management/resource-name-rules#microsoftmachinelearningservices)  
>| Entity | Scope | 長さ | 有効な文字 |
>| :---- | :---- | :---- | :---- |
>| workspaces&nbsp;&nbsp;&nbsp; | resource group&nbsp;&nbsp;&nbsp; | 3-33&nbsp;&nbsp;&nbsp; | 英数字とハイフン。|
>| workspaces / computes&nbsp;&nbsp;&nbsp; | ワークスペース&nbsp;&nbsp;&nbsp; | 2-16&nbsp;&nbsp;&nbsp; | 英数字とハイフン。|



***
`変更履歴`  
`2021/10/16 created by Mochizuki`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  