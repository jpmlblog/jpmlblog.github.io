---
title: FAQ
date: 2020-04-22 10:00:00
categories:
- Azure Machine Learning 全般
tags:
- FAQ
---

よくあるご質問とその回答をお纏めいたします。
<!-- more -->
<br>

***
#### 製品を理解するために参考となる公開情報を教えて欲しい  
<details background="red"><summary style="font-size: 10pt">展開</summary>

別途ブログ記事として掲載しております。以下リンクより参照ください。  
[参考となる公開情報について](https://jpmlblog.github.io/blog/2020/04/22/reference-websites/)  
</details>

***
#### Machine Learning ワークスペースを別のリソース グル―プまたは別のサブスクリプションに移動することは可能か
<details><summary style="font-size: 10pt">展開</summary>

不可能です。参考となる情報を紹介します。  
[Azure Machine Learning ワークスペースとは (#workspace-management)](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-workspace#workspace-management)  
> ! 警告  
Azure Machine Learning ワークスペースを別のサブスクリプションに移動したり、所有するサブスクリプションを新しいテナントに移動したりすることは、サポートされていません。 エラーの原因になります。
</details>

***
#### セキュリティ要件のため各リソースへのアクセスを制限することは可能か
<details><summary style="font-size: 10pt">展開</summary>

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
<details><summary style="font-size: 10pt">展開</summary>

Azure には、マイクロソフトがお客様のご意見を収集するために提供する、プレビュー版、ベータ版、またはその他のプレリリース版の機能、サービス、ソフトウェア、またはリージョン (以下、「プレビュー」といいます) が含まれる場合があります。以下サイトの使用条件に合意することを条件に、プレビューを使用することができます。  
[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/ja-jp/support/legal/preview-supplemental-terms/)    
   > 1. 「現状有姿のまま」「瑕疵を問わない条件」「提供可能な場合に限り提供しうる形で」提供される  
   > 2. サービス レベル契約および限定的保証の対象とはならない
   > 3. カスタマー サポートの対象とならない
   > 4. 随時予告なくプレビューを変更または中止することがある
   > 5. 「一般向け提供製品」でリリースしないことを選択する場合がある

サポート リクエストより一般サービス提供開始予定をお問い合わせいただいても、非公開となりますので回答することができません。提供開始となった際には [Azure の更新情報](https://azure.microsoft.com/ja-jp/updates/?status=nowavailable&product=machine-learning-service,machine-learning-studio) サイトより通知されます。また、Azure ポータルまたは Azure Machine Learning のポータルで通知される場合もあります。  
</details>

***
<br>
※ 適宜追加更新します。