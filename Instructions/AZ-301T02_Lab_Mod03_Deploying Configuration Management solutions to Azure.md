---
lab:
    title: 'Azure への構成管理ソリューションのデプロイ'
    module: 'モジュール 3: Azure ソリューションの監視と自動化'
---

# Azure ソリューションの監視と自動化

# ラボの回答キー: Azure への構成管理ソリューションのデプロイ

## 始める前に

1. 次の認証情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認します。

    - ユーザー名: **Admin**

    - パスワード: **Pa55w.rd**

1. Windows 10 デスクトップの下部にあるタスク バーを確認します。タスク バーには、ラボで使用する一般的なアプリケーションのアイコンがあります。

    - Microsoft Edge

    - ファイル エクスプローラ

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Ubuntu または Windows 上の Bash

    - Windows PowerShell

    > **注意**: これらのアプリケーションへのショートカットは、「**スタート メニュー**」 にあります。

## 演習 1: コンピュティングのリソースをデプロイ

#### タスク 1: Azure Portal を開く

1. タスク バーで、「**Microsoft Edge**」 アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>) に移動します。

1. プロンプトが表示された場合は、このラボで使用する Azure サブスクリプションの所有者役割を持つユーザー アカウントを使用して認証します。

#### タスク 2: Cloud Shell を開く

1. ポータルの上部にある 「**Cloud Shell**」 アイコンをクリックして、新しいシェル インスタンスを開きます。

    > **注意**: 「**Cloud Shell**」 アイコンは、「*より大きい*」 文字と 「*下線*」 文字の組み合わせで構成される記号です。

1. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、初めて使用する **Cloud Shell** を設定するウィザードが表示されます。プロンプトが表示された場合は、「**Azure Cloud Shell へようこそ**」 ペインで、「**Bash (Linux)**」 をクリックします。

    > **注意**: **Cloud Shell** の環境設定オプションが表示されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。その場合は、次のタスクに直接進みます。 

1. 「**ストレージがマウントされていない**」 ペインで、「**詳細設定を表示**」 をクリックし、次のタスクを実行します。

    - 「**サブスクリプション**」 ドロップダウン リスト エントリは、既定値に設定したままにします。

    - 「**Cloud Shell リージョン**」 ドロップダウン リストで、このラボでリソースをデプロイする場所と一致する、またはそれに近い Azure リージョンを選択します。

    - 「**リソース グループ**」 セクションで 「**新規作成**」 オプションを選択し、テキストボックスに 「**AADesignLab1201-RG**」 と入力します。

    - 「**ストレージ アカウント**」 セクションで、「**新規作成**」 オプションが選択されていることを確認し、下のテキストボックスに3 - 24 つの文字と数字の組み合わせで構成される一意の名前を入力します。 

    - 「**ファイル共有**」 セクションで、「**新規作成**」 オプションが選択されていることを確認し、下のテキストボックスに 「**cloudshell**」 と入力します。

    - 「**ストレージを作成**」 ボタンをクリックします。

1. **Cloud Shell** が初回セットアップ手順を完了するのを待ってから、次のタスクに進みます。

#### タスク 3: Linux VM をデプロイ

1. ポータルの上部にある 「Cloud Shell」 アイコンをクリックして、新しい Clould Shell インスタンスを開きます。

1. 「Cloud Shell」 ペインで 「**ファイルをアップロード/ダウンロード**」 アイコンをクリックし、ドロップダウン メニューで 「**アップロード**」 をクリックします。 

1. 「**開く**」 ダイアログボックスで、**\\allfiles\\AZ-301T02\\Module_03\\LabFiles\\Starter\\** フォルダーに移動して、**linux-template.json** ファイルを選択し、「**開く**」 をクリックします。このファイルには、次のテンプレートが含まれています。

```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "userName": {
                "type": "string",
                "defaultValue": "Student"
            },
            "password": {
                "type": "securestring"
            }
        },
        "variables": {
            "vmName": "[concat('lvm', uniqueString(resourceGroup().id))]",
            "nicName": "[concat('nic', uniqueString(resourceGroup().id))]",
            "publicIPAddressName": "[concat('pip', uniqueString(resourceGroup().id))]",
            "virtualNetworkName": "[concat('vnt', uniqueString(resourceGroup().id))]",
            "subnetName": "Linux",
            "imageReference": {
                "publisher": "suse",
                "offer": "opensuse-leap",
                "sku": "42.3",
                "version": "latest"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-06-01",
                "type": "Microsoft.Network/publicIPAddresses",
                "name": "[variables('publicIPAddressName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "publicIPAllocationMethod": "Dynamic"
                }
            },
            {
                "apiVersion": "2017-06-01",
                "type": "Microsoft.Network/virtualNetworks",
                "name": "[variables('virtualNetworkName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "addressSpace": {
                        "addressPrefixes": [
                            "10.0.0.0/16"
                        ]
                    },
                    "subnets": [
                        {
                            "name": "[variables('subnetName')]",
                            "properties": {
                                "addressPrefix": "10.0.0.0/24"
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2017-10-01",
                "type": "Microsoft.Network/networkInterfaces",
                "name": "[variables('nicName')]",
                "location": "[resourceGroup().location]",
                "dependsOn": [
                    "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
                    "[resourceId('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
                ],
                "properties": {
                    "ipConfigurations": [
                        {
                            "name": "ipconfig1",
                            "properties": {
                                "privateIPAllocationMethod": "Dynamic",
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))]"
                                },
                                "subnet": {
                                    "id": "[concat(resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName')), '/subnets/', variables('subnetName'))]"
                                }
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2017-03-30",
                "type": "Microsoft.Compute/virtualMachines",
                "name": "[variables('vmName')]",
                "location": "[resourceGroup().location]",
                "dependsOn": [
                    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
                ],
                "properties": {
                    "hardwareProfile": {
                        "vmSize": "Standard_A1_v2"
                    },
                    "osProfile": {
                        "computerName": "[variables('vmName')]",
                        "adminUsername": "[parameters('username')]",
                        "adminPassword": "[parameters('password')]"
                    },
                    "storageProfile": {
                        "imageReference": "[variables('imageReference')]",
                        "osDisk": {
                            "createOption": "FromImage"
                        }
                    },
                    "networkProfile": {
                        "networkInterfaces": [
                            {
                                "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('nicName'))]"
                            }
                        ]
                    }
                }
            }
        ]
    }
```

1. 「Cloud Shell」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、ハブ仮想ネットワークが含まれるリソース グループの名前を指定する変数を作成します。

```sh
    RESOURCE_GROUP='AADesignLab1202-RG'
```

1. 「**Cloud Shell**」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイに使用する Azure リージョンを指定する変数を作成します (プレースホルダー `<Azure リージョン>` を、このラボでリソースをデプロイする Azure リージョンの名前に置き換えます)。

```sh
    LOCATION='<Azure region>'
```

1. 「**Cloud Shell**」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、新しいリソース グループを作成します。

```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
```

1. 「**Cloud Shell**」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、指定したパラメーター ファイルを使用して Azure Resource Manager テンプレートをデプロイします。

```sh
    az group deployment create --resource-group $RESOURCE_GROUP --template-file ~/linux-template.json --parameters password=Pa55w.rd1234
```

1. デプロイが完了するのを待たずに、次のタスクに進みます。

#### タスク 4: Azure Automation アカウントをデプロイ

1. Azure portal の左上隅にある 「**リソースを作成**」 をクリックします。

1. 「**新規**」 ブレードの上部にある 「**マーケットプレースを検索**」 テキストボックスに 「**Automation**」 と入力し、**Enter** キーを押します。

1. 「**すべて**」 ブレードの検索結果で 「**Automation**」 をクリックします。

1. 「**Automation**」 ブレードで、「**作成**」 をクリックします。

1. 「**Automation アカウントを追加**」 ブレードで、次のタスクを実行します。

    - 「**名前**」 テキストボックスに「**LinuxAutomation**」と入力します。
    
    - 「**サブスクリプション**」 ドロップダウン リスト エントリは、既定値に設定したままにします。

    - 「**リソース グループ**」 セクションで 「**新規作成**」 オプションを選択し、テキストボックスに 「**AADesignLab1203-RG**」 と入力します。

    - 「**場所**」 ドロップダウン リストで、前のタスクで Azure VM をデプロイした場所と一致する、またはそれに近い Azure リージョンを選択します。

    - 「**Azure Run As アカウントを作成**」 セクションで、「**はい**」 オプションが選択されていることを確認します。

    - 「**作成**」 ボタンをクリックします。

1. プロビジョニングが完了するのを待ってから、次のタスクに進みます。 

> **復習**: この演習では、Azure Resource Manager テンプレートを使用して Linux VM を作成し、Azure Portal から Azure Automation アカウントをプロビジョニングしました。


## 演習 2: Azure Automation DSC を設定

#### タスク 1: Linux PowerShell DSC モジュールをインポート

1. Azure portalのハブ メニューで、「**リソース グループ**」 をクリックします。

1. 「**リソース グループ**」 ブレードで、「**AADesignLab1203-RG**」 をクリックします。

1. 「**AADesignLab1203-RG**」 ブレードで、新しく作成された Azure Automation アカウントをクリックします。

1. 「**LinuxAutomation**」 ブレードの左側にある 「**共有リソース**」 セクションで、「**モジュール ギャラリー**」 をクリックします。

1. 「**LinuxAutomation - モジュール ギャラリー**」 ブレードで、次のタスクを実行します。

    - 「**検索**」 テキストボックスに 「**nx**」 と入力し、**Enter** キーを押します。

    - 検索結果で、「**nx**」 モジュールをクリックします。

1. 「**nx**」 ブレードの上部にある 「**インポート**」 ボタンをクリックします。 

1. 「**インポート**」 ブレードで、「**OK**」 ボタンをクリックします。

1. インポート プロセスが完了するのを待ってから、次のタスクに進みます。「**nx モジュール**」 ブレードのステータス メッセージは、モジュールが正常にインポートされたことを示します。

    > **注意**: このプロセスにかかる時間は約 2 分です。

#### タスク 2: Linux DSC 構成を作成

1. 「**LinuxAutomation**」 ブレードに戻ります。

1. 「**LinuxAutomation**」 ブレードに戻って、「**構成管理**」 セクションで 「**State configuration (DSC)**」 をクリックします。

1. 「**LinuxAutomation - State configuration (DSC)**」 ブレードで、「**構成**」 をクリックします。

1. 「**LinuxAutomation - State configuration (DSC)**」 ブレードで、ペインの上部にある 「**+ 追加**」 ボタンをクリックします。

1. 「**インポート**」 ブレードで、次のタスクを実行します。

    - 「**構成ファイル**」 フィールドの横にある、フォルダー アイコンのある青いボタンをクリックします。

    - 「**アップロードするファイルを選択**」 ダイアログボックスで、**\\allfiles\\AZ-301T02\\Module_03\\LabFiles\\Starter\\** フォルダーに移動します。

    - **lampserver.ps1** ファイルを選択します。

    - 「**開く**」 ボタンをクリックしてダイアログボックスを閉じ、「**インポート**」 ブレードに戻ります。

    - 「**名前**」 テキストボックスで、既定のエントリ 「**lampserver**」 をそのまま使用します。

    - 「**説明**」 テキストボックスに 「**LAMP Server configuration using PHP and MySQL (PHP と MySQL を使用した LAMP Server の構成)**」 と入力します。

    - **「OK」** ボタンをクリックします。

1. 「**DSC 構成**」 ペインに戻って、「**更新**」 をクリックした後、新しく作成された 「**lampserver**」 構成をクリックします。

1. 「**lampserver 構成**」 ブレードの上部にある 「**コンパイル**」 ボタンをクリックします。「確認」 ダイアログボックスで、「**はい**」 をクリックして構成のコンパイルを開始します。

1. コンパイル タスクが完了するのを待ちます。コンパイル タスクのステータスを判別するには、「**lampserver 構成**」 ブレードの 「**コンパイル ジョブ**」 セクションの 「**ステータス**」 列を確認してください。

    > **注意**: 最新のコンパイルの状態を確認するには、ブレードを閉じて再度開く必要がある場合があります。このブレードは自動的には更新されません。

#### タスク 3: Linux VM をオンボード

1. 「**LinuxAutomation - State Configuration (DSC)**」 ブレードに戻ります。

1. 「**LinuxAutomation - State Configuration (DSC)**」 ブレードに戻り、「**ノード**」 リンクをクリックします。

1. 「**LinuxAutomation - State configuration (DSC)**」 ブレードで、ペインの上部にある 「**+ 追加**」 ボタンをクリックします。

1. **「仮想マシン」** ブレードで、前の演習でデプロイした Linux 仮想マシンを表すエントリをクリックします。

1. 「仮想マシン」 ブレードで、「**+ 接続**」 をクリックします。

1. 「**登録**」 ブレードで、次のタスクを実行します。

    - 「**登録キー**」 設定は規定値のままにしておきます。

    - 「**ノード構成名**」 ドロップダウン リストで、「**lampserver.localhost**」 エントリを選択します。

    - 残りの設定はすべて既定値のままにしておきます。

    - **「OK」** ボタンをクリックします。

1. 接続プロセスが完了するのを待ってから、次の手順に進みます。

1. 「**LinuxAutomation - State Configuration (DSC)**」 ブレードに戻ります。

1. 「**LinuxAutomation - State configuration (DSC)**」 ブレードの 「**ノード**」 セクションで、前の演習でデプロイした仮想マシンを選択します。

1. 「仮想マシン」 ブレードで、「**ノード構成を割り当てる**」 をクリックします。

1. 「ノード構成を割り当てる」 ブレードで、ノード構成 「**lampserver.host**」 を選択し、「**OK**」 ボタンをクリックします。

1. 「**LinuxAutomation - State Configuration (DSC)**」 ブレードに戻り、「**更新**」 ボタンをクリックします。

1. DSC ノードのリストで、Linux 仮想マシンのステータスが 「**準拠**」 であることを確認します。

> **復習**: この実習では、PowerShell DSC 構成を作成してLinux 仮想マシンに適用しました。


## 演習 3: ラボ リソースの削除

#### タスク 1: Cloud Shell を開く

1. ポータルの上部にある 「Cloud Shell」 アイコンをクリックして、「Cloud Shell」 ペインを開きます。

1. ポータルの下部にある 「Cloud Shell」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このラボで作成したすべてのリソース グループを一覧表示します。

```sh
    az group list --query "[?starts_with(name,'AADesignLab12')]".name --output tsv
```

1. このラボで作成したリソース グループのみが出力に含まれていることを確認します。これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループを削除

1. 「Cloud Shell」 コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このラボで作成したリソース グループを削除します。

```sh
    az group list --query "[?starts_with(name,'AADesignLab12')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
```

1. ポータルの下部にある 「Cloud Shell」 プロンプトを閉じます。


> **復習**: この演習では、このラボで使用したリソースを削除しました。
