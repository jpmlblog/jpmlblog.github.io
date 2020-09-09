---
title: FAQ
date: 2020-04-22 10:00:00
categories:
- Azure Machine Learning 全般
tags:
- FAQ
---

よくあるご質問とその回答をまとめております。
<!-- more -->
<br>

***
#### 製品を理解するために参考となる公開情報を教えて欲しい
<details><summary style="font-size: 10pt">回答</summary>
別途ブログ記事として掲載しております。以下リンクより参照ください。  
[参考となる公開情報について](https://jpmlblog.github.io/blog/2020/04/22/reference-websites/)  
</details>

***
#### Machine Learning ワークスペースを別のリソース グル―プまたは別のサブスクリプションに移動することは可能か
<details><summary style="font-size: 10pt">回答</summary>
不可能です。参考となる情報を紹介します。  
[Azure Machine Learning ワークスペースとは (#workspace-management)](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-workspace#workspace-management)  
> ! 警告  
Azure Machine Learning ワークスペースを別のサブスクリプションに移動したり、所有するサブスクリプションを新しいテナントに移動したりすることは、サポートされていません。 エラーの原因になります。
</details>

***
#### セキュリティ要件のため各リソースへのアクセスを制限することは可能か
<details><summary style="font-size: 10pt">回答</summary>
認証やアクセス権の付与、ネットワーク的な隔離、監視など、Azure Machine Learning で利用できるセキュリティ機能について、以下サイトにて纏めております。  
[Azure Machine Learning のエンタープライズ セキュリティ](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-enterprise-security)  

一部抜粋を紹介します。  

- **ストレージ サービスへのアクセスを制限する**  
   以下サイトに記載の認証方法をサポートしています。  
   [Azure Storage サービスに接続する (#supported-data-storage-service-types)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-access-data#supported-data-storage-service-types)

- **Web サービスへのアクセスを制限する**  
   TLS 1.2 の有効化、キーベースまたはトークン ベースの認証を有効化する方法があります。  
   [TLS を使用して Azure Machine Learning による Web サービスをセキュリティで保護する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-secure-web-service)  
   [Azure Machine Learning のリソースとワークフローの認証を設定する (#web-service-authentication)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-setup-authentication#web-service-authentication)  
- **外部ネットワークからのアクセスを制限する**  
   仮想ネットワークや Private Link を使用する方法があります。  
   [プライベート仮想ネットワークでのトレーニング中や推論中のネットワークの分離](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-enable-virtual-network)  
   [Azure Machine Learning ワークスペース用に Azure Private Link を構成する (プレビュー)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-configure-private-link)  

   公開情報に記載のない利用方法 (例えば、Azure SQL Server の「ファイアウォールと仮想ネットワーク」機能の使用など) はサポートされておりません。詳細につきましては [こちら](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-enable-virtual-network#machine-learning-studio) のサイトの注意書きを参照ください。  
</details>

***
#### 機能の説明にある 「プレビュー」 とは何か
<details><summary style="font-size: 10pt">回答</summary>
Azure には、マイクロソフトがお客様のご意見を収集するために提供する、プレビュー版、ベータ版、またはその他のプレリリース版の機能、サービス、ソフトウェア、またはリージョン (以下、「プレビュー」といいます) が含まれる場合があります。以下サイトの使用条件に合意することを条件に、プレビューを使用することができます。  
[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/ja-jp/support/legal/preview-supplemental-terms/)    
   > 1. 「現状有姿のまま」「瑕疵を問わない条件」「提供可能な場合に限り提供しうる形で」提供される  
   > 1. サービス レベル契約および限定的保証の対象とはならない
   > 1. カスタマー サポートの対象とならない
   > 1. 随時予告なくプレビューを変更または中止することがある
   > 1. 「一般向け提供製品」でリリースしないことを選択する場合がある

プレビューは開発段階のサービス・機能でもあるため、公開中のドキュメントと異なる仕様があったり、メンテナンスに伴い使用できなくなることがあります。以下のようなご質問につきましては、基本的に Azure サポートから回答を提供することが難しいことをご理解ください。  

- 一般サービス提供開始予定
- 予期しない動作の原因調査
- 公開情報にない仕様の確認

提供開始となった際には [Azure の更新情報](https://azure.microsoft.com/ja-jp/updates/?status=nowavailable&product=machine-learning-service,machine-learning-studio) サイトより通知されます。また、Azure ポータルまたは Azure Machine Learning のポータルで通知される場合もあります。   
  
</details>

***
#### コストの見積もり方を知りたい
<details><summary style="font-size: 10pt">回答</summary>
コストの見積もり例について下記記事に纏めています。  

- [Azure Machine Learning のコスト見積もりについて](https://jpmlblog.github.io/blog/2020/06/18/AML-estimate-costs/)  

その他、コスト見積もりの参考となる公開情報を紹介します。

- [Azure Machine Learning のコストを計画して管理する](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-plan-manage-cost)  
- [Azure Machine Learning の価格](https://azure.microsoft.com/ja-jp/pricing/details/machine-learning/)  
-  [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service)  
 
弊社より見積もりの回答が必要な場合、営業担当のタスクとして対応しております。下記サイトよりご依頼ください。  

- [Azure 営業担当者に問い合わせる](https://azure.microsoft.com/ja-jp/overview/sales-number/)
</details>

***
#### Azure Machine Learning Studio (Classic) と Azure Machine Learning のデザイナー機能はどのような違いがあるか
<details><summary style="font-size: 10pt">回答</summary>
それぞれ GUI ベースで機械学習を行うサービスとなりますが、新・旧という形では分けられておらず、データの移行にも対応していません。    

- [Azure Machine Learning](https://docs.microsoft.com/ja-jp/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning?context=azure/machine-learning/service/context/ml-context#azure-machine-learning)  
- [Azure ML Studio (クラシック)](https://docs.microsoft.com/ja-jp/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning?context=azure/machine-learning/service/context/ml-context#azure-ml-studio-classic)  

具体的な差異は以下サイトに纏められております。
 
- [Azure Machine Learning と Machine Learning Studio (classic) の違い](https://docs.microsoft.com/ja-jp/azure/machine-learning/compare-azure-ml-to-studio-classic)
</details>

***
#### リソース名に平仮名や漢字を使用可能か
<details><summary style="font-size: 10pt">回答</summary>
使用可能な文字はありますが、予期せぬエラーが発生しリソース作成が失敗する場合があるため、推奨しません。  

リソース グループ名やリソース名には、有効な文字を指定させていただいております。下記情報に従い、リソース グループ名を英数字とハイフンのみを使用して作成ください。  
 
- [Microsoft.MachineLearningServices](https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/management/resource-name-rules#microsoftmachinelearningservices)
   > Microsoft.MachineLearningServices  
   >|Entity|Scope|長さ|有効な文字|  
   >|:-----|:----|:---|:--------|  
   >|workspaces|resource group|3-33|英数字とハイフン。|  
   >|workspaces / computes|ワークスペース|2-16|英数字とハイフン。|  
</details>

***
`変更履歴`  
`2020/04/22 created by Mochizuki`  
`2020/06/18 modified by Mochizuki` 
`2020/08/26 modified by Mochizuki` 
<br>
※ 適宜追加更新します。  