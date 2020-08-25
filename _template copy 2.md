---
title: モデルを使った推論の実行方法について
date: 2020-08-19 12:00:00
categories:
- Azure Machine Learning
tags:
- デプロイ
- 推論
---
テンプレート
<!-- more -->
<br>

***
Azure Machine Learning を使用して、モデルの推論を実行する方法について紹介します。  

---
## Web サービスの


    model_1_path = Model.get_model_path(model_name='my_first_model')
    model_2_path = Model.get_model_path(model_name='my_second_model')
    
    model_1 = joblib.load(model_1_path)
    model_2 = joblib.load(model_2_path)

![Template](https://jpmlblog.github.io/images/template.png "ファイルの説明")
***