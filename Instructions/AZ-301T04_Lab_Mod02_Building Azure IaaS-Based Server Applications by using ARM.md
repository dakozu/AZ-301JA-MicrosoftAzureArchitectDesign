﻿---
lab:
    title: 'Azure ARM テンプレートと Azure Building Blocks を使用した Azure IaaS ベースのサーバー'
    module: 'モジュール 2: Azure IaaS ベースのサーバー アプリケーション (ADSK) の構築'
---

# Azure IaaS ベースのサーバー アプリケーションの構築。
# ラボの応答キー: Azure リソース マネージャー テンプレートと Azure Building Blocks を使用して Azure IaaS ベースのサーバー アプリケーションの構築。

## 始める前に

1. 次の資格情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認してください:

    - ユーザー名: **Admin**

    - パスワード: **Pa55w.rd**

1. Windows 10 デスクトップの下部にあるタスク バーを確認します。タスク バーには、ラボで使用する共通のアプリケーションのアイコンが含まれています。

    - Microsoft Edge

    - File Explorer

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Windows 上のUbuntu上のバッシュ

    - Windows PowerShell

    > **Note**: >これらのアプリケーションへのショートカットは、 **スタート メニュー** でも検索できます。.


## エクササイズ 1: Azure VM を、Azure Resource Manager テンプレートを PowerShell Desired State Configuration (DSC) 拡張機能と合わせて使用し、Azure Portal からデプロイ。

#### タスク 1: Azure Portal を開

1. タスク バーで、**Microsoft Edge** アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、 **Azure Portal** に移動します(<https://portal.azure.com>)。

1. プロンプトが表示された場合は、このラボで使用する Azure サブスクリプションの所有者役割を持つユーザー アカウントで認証します。

#### タスク 2: Windows Server 2016 Datacenter を実行する Azure VM を作成します。

1. Azure Portal の左上隅にある **リソースの作成** をクリックします。

1. **「新規」** ブレードの上部の **「マーケットプレースの検索」** テキスト ボックスで、 **「Windows Server」** と入力し、**Enter** キーを押します。

1. [**すべて**] のブレードで、検索結果から [**Windows Server**] をクリックします。

1. **Windows Server** ブレードで、**[smalldisk] Windows Server 2016 Datacenter** ソフトウェア プランを選択し、[**作成**] ボタンをクリックします。

1. **基本** タブで、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。
    
    - **リソース グループ** セクションで、テキストボックスの **新規作成** をクリックし、**AADesignLab0301-RG** と入力し、**OK** をクリックします。
      
    - **名前** テキストボックスに、**lab03vm0** の値を入力します。

    - **リージョン** ドロップダウン リストで、このラボでリソースをデプロイする Azure リージョンを選択します。
    
    - **可用性オプション** ドロップダウンリストで、**可用性セット** を選択します。

    - **可用性セット** セクションで、**新規作成** ボックスをクリック、値 **lab03avset0** を入力、**障害ドメイン**と**更新ドメイン** をデフォルト値のままにして、**OK** をクリックします。

    - **イメージ** ドロップダウンリスト内のエントリをデフォルト値に設定したままにします。

    - サイズが **標準 D2s_v3** に設定されていることを確認します

    - **ユーザー名** テキスト ボックスに、**「Student」** の値を入力します。

    - **パスワード** と **パスワードを確認する** テキスト ボックスに、値 **「Pa55w.rd1234」** を入力します。

    - **パブリック インバウンド ポート** セクションで、**選択されたポートを許可** オプションを選択し、**着信ポートの選択** ドロップダウンリストで **HTTP** を選択します。

    - **既に Windows ライセンスを持っていますか** というオプションを **No** に設定したままにしておきます。
    
    - **次: ディスク >** をクリックします。
    
1. **ディスク** タブで、次のタスクを実行します。

    - **OS ディスクタイプ** ドロップダウン リスト エントリが **Premium SSD** に設定されていることを確認します

    - ** 次: ネットワーク >** クリックします。
    
1. **ネットワーク** タブで、次のタスクを実行します。 

    - **仮想ネットワーク** セクションで、 **新規作成** をクリックします。
    
    - **仮想ネットワークの作成** ブレードで、次の設定を指定し、**OK** をクリックします。

        - **名前** テキストボックスに、**lab03vnet0** の値を入力します。

        - **アドレス範囲** テキストボックスに、**10.3.0.0/16** の値を入力します。

        - **サブネット名** テキストボックスに、**subnet-0** の値を入力します。

        - **サブネット アドレス範囲** テキスト ボックスに、**10.3.0.0/24** の値を入力し、**OK** をクリックします。

    - **Public IP** エントリをデフォルト値に設定したままにします。
    
    - **NIC ネットワーク セキュリティ グループ** オプションを **Basic** に設定したままにします。
    
    - **パブリック受信ポート** オプションを **選択したポートを許可する** に。
    
    - **受信ポートを選択** エントリを **HTTP** に設定したままにする。

    - **高速ネットワーク** エントリをデフォルト値に設定したままにします。

    - **次: 管理 >** をクリックします。
    
1. **管理**タブで、次のタスクを実行します。

    - **ブート診断** オプションをデフォルト値に設定したままにします。

    - **OS ゲスト診断** オプションをデフォルト値に設定したままにします。

    - **診断ストレージ　アカウント** エントリをデフォルト値に設定したままにします。
    
    - **システムに割り当てられた管理ID** オプションをデフォルト値に設定したままにします。
    
    - **自動シャットダウンを有効にする** オプションをデフォルト値に設定したままにします。

    - **バックアップを有効にする** オプションをデフォルト値に設定したままにします。

    - **確認および作成** ボタンをクリックします。
    
1. **仮想マシンを作成する** ブレードで、新しい仮想マシンの設定を確認し、**作成** ボタンをクリックします。

1. 最初の仮想マシンがプロビジョニングされるまで待たずに、次のタスクに進みます。


#### タスク 3: DSC 構成 1 を表示します。

1. タスク バーで **ファイル エクスプローラ** アイコンをクリックします。

1. 表示される **ファイル エクスプローラ** ウィンドウで、**\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\** フォルダに移動します。

1. **IISWebServer.zip** ファイルを右クリックし、**すべて展開** オプションを選択します。

1. **圧縮 (Zipped) フォルダーの展開** ダイアログで、次のタスクを実行します。

    - **ファイルがこのフォルダに展開されます:** フィールドで、ファイルを抽出するフォルダの名前を入力します。

    - **完了したら展開されたファイルを表示** チェックボックスを選択してください。
    
    - **展開** ボタンをクリックします。

1. 表示される新しい **ファイル エクスプローラ** ウィンドウで、**IISWebServer.ps1** ファイルを右クリックし、**プログラムから開く** オプションを選択して **Visual Studio Code** アプリケーションを起動します。

1. 表示される **Visual Studio コード** ウィンドウで、PowerShell スクリプトの内容を確認します。

1. **Visual Studio コード** ウィンドウの上部にある **ファイル** メニューをクリックし、**ウィンドウを閉じる**  オプションを選択します。

1. 両方の **ファイル エクスプローラ** ウィンドウを閉じます。

1. **Azure Portal** を開いたまま **Microsoft Edge** ウィンドウに戻ります。


#### タスク 4: Azure ストレージ アカウント 1 を作成する

1. Azure Portal の左上隅にある **リソースの作成** をクリックします。

1. **新規** ブレードの上部にある **マーケットプレイスの検索** テキスト ボックスに **Storage account** と入力し、**Enter** を押します。

1. **すべて** ブレードで、検索結果の **ストレージ アカウント** をクリックします。

1. **ストレージアカウント** ブレードで、**作成** ボタンをクリックします。

1. 次の詳細で、**ストレージ アカウントの作成**  ブレードを完成させます。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。
    
    - **リソース グループ** セクションで、**AADesignLab0301-RG** を選択します。

    - **名前** テキストボックスに、3 - 24の小文字および数字の組み合わせで構成される一意の名前を入力します。
    
    - **Location** エントリは、この演習で前に選択したのと同じ Azure リージョンに設定したままにします。
    
    - **パフォーマンス** セクションで、**standard** オプションが選択されていることを確認します。     

    - **アカウントの種類** ドロップダウンリストで、**ストレージ(汎用v1)** オプションが選択されていることを確認します。
    
    - **レプリケーション** ドロップダウン リストで、**ローカル冗長ストレージ (LRS)** エントリを選択します。

    - **確認及び作成** ボタンをクリックし、**作成** をクリックします。

1. 次のタスクに進む前に、デプロイが完了するのを待ちます。

    > **注記**: この操作には約 2 分かかります。


#### タスク 5: DSC 構成を Azure ストレージ 1 にアップロードします。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

1. **リソース グループ** ブレードで、ストレージ アカウントをデプロイしたリソース グループを表すエントリをクリックします。

1. リソース グループ ブレードで、新しく作成されたストレージ アカウントを表すエントリをクリックします。

1. **概要** 選択を有効にして、ストレージ アカウント ブレードで **コンテナー** をクリックします。

1. ブレードの上部にある **＋コンテナー** をクリックします。

1. 表示される **新規コンテナー** ペインで、次の設定を指定し、**作成** をクリックします。

    - **名前** テキストボックスに、**「config」** という値を入力します。

    - **パブリック アクセス レベル** リストで、**BLOB (BLOB の場合のみの匿名読み取りアクセス)** オプションを選択します。

1. **BLOB サービス** ブレードに戻り、新しい **config** コンテナーを表すエントリをクリックします。

1. **config** ブレードで、ブレードの上部にある **アップロード** ボタンをクリックします。

1. **BLOB のアップロード** ペインで、次のタスクを実行します。

    - **ファイル** フィールドの右側にある青いフォルダ ボタンをクリックします。

    - 表示される **ファイルを開く** ダイアログで、**\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\** フォルダに移動します。

    - **IISWebServer.zip** ファイルを選択します。

    - **開く** ボタンをクリックしてダイアログ ボックスを閉じ、**BLOBのアップロード** ポップアップに戻ります。

    - **アップロード** ボタンをクリックします。

1. **config** ブレードに移動し、**IISWebServer.zip** blob を表すエントリをクリックします。

1. 表示される **BLOB プロパティ** ポップアップで、**URL** プロパティの値を見つけて記録します。この URL は、この演習の後半で使用されます。


#### タスク 6: Azure portal から PowerShell DSC 拡張機能を用いて Azure Resource Manager テンプレートを使用して Azure VM をデプロイします。

1. Azure Portal の左上隅にある **リソースの作成** をクリックします。

1. **新規** ブレードの上部にある **マーケットプレイスの検索** テキスト ボックスで、**Template Deployment** と入力し、**Enter** を押します。

1. **すべて** のブレードで、検索結果から **テンプレートのデプロイ** をクリックします。

1. **テンプレートのデプロイ** ブレードで、**作成** ボタンをクリックします。

1. **カスタムデプロイ** ブレードで、**エディターで独自のテンプレートを作成**をクリックします。

1. **テンプレートの編集** ブレードで、**ファイルの読み込み**をクリックします。

1. **アップロードするファイルを選択** ダイアログ ボックスで、**\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\**フォルダに移動し、**dsc-extension-template.json** ファイルを選択し、**開く**をクリックします。これにより、テンプレート エディタ ペインに次のコンテンツが読み込まれます。

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "virtualMachineName": {
                "type": "string",
                "defaultValue": "lab03vm0"
            },
            "configurationModuleUrl": {
                "type": "string"
            },
            "extensionFunction": {
                "type": "string",
                "defaultValue": "IISWebServer.ps1\\IISWebServer"
            }
        },
        "resources": [
            {
                "apiVersion": "2018-06-01",
                "type": "Microsoft.Compute/virtualMachines/extensions",
                "name": "[concat(parameters('virtualMachineName'), '/dscExtension')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "publisher": "Microsoft.Powershell",
                    "type": "DSC",
                    "typeHandlerVersion": "2.75",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "ModulesUrl": "[parameters('configurationModuleUrl')]",
                        "ConfigurationFunction": "[parameters('extensionFunction')]",
                        "Properties": {
                            "MachineName": "[parameters('virtualMachineName')]"
                        }
                    },
                    "protectedSettings": null
                }
            }
        ]
    }
    ```

1. **保存** ボタンをクリックしてテンプレートを保持します。

1. **カスタムデプロイ** ブレードに戻って、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。

    - **リソースグループ** セクションで、**既存のリソースの使用** オプションを選択し、ドロップダウン リストで、この演習で前に作成したリソース グループを選択します。

    - **ロケーション** ドロップダウン リストをデフォルト値に設定したままにします。

    - **仮想マシン名** フィールドをデフォルト値に設定したままにします: **lab03vm0**.

    - **構成モジュール URL** フィールドに、前のタスクで記録した URL 値を入力します。

    - **拡張機能** フィールドをデフォルト値に設定したままにします: **IISWebServer.ps1\\IISWebServer**。

    - **利用規約** セクションで、**上記の条件に同意する** チェックボックスを選択します。

    - **購入** ボタンを選択します。

1. 次のタスクに進む前に、DSC 構成のデプロイが完了するのを待ちます。

    > **注記**: DSC 構成のデプロイには最大 10 分かかる場合があります。


#### タスク 7: Azure VM が Web コンテンツを提供していることを確認します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

1. **リソース グループ** ブレードで、仮想マシンをデプロイしたリソース グループを表すエントリをクリックします。

1. リソース グループ ブレードで、デプロイした **仮想マシン** を表すエントリをクリックします。

1. **仮想マシン** ブレードで、**パブリック IP アドレス** エントリを見つけて、その値を識別します。

1. 新しい Microsoft Edge タブを開き、前の手順で特定した IP アドレスに移動します。

1. 既定のインターネット インフォメーション サービス Web ページにアクセスできることを確認します。

1. 新しいブラウザ タブを閉じます。

> **復習**: この演習では、Azure Portalから **仮想マシン** をデプロイし、**PowerShell DSC** 拡張機能を使用して、自動実行で仮想マシンに変更を適用しました。


## 演習 2: Azure Portal から PowerShell Desired State Configuration (DSC) 拡張機能を持つ Azure リソース マネージャー テンプレートを使用して、Azure 仮想マシン スケール セット (VMSS) をデプロイします。

#### タスク 1: Azure リソース マネージャー テンプレートを表示する。

1. タスク バーで **ファイル エクスプローラ** アイコンをクリックします。

1. 表示される **ファイル エクスプローラ** ウィンドウで、**\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\** フォルダに移動します。

1. **vmss-template.json** ファイルを右クリックし、**ビジュアル スタジ オコード** アプリケーションを起動するには、**コードで開く** オプションを選択します。

1. 表示される **Visual Studio コード** ウィンドウで、JSON ファイルの内容を確認します。

1. **Visual Studio コード** ウィンドウの上部にある **ファイル** メニューをクリックし、**ウィンドウを閉じる**  オプションを選択します。

1. **ファイル エクスプローラ** ウィンドウを閉じます。

1. **Azure Portal** を開いたまま **Microsoft Edge** ウィンドウに戻ります。


#### タスク 2: ARM を使用して VMSS をデプロイします。

1. Azure Portal のハブ メニューで、**リソースの作成** をクリックします。

1. **新規** ブレードの上部にある **マーケットプレイスの検索** テキスト ボックスで、**Template Deployment** と入力し、**Enter** を押します。

1. **すべて** のブレードで、検索結果から **テンプレートのデプロイ** をクリックします。

1. **テンプレートのデプロイ** ブレードで、**作成** ボタンをクリックします。

1. **カスタムデプロイ** ブレードで、**エディターで独自のテンプレートを作成** をクリックします。

1. **テンプレートを編集** ブレードで、**ファイルを読み込む** をクリックします。

1. 表示される ファイルを **開く** ダイアログで、**\\allfiles\\AZ-301T04\\Module_02\\LabFiles\\Starter\\*** フォルダに移動します。

1. **vmss-template.json** ファイルを選択します。

1. **開く** ボタンをクリックします。

1. **編集テンプレート** ブレードに戻り、**保存** ボタンをクリックしてテンプレートを保持します。

1. **カスタムデプロイ** ブレードに戻って、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。

    - **リソース グループ** セクションで、**新規作成** オプションを選択し、テキストボックスに **AADesignLab0302-RG** と入力します。

    - **場所** エントリをデフォルト値に設定したままにします。

    - *Admin User Name** テキストボックスに、**Student** の値を入力します。

    - **Admin Password** テキストボックスに、値 **「Pa55w.rd1234」** を入力します。

    - **インスタンス カウント** テキストボックスに、**「2」** の値を入力します。

    - **オーバープロビジョニング** テキストボックスをデフォルト値に設定したままにします: **true**。

    - **構成モジュール URL** テキスト ボックスに、この演習の前の演習でアップロードされた BLOB に対して記録した URL を入力します。

    - **利用規約** セクションで、**上記の条件に同意する** チェックボックスを選択します。

    - **購入** ボタンを選択します。

1. 次のタスクに進む前に、デプロイが完了するのを待ちます。


#### タスク 3: VMSS インスタンスが Web コンテンツを提供していることを確認します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

1. **リソース グループ** ブレードで、仮想マシンスケール セットをデプロイしたリソース グループを表すエントリをクリックします。

1. リソース グループ ブレードで、**パブリック IP アドレス** タイプのリソースをクリックします。

1. パブリック IP アドレス リソース ブレードで 、**Essentials** セクションで、**IP アドレス** エントリの値を識別します。

1. 新しい Microsoft Edge タブを開き、前の手順で特定した IP アドレスに移動します。

1. 既定のインターネット インフォメーション サービス Web ページにアクセスできることを確認します。

1. 新しいブラウザ タブを閉じて、**Azure Portal** が現在アクティブな状態でブラウザ タブに戻ります。

> **復習**: この演習では、仮想マシンスケール セットを構成し、PowerShell DSC を使用して個々のインスタンスを構成しました。


## 演習 3: Azure Cloud Shellから PowerShell Desired State Configuration (DSC) 拡張機能を用いて Azure Building Blocks を使用して、Windows Server 2016 および Linux を実行する Azure VM をデプロイします。

#### タスク 1: Cloud Shell を開きます。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

    > **注記**: **Cloud Shell** アイコンは、*より大きい* 文字と *アンダースコア* 文字の組み合わせで構成されるシンボルです。

1. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、初めて使用する **Cloud Shell** を構成するウィザードが表示されます。プロンプトが表示された場合は、**Azure Cloud Shell** ペインで 、**Bash (Linux)** をクリックします。

    > **注記**: **Cloud Shell** の構成オプションが表示されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。その場合は、次のタスクに直接進みます。

1. **マウントされたストレージがありません** のペインで、**詳細設定** を表示し、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。

    - **Cloud Shell リージョン** ドロップダウン リストで、この演習でリソースをデプロイする Azure リージョンの一致またはロケーションの近くを選択します。

    - [**リソース グループ**] セクションで [**新規作成**] オプションが選択されていることを確認し、[**AADesignLab0301-RG**] を選択します。

    - **ストレージ アカウント** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに、3 - 24の小文字および数字の組み合わせで構成される一意の名前を入力します。

    - **ファイル共有** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキストボックスに **cloudshell** と入力します。

    - **ストレージの作成** ボタンを選択します。

1. **Cloud Shell** が初めてセットアップ手順を完了するのを待ってから、次のタスクに進みます。


#### タスク 2: Azure Building Blocks npm パッケージを Azure Cloud Shell 1 にインストールします。

1. ポータルの下部にある **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks npm パッケージをインストールするローカル ディレクトリを作成します。

    ```sh
    mkdir ~/.npm-global
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して npm 構成を更新し、新しいローカル ディレクトリを含めます。

    ```sh
    npm config set prefix '~/.npm-global'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、~./bashrc 設定ファイルを開いて編集します。

    ```sh
    vi ~/.bashrc
    ```

1. **Cloud Shell** コマンドプロンプトで、vi エディタ インターフェイスでファイルの一番下までスク役割し(または **G** と入力)、最後の行の右端の文字(または **$** と入力)、**a** と入力して **INSERT** モードを入力し、**Enter** を押して新しい行を開始します。新しく作成されたディレクトリをシステム パスに追加するには、次のように入力します。

    ```sh
    export PATH="$HOME/.npm-global/bin:$PATH"
    ```

1. **Cloud Shell** コマンド プロンプトで、vi エディタ インターフェイスで変更内容を保存してファイルを閉じるには、**Esc** キーを押し、**:** を押し、**wq!** と入力し、**Enter** を押します。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して Azure Building Blocks npm パッケージをインストールします。

    ```sh
    npm install -g @mspnp/azure-building-blocks
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押してシェルを終了します。

    ```sh
    exit
    ```

1. **Cloud Shell のタイムアウト** ペインで、**再接続** をクリックします。

    > **注記**: 有効にするには、Buliding Blocks npm パッケージをインストールするために Cloud Shell を再起動する必要があります。


#### タスク 3: Azure Building Blocks 1 を使用して、Cloud Shellから Windows Server 2016 Azure VM をデプロイします。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks 参照アーキテクチャ ファイルを含む GitHub リポジトリをダウンロードします。

    ```
    git clone https://github.com/mspnp/reference-architectures.git
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、このデプロイに使用する Azure Building Blocks パラメーター ファイルの内容を表示します。

    ```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" --output tsv | tr -d '"')
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、この演習で前に作成したリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP='AADesignLab0303-RG'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します。

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0301-RG'].location" --output tsv)
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、展開に使用するリソース グループを作成します:

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して **adminUsername** パラメーターのプレースホルダを、Building Blocks パラメーター ファイルの値 **Student** に置き換えます。

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して　**adminPassword** パラメーターのプレースホルダを、Building Blocks パラメーター ファイルの値 **Pa55w.rd1234** に置き換えます。

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、ビルディング パラメーター ファイルでパラメーター値が正常に変更されたことを確認します。

    ```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用して Windows Server 2016 Azure VM をデプロイします。

    ```sh
    azbb -g $RESOURCE_GROUP -s $SUBSCRIPTION_ID -l $LOCATION -p ./reference-architectures/virtual-machines/single-vm/parameters/windows/single-vm.json --deploy
    ```

1. 次のタスクに進む前に、デプロイが完了するのを待ちます。


#### タスク 4: Windows Server 2016 Azure VM が Web コンテンツを提供していることを確認します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

1. **リソース グループ** ブレードで、この演習の前半で Windows Server 2016 データセンター仮想マシンをデプロイしたリソース グループを表すエントリをクリックします。

1. リソース グループ ブレードで、デプロイした仮想マシンを表すエントリをクリックします。

1. **仮想マシン** ブレードで、**パブリック IP アドレス** エントリを見つけて、その値を識別します。

1. 新しい Microsoft Edge タブを開き、前の手順で特定した IP アドレスに移動します。

1. 既定のインターネット インフォメーション サービス Web ページにアクセスできることを確認します。

1. 新しいブラウザ タブを閉じます。


#### タスク 5: Azure Building Blocks を使用して、Cloud Shellから Linux Azure VM をデプロイします。

1.  **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、このデプロイに使用する Azure Building Blocks パラメーター ファイルの内容を表示します。

    ```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、Linux VM へのアクセス時に認証に使用する SSH キーペアを生成します。

    ```sh
    ssh-keygen -t rsa -b 2048
    ```

    - キーを保存するファイルの入力を求めるプロンプトが表示された場合は、**Enter** を押してデフォルト値 **(~/.ssh/id_rsa)** を受け入れます。

    - パスフレーズの入力を求めるメッセージが表示された場合は、**Enter** を 2 回押します。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、新しく生成されたキーペアの公開キーを指定する変数を作成します。

    ```sh
    PUBLIC_KEY=$(cat ~/.ssh/id_rsa.pub)
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、新しく生成されたキーペアの公開キーを指定し、公開キーに含まれる可能性のある特殊文字を考慮する変数を作成します。

    ```sh
    PUBLIC_KEY_REGEX="$(echo $PUBLIC_KEY | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
    ```

    > **注記**: これは、**sed** ユーティリティを使用して、この文字列を Azure Building Blocks パラメーター ファイルに挿入するためです。または、ファイルを開いて、公開キー文字列をファイルに直接入力することもできます。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" --output tsv | tr -d '"')
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用するリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP='AADesignLab0304-RG'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します。

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0301-RG'].location" --output tsv)
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して **adminUsername** パラメーターのプレースホルダを、Building Blocks パラメーター ファイルの値 **Student** に置き換えます。

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して**sshPublicKey** パラメーターのプレースホルダを、Building Blocks パラメーター ファイルの **$PUBLIC_KEY_REGEX** 変数の値に置き換えます。

    ```sh
    sed -i.bak2 's/"sshPublicKey": ""/"sshPublicKey": "'"$PUBLIC_KEY_REGEX"'"/' ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、ビルディング パラメーター ファイルでパラメーター値が正常に変更されたことを確認します。

    ```sh
    cat ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して新しいリソース グループを作成します。

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用して Linux Azure VM をデプロイします。

    ```sh
    azbb -g $RESOURCE_GROUP -s $SUBSCRIPTION_ID -l $LOCATION -p ./reference-architectures/virtual-machines/single-vm/parameters/linux/single-vm.json --deploy
    ```

1. 次のタスクに進む前に、デプロイが完了するのを待ちます。


#### タスク 7: Linux Azure VM が Web コンテンツ 1 を提供していることを確認します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

1. **リソース グループ** ブレードで、仮想マシンをデプロイしたリソース グループを表すエントリをクリックします。

1. リソース グループ ブレードで、デプロイした仮想マシンを表すエントリをクリックします。

1. **仮想マシン** ブレードで、**パブリック IP アドレス** エントリを見つけて、その値を識別します。

1. 新しい Microsoft Edge タブを開き、前の手順で特定した IP アドレスに移動します。

1. 既定のの Apache2 Ubuntu Web ページにアクセスできることを確認します。

1. 新しいブラウザ タブを閉じます。

1. **Cloud Shell** ペインを閉じます。

> **復習**: この演習では、Azure Building Blocks を使用して、Windows Server 2016 データセンターと Linux をCloud Shellから実行する Azure VM をデプロイしました。


## 演習 4: ラボ リソースの削除

#### タスク 1: Cloud Shell を開きます。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、[Cloud Shell] ペインを開きます。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、このラボで作成したすべてのリソース グループを一覧表示します。

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab03')]".name --output tsv
    ```

1. 出力に、この実習ラボで作成したリソース グループのみが含まれていることを確認します。これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループ 1 を削除します。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、このラボで作成したリソース グループを削除します。

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab03')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. ポータルの下部にある **Cloud Shell** プロンプトを閉じます。


> **復習**: この演習では、この演習で使用したリソースを削除しました。
