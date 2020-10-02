---
title: トラブルシューティング
description: Azure Stack HCI 上の Azure Kubernetes Service に関するトラブルシューティングのためのガイド
author: davannaw-msft
ms.topic: how-to
ms.date: 09/22/2020
ms.author: dawhite
ms.openlocfilehash: 312431b1ae36debc79b1a9bd3a874b648dfc9cd9
ms.sourcegitcommit: dabbe44c3208fbf989b7615301833929f50390ff
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2020
ms.locfileid: "90949346"
---
# <a name="troubleshooting-azure-kubernetes-service-on-azure-stack-hci"></a>Azure Stack HCI 上の Azure Kubernetes Service に関するトラブルシューティング

Azure Stack HCI 上の Azure Kubernetes Service を使用して Kubernetes クラスターを作成または管理するときに、問題が発生することがあります。 以下に、これらの問題の解決に役立つトラブルシューティングのガイドラインを示します。 

## <a name="troubleshooting-azure-stack-hci"></a>Azure Stack HCI のトラブルシューティング
Azure Stack HCI クラスター内の複数のサーバーにわたる、ネットワークとストレージの QoS (サービスの品質) 設定に関するクラスター検証レポートについてトラブルシューティングを行い、重要なルールが定義されていることを確認するには、「[クラスター検証レポートのトラブルシューティング](/azure-stack/hci/manage/validate-qos)」を参照してください。

CredSSP に関する問題をトラブルシューティングするには、「[CredSSP のトラブルシューティング](/azure-stack/hci/manage/troubleshoot-credssp)」を参照してください。

## <a name="troubleshooting-windows-admin-center"></a>Windows Admin Center のトラブルシューティング
この製品は現在、パブリック プレビューの状態にあります。これは、製品がまだ開発中であることを意味します。 現時点では、Windows Admin Center の Azure Kubernetes Service 拡張機能にはいくつかの問題があります。 
* 現時点では、Azure Stack HCI 上の Azure Kubernetes Service をセットアップするために使用するシステムのクラスター内の各サーバーは、信頼されたサーバーである必要があります。 これは、Windows Admin Center はクラスター内の各サーバーに対して、1 つだけでなく、いくつかの CredSSP 操作を実行できる必要があることを意味します。 
* `msft.sme.aks couldn't load` というエラーが発生し、チャンクの読み込みに失敗したとこのエラーで表示された場合は、最新バージョンの Edge または Google Chrome を使用してもう一度試してください。
* Azure Kubernetes Service ホストのセットアップ ウィザードまたは Kubernetes クラスターの作成ウィザードのいずれかを開始する前に、Windows Admin Center を通して Azure にサインインする必要があります。 ワークフローの間に再署名が必要な場合があります。 Windows Admin Center を通した Azure へのサインインで問題が生じている場合は、[Azure portal](https://portal.azure.com/) などの別の操作元から Azure アカウントへのサインインを試みます。 引き続き問題が発生する場合は、サポートに連絡する前に [Windows Admin Center の既知の問題](/windows-server/manage/windows-admin-center/support/known-issues)に関する記事を確認してください。
* Windows Admin Center に表示された Azure Stack HCI デプロイ上の Azure Kubernetes Service の現在のイテレーションでは、Azure Kubernetes Service ホストをセットアップしたユーザーのみがシステムに Kubernetes クラスターを作成できます。 この問題を回避するには、Azure Kubernetes Service ホストをセットアップしたユーザー プロファイルから、新しい Kubernetes クラスターを起動することになるユーザー プロファイルに、`.wssd` フォルダーをコピーします。
* いずれかのウィザードで誤った構成に関するエラーが発生する場合は、クラスターのクリーンアップ操作を実行します。 これにより、`C:\Program Files\AksHci\mocctl.exe` ファイルが削除されることがあります。
* クラスターの作成ウィザード内で CredSSP を正常に機能させるには、同じアカウントによって Windows Admin Center のインストールと使用を行う必要があります。 1 つのアカウントでインストールした後、別のアカウントで使おうとするとエラーが発生します。
* クラスターのデプロイ中に、helm.zip ファイルの転送に関する問題が発生することがあります。 これは多くの場合、helm.zip ファイルへのパスが存在しないか無効であることを通知するエラーになります。 この問題を解決するには、元に戻り、デプロイを再試行してください。
* 長時間にわたってデプロイが停止する場合は、CredSSP または接続の問題が発生している可能性があります。 以下の手順を試してデプロイのトラブルシューティングを行ってください。 
    1.  WAC を実行しているマシンの PowerShell ウィンドウで、次のコマンドを実行します。 
    ```PowerShell
    Enter-PSSession <servername>
    ```
    2.  このコマンドが成功する場合は、サーバーに接続でき、接続に関する問題はありません。
    
    3.  CredSSP の問題が発生している場合は、次のコマンドを実行して、ゲートウェイ マシンとターゲット マシンの間の信頼をテストします。 
    ```PowerShell
    Enter-PSSession –ComputerName <server> –Credential company\administrator –Authentication CredSSP
    ``` 
    次のコマンドを実行して、ローカル ゲートウェイにアクセスする際の信頼をテストすることもできます。 
    ```PowerShell
    Enter-PSSession -computer localhost -credential (Get-Credential)
    ``` 
* Azure Arc を使用していて、複数のテナント ID がある場合は、次のコマンドを実行して、デプロイの前に目的のテナントを指定します。 そのようにしないと、デプロイの失敗が起きる可能性があります。

```Azure CLI
az login –tenant <tenant>
```
* 新しい Azure アカウントを作成したばかりで、ゲートウェイ マシン上のアカウントにログインしていない場合は、WAC ゲートウェイを Azure に登録する際に問題が発生することがあります。 この問題を軽減するには、ブラウザーの別のタブまたはウィンドウで Azure アカウントにサインインしてから、WAC ゲートウェイを Azure に登録します。

### <a name="creating-windows-admin-center-logs"></a>Windows Admin Center ログの作成
Windows Admin Center で問題を報告するときには、開発チームが問題を診断できるようにログを添付すると助けになります。 Windows Admin Center のエラーは、通常、2 つの形で示されます。Windows Admin Center を実行しているマシンのイベント ビューアーに表示されるイベント、またはブラウザー コンソールに表示される JavaScript の問題です。 Windows Admin Center のログを収集するには、パブリック プレビュー パッケージで提供されている `Get-SMEUILogs.ps1` スクリプトを使用します。 
 
このスクリプトを使用するには、スクリプトが格納されているファイル ディレクトリで次のコマンドを実行します。 
 
```PowerShell
./Get-SMEUILogs.ps1 -ComputerNames [comp1, comp2, etc.] -Destination [comp3] -HoursAgo [48] -NoCredentialPrompt
```
 
コマンドには以下のパラメーターがあります。
 
* **-ComputerNames**:ログの収集対象とするマシンの一覧。
* **-Destination**:ログの集計先のマシン。
* **-HoursAgo**:このパラメーターでは、スクリプト実行時間の何時間前からログを収集するかを定義します。
* **-NoCredentialPrompt**:これは、資格情報のプロンプトをオフにし、現在の環境における既定の資格情報を使用するためのスイッチです。
 
このスクリプトの実行に問題がある場合は、次のコマンドを実行してヘルプ テキストを表示できます。 
 
```PowerShell
GetHelp .\Get-SMEUILogs.ps1 -Examples
```

## <a name="troubleshooting-windows-worker-nodes"></a>Windows ワーカー ノードのトラブルシューティング 
Windows ワーカー ノードにログインするには、まず、`kubectl get` を実行してノードの IP アドレスを取得し、`EXTERNAL-IP` の値を書き留めます。

```PowerShell
kubectl get nodes -o wide
``` 
`ssh Administrator@ip` を使用して、SSH でノードに接続します。 SSH でノードに接続したら、`net user administrator *` を実行して管理者のパスワードを更新できます。 

## <a name="troubleshooting-linux-worker-nodes"></a>Linux ワーカー ノードのトラブルシューティング 
Linux ワーカー ノードにログインするには、まず、`kubectl get` を実行してノードの IP アドレスを取得し、`EXTERNAL-IP` の値を書き留めます。

```PowerShell
kubectl get nodes -o wide
``` 
`ssh clouduser@ip` を使用して、SSH でノードに接続します。 

## <a name="troubleshooting-azure-arc-for-kubernetes"></a>Azure Arc for Kubernetes のトラブルシューティング
接続、アクセス許可、および Arc エージェントに関連するいくつかの一般的なシナリオのトラブルシューティングを行うには、[Azure Arc 対応 Kubernetes のトラブルシューティング](/azure/azure-arc/kubernetes/troubleshooting)に関するページを参照してください。

## <a name="next-steps"></a>次の手順
Azure Stack HCI で Azure Kubernetes Service を使用している間に問題が引き続き発生する場合は、[GitHub](https://aka.ms/aks-hci-issues) を通してバグを報告してください。  
