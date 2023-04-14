---
title: Azure Machine Learning Studio の parquet ファイルから作成したデータ アセットによる AutoML ジョブの [特徴量化の表示] における既知の問題について 
date: 2023-04-13 00:00:00
categories:
- Azure Machine Learning
tags:
- AutoML
---

Azure Machine Learning Studio の AutoML ジョブにおいて確認されている既知の問題についてご紹介いたします。

<!-- more -->
<br>

***
## 既知の問題の概要

Azure Machine Learning Studio から AutoML ジョブを作成する際に、parquet ファイルを使用したデータ アセットを選択した場合に [タスクと設定の選択] における [特徴量化の表示] 画面において表示されるカラムがデータ アセットのものではなく、作成に使用した parquet ファイルにおけるすべてのカラム表示されてしまう問題が確認されております。  
<br>

## 問題の詳細

問題の詳細について具体的な例を挙げてご紹介いたします。  
※ 使用するデータは Scikit Learn Toy Datasets の 1 つである [Diabetes dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset
) を使用しています。  

Azure Machine Learning Studio の [自動 ML] より画面にしたがって、自動 ML ジョブの作成を進めていきます。  

以下の例では、S4, S5, S6 および Y の 4 つのカラムを選択してデータ アセットを作成しています。  

// parquet ファイルからカラムを選択してデータ アセットを作成
<img src="https://jpmlblog.github.io/images/AML-automl-issue/azureml-parquet.png" width=500px align="left" border="1"><br clear="left">


preview を確認すると、作成時に選択した 4 つのカラム (S4, S5, S6 および Y) でデータ アセットが作成されていることが確認できます。

// parquet ファイルから作成したデータ アセットのプレビュー
<img src="https://jpmlblog.github.io/images/AML-automl-issue/azureml-parquet-preview.png" width=500px align="left" border="1"><br clear="left">

AutoML ジョブの設定を進めていくと [タスクと設定の選択] の [特徴量化の表示] を開くと、データ アセット作成時に選択したカラム (S4, S5, S6 および Y) 以外のカラムも表示されています。( parquet ファイルのすべてのカラムが表示されています。)

こちらが、本記事にてご紹介している不具合となります。

// AutoML ジョブの [特徴量化の表示] の設定画面
<img src="https://jpmlblog.github.io/images/AML-automl-issue/azureml-parquet-featurization.png" width=500px align="left" border="1"><br clear="left">


なお、csv ファイルを使用して同様な操作により作成したデータ アセットを使用した場合には、データ アセットのカラムのみ (S4, S5, S6 および Y) が表示されています。

// csv ファイルから作成したデータ アセットによる [特徴量化の表示] の設定画面
<img src="https://jpmlblog.github.io/images/AML-automl-issue/azureml-csv-featurization.png" width=500px align="left" border="1"><br clear="left">
<br>

## 対応状況

現在、本事象は Azure Machine Learning の修正予定の項目となっております。
しかしながら、現時点ではトレーニング自体は実施可能であるため、ご利用いただく際の運用によって対応していただく必要がございます。
大変恐れ入りますが、ご了承のほど何卒お願いいたします。

また、本事象が原因としたお客様のビジネス インパクトが多大なものである場合には、幣サポート チームより製品開発部門に対して、本機能の開発優先度をあげるようプッシュさせていただくことも可能です。
その際には具体的な損失金額や被害人数等の情報をご記載いただいたうえで、サポート リクエスト発行いただきご相談いただけますと幸いです。


Azure サポート リクエストを発行いただける際には 下記サイトをご参考いただけますと幸いです。

- [Azure サポート要求を作成する](https://docs.microsoft.com/ja-jp/azure/azure-portal/supportability/how-to-create-azure-support-request)  
<br>

※ 一般的に、[Azure portal](https://portal.azure.com/) の [ヘルプとサポート] から [新しいサポート リクエスト] を作成頂くことをお勧めいたします。


***
`変更履歴`  
`2023/04/** created by Narita`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。 
