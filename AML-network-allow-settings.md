---
title: Azure Machine Learning 宛通信の許可リストについて
date: 2020-10-10 12:00:00
categories:
- Azure Machine Learning
tags:
- Network
- Firewall
- Proxy
---
オンプレミスの環境から Azure Machine Learning のサービスを利用する際、外部ネットワーク宛の通信をファイアウォールやプロキシ サーバー等によって制御している場合に、許可すべきホスト名等の情報をご紹介します。  

<!-- more -->
<br>

***
## ホスト名ベースの許可
Azure Machine Learning で使用する通信先ホスト名の一覧は以下サイトに一覧化されております。これらのホスト名宛の送信方向の通信を許可するよう設定ください。  

- [ファイアウォールの内側で Azure Machine Learning のワークスペースを使用する - # Microsoft のホスト](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-access-azureml-behind-firewall#microsoft-hosts)

  >|ホスト名&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|目的|
  >|:---|:---|
  >|login.microsoftonline.com|認証|
  >|management.azure.com|ワークスペース情報を取得するために使用されます|
  >|*.batchai.core.windows.net|クラスターをトレーニングします|
  >|ml.azure.com|Azure Machine Learning Studio|
  >|default.exp-tas.com|Azure Machine Learning スタジオによって使用されます|
  >|*.azureml.ms|Azure Machine Learning API によって使用されます|
  >|*.experiments.azureml.net|Azure Machine Learning で実行される実験で使用されます|
  >|*.modelmanagement.azureml.net|モデルを登録してデプロイするために使用されます|
  >|mlworkspace.azure.ai|ワークスペースを表示するときに Azure portal によって使用されます|
  >|*.aether.ms|Azure Machine Learning パイプラインを実行するときに使用されます|
  >|*.instances.azureml.net|Azure Machine Learning のコンピューティング インスタンスです
  >|*.instances.azureml.ms|ワークスペースで Private Link が有効な場合の Azure Machine Learning のコンピューティング インスタンスです|
  >|windows.net|Azure Blob Storage|
  >|vault.azure.net|Azure Key Vault|
  >|azurecr.io|Azure Container Registry|
  >|mcr.microsoft.com|基本 Docker イメージ用の Microsoft Container Registry|
  >|\<your-acr-server-name\>.azurecr.io|Azure Container Registry が仮想ネットワークの背後にある場合にのみ必要です。 この構成では、Microsoft 環境からサブスクリプションの ACR インスタンスへのプライベート リンクが作成されます。 Azure Machine Learning ワークスペースの ACR サーバー名を使用します。|
  >|*.notebooks.azure.net|Azure Machine Learning studio のノートブックで必要です。|
  >|graph.windows.net|ノートブックに必要です|

***
## ポート番号の許可
HTTPS または HTTP (443 および 80) のプロトコルで通信を行います。上記したホスト名一覧に対し、送信方向のポート番号 443 および 80 宛の通信を許可するよう設定ください。  

***
## WebSocket の考慮
ネットワーク機器などで WebSocket の通信をブロックしている場合、上述したホスト名一覧について許可いただくことをお勧めいたします。  
なお、現時点 (2020/10/10) では以下のホスト名について WebSocket の通信を行うことが確認できておりますので、宛先を絞り込む必要がある場合には、最低限以下ホスト名宛の WebSocket 通信を許可するよう設定ください。  

*.azureml.ms  
*.notebooks.azure.net  
*.instances.azureml.net  
*.experiments.azureml.net  

今後サービス側機能の変更によって別のホスト名で WebSocket 通信を必須とする場合がございますので、その際には適宜許可を追加頂くことをご検討ください。

- [(参考情報) コンピューティング クラスターとインスタンス](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-secure-training-vnet#compute-clusters--instances)
  >コンピューティング インスタンスの Jupyter 機能を動作させるには、Web ソケット通信が無効になっていないことを確認してください。

***
## IP アドレスの範囲指定で許可する場合
下記サイトに紹介されている IP アドレス一覧を使用して許可する方法につきましては、API で一覧を取得する方法が現時点 (2020/10/10) でプレビュー段階であること、またサイトからダウンロードする方法については非推奨となっていることから、利用をお勧めいたしません。  

- [オンプレミスのサービス タグ](https://docs.microsoft.com/ja-jp/azure/virtual-network/service-tags-overview#service-tags-on-premises)


***
`変更履歴`  
`2020/10/10 created by Mochizuki`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  
