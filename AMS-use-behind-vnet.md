---
title: 仮想ネットワーク上で Azure Machine Learnining を使用する方法について
date: 2020-10-28 12:00:00
categories:
- Azure Machine Learning
tags:
- 仮想ネットワーク
- Private Link
---
Azure Machine Learning を仮想ネットワーク環境で使用する場合に、参考となる情報を列記いたします。また、具体的な作成方法などを本記事にて紹介させていただきます。  

- [仮想ネットワークの分離とプライバシーの概要](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-network-security-overview)  
- [仮想ネットワークを使用して Azure Machine Learning ワークスペースをセキュリティで保護する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-secure-workspace-vnet)  
- [Azure 仮想ネットワークで Azure Machine Learning Studio を使用する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-enable-studio-virtual-network)  
- [Azure Machine Learning ワークスペース用に Azure Private Link を構成する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-configure-private-link?tabs=azure-resource-manager)  

<!-- more -->
<br>

***
## 作成方法について
ここでは新しく仮想ネットワークを作成し、その配下に各リソースを作成する手順を紹介します。主に以下サイトの手順に従っております。  

- [仮想ネットワークの背後にワークスペースをデプロイする](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-create-workspace-template?wt.mc_id=ignite2020_presentations&tabs=azpowershell#deploy-workspace-behind-a-virtual-network)  

まずは各リソースを作成するリソース グループを作成します。なんらかのリソースの作成に失敗した際に、残存リソースの削除を簡単にするため、新たにリソース グループを作成することをおススメします。リソース基本パラメータおよび実行コマンドは以下の通りです。  

|項目|パラメータ|
|:--|:--|
|リージョン|`eastus` (米国東部)|
|リソースグル―プ|`amlvnetrg`|

```PowerShell
New-AzResourceGroup -Name amlvnetrg -Location eastus
```

次に、以下のような構成で各リソースを作成します。指定可能なパラメータは、テンプレート ファイル 「[201-machine-learning-advanced/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json)」 より参照頂けます。それぞれ用途に応じてカスタイマイズください。  

|項目|パラメータ|仮想ネットワーク化|
|:--|:--|:--|
|リージョン|`eastus` (米国東部)|-|
|リソースグル―プ|`amlvnetrg`|-|
|仮想ネットワーク|`amlvnet`|-|
|サブネットワーク|`amlvsubnet`|-|
|ワークスペース|`amlvnetworkspace`|Private Link|
|ストレージ アカウント|`amlvnetstorage`|Vnet|
|Key Vault|`amlvnetkeyvault`|Vnet|
|Application Insights|`amlvnetappinsights`|- (未サポート)|
|Container Registry|`amlvnetconreg`|- (クォータ拡張要)|

```powershell
New-AzResourceGroupDeployment `
  -Name "amlvnetdeployment" `
  -ResourceGroupName "amlvnetrg" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-machine-learning-advanced/azuredeploy.json" `
  -location "eastus" `
  -vnetOption "new" `
  -vnetName "amlvnet" `
  -subnetName "amlvsubnet" `
  -privateEndpointType "AutoApproval" `
  -workspaceName "amlvnetworkspace" `
  -storageAccountBehindVNet "true" `
  -storageAccountName "amlvnetstorage" `
  -keyVaultName "amlvnetkeyvault" `
  -keyVaultBehindVNet "true" `
  -applicationInsightsName "amlvnetappinsights" `
  -containerRegistryName "amlvnetconreg"
```

>注意点 1 :  
>10/28 現在、Application Insights は仮想ネットワーク背後へのデプロイをサポートしていません。

>注意点 2:  
>Container Registry を仮想ネットワーク背後にデプロイ擦る場合、幾つか[条件](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-secure-workspace-vnet#enable-azure-container-registry-acr)があります。使用したい場合、まず 「[プライベート エンドポイントとプライベート DNS クォータの引き上げ](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-manage-quotas#private-endpoint-and-private-dns-quota-increases)」 に従って、クォータ要求の引き上げをご依頼ください。  
> >次のシナリオでは、場合によっては Microsoft が所有するサブスクリプションでクォータの割り当てを依頼する必要があります。
> >
> >- カスタマーマネージド キー (CMK) を使用する Private Link 対応ワークスペース
> >- 仮想ネットワークの背後にあるワークスペースの Azure Container Registry
> >- Private Link 対応の Azure Kubernetes Service クラスターのワークスペースへのアタッチ 。
>
>クォータ引き上げ後、上述のコマンドの最後の部分を以下の通り変更することで仮想ネットワーク背後へのデプロイを実行することが可能です。  
>
> ```powershell
>   -containerRegistryName "amlvnetconreg" `
>   -containerRegistryBehindVNet "true" `
>   -containerRegistryOption "new" `
>   -containerRegistrySku "Premium"
> ```
>
>Container Registry を含めて仮想ネットワーク背後への配置した状態は、下記イメージのような状態となります。詳細は 「[ワークスペースと関連するリソースをセキュリティで保護する](https://docs.microsoft.com/ja-jp/azure/machine-learning/how-to-network-security-overview#secure-the-workspace-and-associated-resources)」 を参照ください。    
>
><img src="https://docs.microsoft.com/ja-jp/azure/machine-learning/media/how-to-network-security-overview/secure-workspace-resources.png" width=500px align="left" border="1"><br clear="left">

>注意点 3 :  
>Key Vault リソースは一度削除してから同名で作り直すと soft-delete のエラーが発生します。これは、[Key Vault の論理的な削除](https://docs.microsoft.com/ja-jp/azure/key-vault/general/soft-delete-overview)が働いているためです。「[論理的に削除されたキー コンテナーを一覧表示、回復、または消去する](https://docs.microsoft.com/ja-jp/azure/key-vault/general/key-vault-recovery?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault)」 の手順により、完全に削除することが可能です。 
>
> ```powershell
> New-AzResourceGroupDeployment : xx:xx:xx - Resource Microsoft.KeyVault/vaults 'amlvnetkeyvault' failed with message '{
>   "error": {
>     "code": "ConflictError",
>     "message": "Exist soft deleted vault with the same name. "
>   }
> }'
> ```

<br>
※ 順次追加予定です。

***
`変更履歴`  
`2020/10/28 created by Mochizuki`

※ 本記事は 「[jpmlblog について](https://jpmlblog.github.io/blog/2020/01/01/about-jpmlblog/)」 の留意事項に準じます。  
※ 併せて 「[ホームページ](https://jpmlblog.github.io/blog/)」 および 「[記事一覧](https://jpmlblog.github.io/blog/archives/)」 もご参照いただければ幸いです。  