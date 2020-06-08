---
title: Machine Learning Studio (Classic) ワークスペースが削除できない事象について
date: 2020-03-10 00:00:00
categories:
- Machine Learning Studio (Classic)
tags:
- リソース削除
---
Machine Learning Studio (Classic) のワークスペース削除後も、課金が継続している場合の対処方法をご紹介します。  
<!-- more -->
<br>

***
Azure ポータル上には表示されていないリソースが継続して課金されているといった事象は、リソースの削除時の操作やサービス側の不具合等に起因して発生する可能性があります。Azure Machine Learning Studio (Classic) リソースにおいても同様に発生することが報告されており、その際の対処方法は以下の通りとなります。

(1) 課金が発生しているリソース ID を確認  
(2) リソース削除を希望するサポート リクエストを発行

それぞれ詳細手順を後述にご紹介いたします。  
<br>

### (1) リソース ID の確認手順

課金対象になっているリソースの特定には、ワークスペース ID またはリソース ID が必要になります。それぞれ確認方法は下記を参照ください。

- ワークスペース ID の確認方法

   [Azure Machine Learning Studio (Classic) ワークスペース](https://studio.azureml.net/Home) にアクセスし、[Settings] メニューよりワークスペース ID を確認します。  
   ![workspace-id.png](https://jpmlblog.github.io/images/AMLSC-cannot-delete/workspace-id.png)  


- リソース ID の確認方法

   Azure ポータルでダウンロードできる使用状況レポートの csv ファイルの ResourceId (ResourceId) よりご確認いただけます。下記のサイトの 「Azure 請求書 (.pdf) のダウンロード」 セクションの 4. の手順より、csv ファイルをダウンロードして確認します。  

   [Azure の請求書と毎日の使用状況データをダウンロードまたは表示する](https://docs.microsoft.com/ja-jp/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date)  
   ![download-invoice.png](https://jpmlblog.github.io/images/AMLSC-cannot-delete/download-invoice.png)  

### (2) リソース削除を希望するサポート リクエストを発行

確認したワークスペース ID またはリソース ID を、サポート

[Azure サポート要求を作成する方法](https://docs.microsoft.com/ja-jp/azure/azure-portal/supportability/how-to-create-azure-support-request)  
![support-request.png](https://jpmlblog.github.io/images/AMLSC-cannot-delete/support-request.png)  
![support-request-detail.png](https://jpmlblog.github.io/images/AMLSC-cannot-delete/support-request-detail.png)  

***