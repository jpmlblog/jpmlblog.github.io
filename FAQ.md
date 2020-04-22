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
## セキュリティ要件のため外部アクセスを制限したい  

認証やアクセス権の付与、ネットワーク的な隔離、監視など、Azure Machine Learning で利用できるセキュリティ機能について、以下サイトにて纏めております。  
[Azure Machine Learning のエンタープライズ セキュリティ](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-enterprise-security)

- **ストレージ サービスへのアクセスを制限する**  
   以下サイトに記載の認証方法をサポートしています。  
   [Azure Storage サービスに接続する (#supported-data-storage-service-types)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-access-data#supported-data-storage-service-types)

- **Web サービスへのアクセスを制限する**  
   TLS 1.2 の有効化、キーベースまたはトークン ベースの認証を有効化する方法があります。  
   [TLS を使用して Azure Machine Learning による Web サービスをセキュリティで保護する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-secure-web-service)  
   [Azure Machine Learning のリソースとワークフローの認証を設定する (#web-service-authentication)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-setup-authentication#web-service-authentication)  

- **外部ネットワークからのアクセスを制限する**  
   仮想ネットワークや Private Link を使用する方法があります。設定することによって使用できなくなる機能がある点や、公開情報に記載のない利用方法は基本的にサポートされていない点について予め留意ください。  
   [Azure Virtual Network 内で Azure ML の実験と推論のジョブを安全に実行する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-enable-virtual-network)  
   [Configure Azure Private Link for an Azure Machine Learning workspace (Preview)](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-configure-private-link)  

## 「プレビュー」機能の制限について

Azure には、マイクロソフトがお客様のご意見を収集するために提供する、プレビュー版、ベータ版、またはその他のプレリリース版の機能、サービス、ソフトウェア、またはリージョン (以下、「プレビュー」といいます) が含まれる場合があります。以下サイトの使用条件に合意することを条件に、プレビューを使用することができます。  
[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/ja-jp/support/legal/preview-supplemental-terms/)    
   > 1. 「現状有姿のまま」「瑕疵を問わない条件」「提供可能な場合に限り提供しうる形で」提供される  
   > 2. サービス レベル契約および限定的保証の対象とはならない
   > 3. カスタマー サポートの対象とならない
   > 4. 随時予告なくプレビューを変更または中止することがある
   > 5. 「一般向け提供製品」でリリースしないことを選択する場合がある


***
<br>
※ 適宜追加更新します。
