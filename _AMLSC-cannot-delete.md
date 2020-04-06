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
Azure ポータル上には表示されていないリソースが継続して課金されているといった事象は、リソースの削除時の操作やサービス側の不具合等に起因して発生する可能性があります。Machine Learning Studio (Classic) リソースにおいても同様に発生することが報告されており、その際の対処方法は以下の通りとなります。

(1) 課金が発生しているリソース ID を確認  
(2) リソース削除を希望するサポート リクエストを発行

それぞれ詳細手順を後述にご紹介いたします。  
<br>

### (1) リソース ID の確認手順

以下の手順に

- [Azure の請求書と毎日の使用状況データをダウンロードまたは表示する](https://docs.microsoft.com/ja-jp/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date)






リソース ID を基に製品開発部門へ削除依頼を実施させていただく必要があります。  

以下の手順にて

Azure ポータルでダウンロードできる使用状況レポートの csv ファイルから、該当の ResourceId (ResourceId) の項目をご確認ください。  





***