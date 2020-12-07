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

***
## コスト見積もり例について
Azure Machine Learning のワークスペース自体には課金は発生しません。ワークスペースで作成したリソースに課金が発生します。  

例えば、開発環境として使用する Azure Machine Learning コンピューティング インスタンスや、トレーニングの実行環境として使用する Azure Machine Learning コンピューティング クラスターは、稼働時間分が課金対象となります。また、作成したモデルをデプロイした場合、デプロイ先のサービスについても比較的大きな課金が発生します。参考に見積もり例を後述に紹介します。  

※ Enterprise エディションの追加料金は発生しないため、考慮を省略します。  

※ ご利用方法によって試算より誤差が生じる場合がありますので、予めご留意ください。

### コンピューティング インスタンス
開発環境として Azure Machine Learning コンピューティング インスタンス (東日本リージョン、STANDARD_D11_V2) を 1 日 10 時間起動し、20 日間使用する場合の月額  

<font color="#FF0000">**注意:**  
  VM としての料金に加えて、下記 3 つのサービスに対して課金が発生いたします。これは、コンピューティング インスタンスで使用する VMSS (仮想マシン スケール セット) に紐づいており、コンピューティング インスタンスを停止しても VMSS は削除されないため、課金も継続いたします。  

例 (東日本リージョン):  
- ロード バランサー (約 42.336 [円/日])
- パブリック IP アドレス (約 8.467 [円/日])
- マネージド ディスク (VM サイズや利用状況に依存 例: Standard_D11_v2 作成直後で約 4.268 [円/日])

(参考) [Managing a compute instance](https://docs.microsoft.com/en-us/azure/machine-learning/concept-compute-instance#managing-a-compute-instance)  
>Please note stopping the compute instance stops the billing for compute hours but you will still be billed for disk, public IP, and standard load balancer.
</font>

(VM)  
`25.648 [円/時間] x 10 [時間/日] x 20 [日] = 5129.6 [円]`  

(関連サービス)  
`(42.336 + 8.467 + 4.268 [円/日]) x 30 [日] = 1652.13 [円]`  

→ 合計 `5129.6 + 1652.13 = 6781.73 [円]`  

- 参考サイト  
  [Azure Machine Learning の価格](https://azure.microsoft.com/ja-jp/pricing/details/machine-learning/)  
  [Load Balancer の価格](https://azure.microsoft.com/ja-jp/pricing/details/load-balancer/)  
  [パブリック IP アドレスの料金](https://azure.microsoft.com/ja-jp/pricing/details/ip-addresses/)  
  [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service)  


### コンピューティング クラスター
トレーニング ターゲットとして Azure Machine Learning コンピューティング クラスター (東日本リージョン、STANDARD_DS3_V2) を最小 0 ノード、最大 2 ノードで作成し、2 ノードで 1 日 4 時間、20 日間使用する場合の月額  

→ `45.808 [円/時間/ノード] x 2 [ノード] x 4 [時間/日] x 20 [日] = 7329.28 [円]`

- 参考サイト  
  [サポートされている VM シリーズおよびサイズ](https://docs.microsoft.com/ja-jp/azure/machine-learning/concept-compute-target#supported-vm-series-and-sizes)  
  [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service) 

### 推論クラスター (Azure Kubernetes Service, AKS)
推論用クラスターとして Azure Kubernetes Service の仮想マシン (東日本リージョン、STANDARD_DS12_V2) を 3 ノードで作成し、20 日間使用する場合の月額  

> 注意:  
コア数合計を 12 以上で作成する必要があります。  

→ `51.408 [円/時間/ノード] x 3 [ノード] x 24 [時間/日] x 30 [日] = 111041.28 [円]`

- 参考サイト  
  [Azure Kubernetes Service (AKS) の価格](https://azure.microsoft.com/ja-jp/pricing/details/kubernetes-service/)  
  [料金計算ツール (+Azure Machine Learning)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=machine-learning-service)  

### Azure Container Instance (ACI)
モデルを Azure Container Instance (vCPU 1.8、メモリ 4 GiB) にデプロイし、30 日間使用する場合の月額  

> 注意:  
Azure Machine Learning で ACI にモデルをデプロイする場合、指定したコンテナーに加えて azureml-fe-aci (それぞれ vCPU 0.1、メモリ 0.5 GiB) が作成されます。

(vCPU)  
`0.0015743 [円/秒/vCPU] x 1.9 [vCPU] x 3600 [秒/時間] x 24 [時間/日] x 30 [日] = 7753.11264 [円]`  
   
(メモリ)  
`0.0001721 [円/秒/GiB] x 4.5 [Gib] x 3600 [秒/時間] x 24 [時間/日] x 30 [日] = 2007.3744 [円]`  

→ 合計 `7753.11264 + 2007.3744 = 9760.48704 [円]`

- 参考サイト  
  [Container Instances の価格](https://azure.microsoft.com/ja-jp/pricing/details/container-instances/)  
  [料金計算ツール (+Container Instance)](https://azure.microsoft.com/ja-jp/pricing/calculator/?service=container-instances)  

***
## 見積もりが難しいコストについて
上記に加えて以下リソースの課金が発生いたします。これらはご利用方法によって金額が大きく異なりますため、試算を含めた見積もりを例示することができません。一般的に上述の料金と比較して数パーセント程度の小さい金額となりますため、一定期間ご利用いただいた後、実際の課金額を基に見積もることをお勧めします。  

- [Azure Container Registry Basic アカウント](https://azure.microsoft.com/ja-jp/pricing/details/container-registry/)  
- [Azure ブロック BLOB Storage (汎用 v1)](https://azure.microsoft.com/ja-jp/pricing/details/storage/blobs/)  
- [Key Vault](https://azure.microsoft.com/ja-jp/pricing/details/key-vault/)  

また、ワークスペースやストレージ等を仮想ネットワークに配置する場合、パブリック エンドポイントやプライベート DNS ゾーン、ロード バランサーの料金が追加で発生いたします。固定でかかる費用となりますので、こちらも一定期間ご利用いただい後、実際の課金額を基に見積もることをお勧めします。  

- [仮想ネットワークの分離とプライバシーの概要](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-network-security-overview)

***
## 見積もりの依頼について
弊社より見積もりの回答が必要な場合、営業担当のタスクとして対応しております。下記サイトよりご依頼ください。  

- [Azure 営業担当者に問い合わせる](https://azure.microsoft.com/ja-jp/overview/sales-number/)  
***
`変更履歴`  
`2020/06/18 created by Mochizuki`  
`2020/11/12 modified by Mochizuki`  
`2020/11/18 modified by Mochizuki`  
`2020/11/27 modified by Mochizuki`  

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  