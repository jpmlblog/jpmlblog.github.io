---
title: Azure Machine Learning ワークスペースの作成が Internet Explorer 11 で失敗する
date: 2020-08-31 09:00:00
categories:
- Azure Machine Learning
tags:
- Internet Explorer 11
---
Internet Explorer 11 利用時に確認できている現象について紹介します。
<!-- more -->
<br>

***
Azure Machine Learning ワークスペースの作成が失敗するといった報告が複数件寄せられております。この事象は、Azure ポータルを Internet Explorer 11 で開いてリソース作成を行った場合に発生することが確認できております。  

<div style="text-align: center;">
(再現時、下記表示のまま状態が遷移しません)
<img src="https://jpmlblog.github.io/images/AML-IE11/Create-workspace-error.png" width=600px>
</div>    

また、Azure Machine Learning ポータル (https://ml.azure.com/) の操作においても予期しないエラーが発生するといった報告もございます。これらの事象は、別のブラウザ (Microsoft Edge または Chrome) を使用することで回避できる可能性があります。  

Internet Explorer 11 は弊社サービスに限らず幅広い Web サービスで、順次サポート対象外になる予定です。恐れ入りますが、現段階で Microsoft Edge などへの移行をご検討ください。  
 
- [Microsoft 365 アプリの Internet Explorer 11 のサポート終了と Windows 10 での Microsoft Edge レガシー版のサービス終了](https://blogs.windows.com/japan/2020/08/18/microsoft-365-apps-say-farewell-to-internet-explorer-11/)

***
`変更履歴`  
`2020-08-31 created by Mochizuki`