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
   [Azure Virtual Network 内で Azure ML の実験と推論のジョブを安全に実行する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-enable-virtual-network)  
   [Configure Azure Private Link for an Azure Machine Learning workspace (Preview)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-configure-private-link)  

   公開情報に記載のない利用方法 (例えば、Azure SQL Server の「ファイアウォールと仮想ネットワーク」機能の使用など) はサポートされておりません。また、[Azure Machine Learning Studio](https://ml.azure.com/) の *ノートブック* や *自動 ML* 、*データセット* 、*データのラベル付け* は、仮想ネットワークに配置したストレージの利用をサポートしておりません。[こちら](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-enable-virtual-network) のサイトの注意書きを参照ください。  
   > ! Warning  
   Microsoft does not support using the Azure Machine Learning Studio features such as Automated ML, Datasets, Datalabeling, Designer, and Notebooks if the underlying storage has virtual network enabled.
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
<summary style="font-size: 10pt">回答</summary>
Azure Machine Learning のワークスペース自体には課金は発生しません。ワークスペースで作成したリソースに課金が発生します。  

[Azure Machine Learning の価格](https://azure.microsoft.com/ja-jp/pricing/details/machine-learning/)

主要な課金対象は、開発環境やトレーニングの実行環境として作成した仮想マシンです。また、作成したモデルをデプロイした場合、デプロイ先のサービスについても課金が発生します。参考に見積もり例をご紹介いたします。  

※ Enterprise エディションの追加料金は (6/8 現在) 発生しないため、考慮を省略します。

- 開発環境として Azure Machine Learning コンピューティング インスタンス (東日本リージョン、STANDARD_DS3_V2) を 1 日 10 時間、20 日間使用する場合  
   > 45.808 [円/時間] x 24 [時間/日] x 20 [日] = 21987.84 [円]
   
   - 参考サイト  
      [Azure Machine Learning の価格](https://azure.microsoft.com/ja-jp/pricing/details/machine-learning/)  
      [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service)  

- トレーニング ターゲットとして Azure Machine Learning コンピューティング クラスター (東日本リージョン、STANDARD_DS3_V2) を作成し、



その他の関連するリソースの課金については、お客様の使い方によって異なりますので、見積もりを回答することができません。  
 
弊社にて見積もりが必要な場合には、営業担当者へご依頼いただくことをお勧めいたします。  

https://azure.microsoft.com/ja-jp/overview/sales-number/
</details>

***
#### Machine Learning Studio (Classic) と Azure Machine Learning のデザイナー機能はどのような違いがあるか
<details><summary style="font-size: 10pt">回答</summary>
※ メンテナンス中です。

</details>

***
`変更履歴`  
`2020/04/22 created by Mochizuki`  
<br>
※ 適宜追加更新します。  