---
title: Azure Machine Learning のコスト見積もりについて
date: 2020-06-18 09:00:00
categories:
- Azure Machine Learning
tags:
- 価格
---
Azure Machine Learning のコスト見積もりについて、参考となる情報を紹介します。
<!-- more -->
<br>

***
本記事では具体的なコストの見積もり例を紹介します。  
コストの管理に関する基本的な考え方は、下記サイトの内容を参照ください。  

- [Azure Machine Learning のコストを計画して管理する](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-plan-manage-cost)  
<br>

## コスト見積もり例について
Azure Machine Learning のワークスペース自体には課金は発生しません。ワークスペースで作成したリソースに課金が発生します。  

例えば、開発環境として使用する Azure Machine Learning コンピューティング インスタンスや、トレーニングの実行環境として使用する Azure Machine Learning コンピューティング クラスターは、稼働時間分が課金対象となります。また、作成したモデルをデプロイした場合、デプロイ先のサービスについても比較的大きな課金が発生します。参考に見積もり例を後述に紹介します。  

※ Enterprise エディションの追加料金は (6/8 現在) 発生しないため、考慮を省略します。  

※ ご利用方法によって試算より誤差が生じる場合がありますので、予めご留意ください。

- 開発環境として Azure Machine Learning コンピューティング インスタンス (東日本リージョン、STANDARD_DS3_V2) を 1 日 10 時間、20 日間使用する場合  

   → `45.808 [円/時間] x 10 [時間/日] x 20 [日] = 9161.6 [円]`

   - 参考サイト  
   [Azure Machine Learning の価格](https://azure.microsoft.com/ja-jp/pricing/details/machine-learning/)  

- トレーニング ターゲットとして Azure Machine Learning コンピューティング クラスター (東日本リージョン、STANDARD_DS3_V2) を最小 0 ノード、最大 2 ノードで作成し、2 ノードで 1 日 4 時間、20 日間使用する場合  

   → `45.808 [円/時間/ノード] x 2 [ノード] x 4 [時間/日] x 20 [日] = 7329.28 [円]`

   - 参考サイト  
   [サポートされている VM シリーズおよびサイズ](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-compute-target#supported-vm-series-and-sizes)  
   [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service) 

- 推論用クラスターとして Azure Kubernetes Service の仮想マシン (東日本リージョン、STANDARD_DS12_V2) を 3 ノードで作成し、20 日間使用する場合  
   > 注意:  
   コア数合計を 12 以上で作成する必要があります。  

   → `51.408 [円/時間/ノード] x 3 [ノード] x 24 [時間/日] x 30 [日] = 111041.28 [円]`

   - 参考サイト  
   [Azure Kubernetes Service (AKS) の価格](https://azure.microsoft.com/ja-jp/pricing/details/kubernetes-service/)  
   [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service)  

- モデルを Azure Container Instance (vCPU 1、メモリ 2 GiB) にデプロイし、30 日間使用する場合  
   > 注意:  
   Azure Machine Learning で ACI にモデルをデプロイする場合、指定したコンテナーに加えて azureml-init-aci および azureml-fe-aci (それぞれ vCPU 0.1、メモリ 0.5 GiB) が作成されます。

   (vCPU)  
   0.0015743 [円/秒/vCPU] x 1.2 [vCPU] x 3600 [秒/時間] x 24 [時間/日] x 30 [日] = 4896.70272 [円]  
   (メモリ)  
   0.0001721 [円/秒/GiB] x 3 [Gib] x 3600 [秒/時間] x 24 [時間/日] x 30 [日] = 1338.2496 [円]  
   → `4896.70272 + 1338.2496 = 6234.95232 [円]`

   - 参考サイト  
   [Container Instances の価格](https://azure.microsoft.com/ja-jp/pricing/details/container-instances/)  
   [料金計算ツール (+Container Instance)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=container-instances)  
<br>  

## 見積もりが難しいコストについて
上記に加えて以下リソースの課金が発生いたします。これらはご利用方法によって金額が大きく異なりますため、試算を含めた見積もりを例示することができません。一般的に上述の料金と比較して数パーセント程度の小さい金額となりますため、一定期間ご利用いただいた後、実際の課金額を基に見積もることをお勧めします。  

- [Azure Container Registry Basic アカウント](https://azure.microsoft.com/ja-jp/pricing/details/container-registry/)  
- [Azure ブロック BLOB Storage (汎用 v1)](https://azure.microsoft.com/ja-jp/pricing/details/storage/blobs/)  
- [Key Vault](https://azure.microsoft.com/ja-jp/pricing/details/key-vault/)  
<br>

## 見積もりの依頼について
弊社より見積もりの回答が必要な場合、営業担当のタスクとして対応しております。下記サイトを参考にご依頼ください。  

- [Azure 営業担当者に問い合わせる](https://azure.microsoft.com/ja-jp/overview/sales-number/)  
***
`変更履歴`  
`2020/06/18 created by Mochizuki`  