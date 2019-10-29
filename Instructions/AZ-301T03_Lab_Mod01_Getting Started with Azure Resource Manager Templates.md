---
lab:
    title: 'Azure Resource Manager テンプレートと Azure Building Blocks の使用を開始する 注意'
    module: 'モジュール 1: Azure Resource Manager を使用したリソースのデプロイ'
---

# Azure Resource Manager を使用してリソースをデプロイする
# ラボの回答キー: Azure Resource Manager テンプレートと Azure Building Blocks の使用を開始する 注意

## を始める前に。

1. 次の資格情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認します:

    - ユーザ名: **Admin**

    - パスワード: **Pa55w.rd**

2. Windows 10 デスクトップの下部にあるタスクバーを確認します。 タスクバーには、ラボで使用する一般的なアプリケーションのアイコンが含まれています:

    - Microsoft Edge

    - File Explorer

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **注記**: これらのアプリケーションへのショートカットは、**スタート メニュー**で見つけることができます。


## エクササイズ 1: Azure Portal から Azure Resource manager テンプレートを使用して、コア Azure リソースをデプロイします。

#### タスク 1: Azure Portal を開きます。

1. タスクバーで、**Microsoft Edge** アイコンをクリックします。

2. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>) に移動します。

3. プロンプトが表示された場合は、このラボで使用する Azureサブスクリプションの所有者役割を持つユーザ アカウントで認証します。

#### タスク 2: Azure Resource Manager テンプレートを使用して、Azure Portal から Azure 仮想ネットワークをデプロイします。

1. Azure Portal の左上隅にある **リソースの作成** をクリックします。

2. **New** ブレードの上部にある **Search the Marketplace** テキスト ボックスで、**Template Deployment** と入力し、**Enter**キーを押します。

3. **Everything** ブレードの、検索結果で **Template deployment** をクリックします。

4. **Template deployment** ブレードで、**作成** ボタンをクリックします。

5. **Custom deployment** ブレードで、**Build your own template in the editor** リンクをクリックします。

6. **Edit template** ブレードで、**Load file** をクリックします。

7. **Choose File to Upload** ダイアログ ボックスで、 **\\allfiles\\AZ-301T03\\Module_01\\Labfiles\\Starter\\**フォルダに移動し、**vnet-simple-template.json** ファイルを選択し、**Open**をクリックします。 これにより、テンプレート エディタ ペインに次のコンテンツが読み込まれます:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "vnetNamePrefix": {
                "type": "string",
                "defaultValue": "vnet-",
                "metadata": {
                    "description": "Name prefix of the vnet"
                 }
            },
            "vnetIPPrefix": {
                "type": "string",
                "defaultValue": "10.2.0.0/16",
                "metadata": {
                    "description": "IP address prefix of the vnet"
                }
            },
            "subnetNamePrefix": {
                "type": "string",
                "defaultValue": "subnet-",
                "metadata": {
                    "description": "Name prefix of the subnets"
                }
            },
            "subnetIPPrefix": {
                "type": "string",
                "defaultValue": "10.2.0.0/24",
                "metadata": {
                    "description": "IP address prefix of the first subnet"
                }
            }
        },
        "variables": {
            "vnetName": "[concat(parameters('vnetNamePrefix'), resourceGroup().name)]",
            "subnetNameSuffix": "0"
        },
        "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[variables('vnetName')]",
            "type": "Microsoft.Network/virtualNetworks",
            "location": "[resourceGroup().location]",
            "scale": null,
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[parameters('vnetIPPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[concat(parameters('subnetNamePrefix'), variables('subnetNameSuffix'))]",
                        "properties": {
                        "addressPrefix": "[parameters('subnetIPPrefix')]"
                        }
                    }
                ],
                "virtualNetworkPeerings": [],
                "enableDdosProtection": false,
                "enableVmProtection": false
            },
            "dependsOn": []
            }
        ]
    }
    ```

8. **保存** ボタンをクリックしてテンプレートを保持します。

9. **Custom deployment** ブレードに戻って、次のタスクを実行します。

    - **Subscription** ドロップダウン リスト エントリ セットをデフォルト値に設定したままにします。

    - **リソース グループ** セクションで、**新規作成** オプションを選択し、テキスト ボックスに **AADesignLab0201-RG** と入力します。

    - **Location** ドロップダウン リストで、このラボでリソースをデプロイする Azure リージョンを選択します。

    - **vnetNamePrefix** テキスト ボックスをデフォルト値に設定したままにします。

    - **vnetIPPrefix** テキスト ボックスをデフォルト値に設定したままにします。

    - **subnetNamePrefix** テキスト ボックスをデフォルト値に設定したままにします。

    - **subnetIPPrefix** テキスト ボックスをデフォルト値に設定したままにします。

    - **利用規約** セクションで、**上記の利用規約に同意する** を選択します。

    - **Purchase** ボタンを選択します。

10. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。


#### タスク 3: デプロイメント メタデータを表示します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

2. **リソース グループ** ブレードで、前のタスクでテンプレートをデプロイしたリソース グループを表すエントリをクリックします。

3. リソース グループ ブレードで、**概要** 選択を有効にした状態で、**Deployment** リンクをクリックします。

4. 結果のブレードで、最新のデプロイメントをクリックして、新しいブレードでそのメタデータを表示します。

5. デプロイメント ブレード内で、**操作の詳細** セクションに表示される情報を確認します。

> **校閲**: このエキササイズでは、Azure Portal の Azure Resource Manager テンプレートを使用して Azure 仮想ネットワークをデプロイしました:



## エクササイズ 2: Azure Cloud Shell から Azure Building Blocks を使用して、コア Azure リソースをデプロイします:

#### タスク 1: Cloud Shell を開

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

    > **注記**: **Cloud Shell** アイコンは、*より大きい* 文字と *アンダースコア* 文字の組み合わせで構成されるシンボルです。

2. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、初めて使用する **Cloud Shell** を設定するウィザードが表示されます。 プロンプトが表示された場合は、**Azure Cloud Shell** ペインで 、**Bash (Linux)** をクリックします。

    > **注記**: **Cloud Shell** の構成オプションが表示されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。 その場合は、次のタスクに直接進みます。

3. **マウントされたストレージがありません** ペインで、**詳細設定を表示** をクリックし、次のタスクを実行します:

    - **Subscription** ドロップダウン リスト エントリ セットをデフォルト値に設定したままにします。

    - **Cloud Shell リージョン** ドロップダウン リストで、Azure リージョンの一致を選択するか、このラボでリソースをデプロイした場所の近くに選択します

    - リソース グループ: **新規作成** オプションが選択されていることを確認し、テキスト ボックスで **AADesignLab0202-RG**と入力します。

    - **ストレージ アカウント** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに、3 - 24 文字と数字の組み合わせで構成される一意の名前を入力します。 

    - **ファイル共有** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに **cloudshell** と入力します。

    - **ストレージの作成** ボタンをクリックします。

4. **Cloud Shell** が初回のセットアップ操作を完了してから、次のタスクに進みます。


#### タスク 2: Azure Building Blocks npm パッケージを Azure Cloud Shell にインストールします。

1. ポータルの下部にある **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Azure Building Blocks npm パッケージをインストールするローカル ディレクトリを作成します:

    ```sh
    mkdir ~/.npm-global
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して npm 構成を更新して、新しいローカル ディレクトリを含めます:

    ```sh
    npm config set prefix '~/.npm-global'
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、~./bashrc 設定ファイルを開いて編集します:

    ```sh
    vi ~/.bashrc
    ```

4. **Cloud Shell** コマンド プロンプトで、vi エディタインターフェイスでファイルの一番下までスク役割し (または **G** と入力)、最後の行の右端の文字 (または **$** と入力)、**a** と入力して **INSERT** モードを入力し、**Enter** キーを押して新しいラインを開始し、新しく作成されたディレクトリをシステム パスに追加するには、次のように入力します:

    ```sh
    export PATH="$HOME/.npm-global/bin:$PATH"
    ```

5. **Cloud Shell** コマンド プロンプトで、vi エディタ インターフェイスで変更内容を保存してファイルを閉じるには、**Esc** キーを押し、**:** を押し、**wq!** と入力し、**Enter** キーを押します。

6. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して Azure Building Blocks npm パッケージをインストールします:

    ```sh
    npm install -g @mspnp/azure-building-blocks
    ```

7. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してシェルを終了します:

    ```sh
    exit
    ```

8. **Cloud Shell のタイム アウト** ペインで、**再接続** をクリックします。 

    > **注記**: Buliding Blocks npm パッケージをインストールし、有効にするには、Cloud Shell を再起動する必要があります。


#### タスク 3: Azure Building Blocks 1 を使用して、Cloud Shell から Azure Virtual Network をデプロイします。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Azure Building Blocks テンプレートを含む GitHub リポジトリをダウンロードします:

    ```sh
    git clone https://github.com/mspnp/template-building-blocks.git
    ```

2.  **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このデプロイに使用する Azure Building Blocks パラメーター ファイルの内容を表示します:

    ```sh
    cat ./template-building-blocks/scenarios/vnet/vnet-simple.json 
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Azure サブスクリプションの名前を指定する変数を作成します:

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このエキササイズで前に作成したリソース グループの名前を指定する変数を作成します:

    ```sh
    RESOURCE_GROUP='AADesignLab0202-RG'
    ```

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイに使用する Azure リージョンを指定する変数を作成します:

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0201-RG'].location" --output tsv)
    ```

6. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Azure Building Blocksを使用して仮想ネットワークをデプロイします:

    ```sh
    azbb -g $RESOURCE_GROUP -s $SUBSCRIPTION_ID -l $LOCATION -p ./template-building-blocks/scenarios/vnet/vnet-simple.json --deploy
    ```

7. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。


#### タスク 4: デプロイメント メタデータ を表示します。

1. ポータルの左側で、**リソースグループ** リンクをクリックします。

2. **リソース グループ** ブレードで、このエキササイズで前に作成したリソース グループを表すエントリをクリックします。

3. リソース グループ ブレードで、**概要** 選択を有効にした状態で、**Deployment** リンクをクリックします。

4. 結果のブレードで、最新のデプロイメントをクリックして、新しいブレードでそのメタデータを表示します。

5. デプロイメント ブレード内で、**操作の詳細** セクションに表示される情報を確認します。

6. **Cloud Shell** ペインを閉じます。

> **校閲**: このエキササイズでは、Azure Portal の Azure Resource Manager テンプレートを使用して Azure 仮想ネットワークをデプロイしました:


## エクササイズ 3: ラボ リソースの削除

#### タスク 1: Cloud Shell を開

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、Cloud Shell ペインを開きます。

2. ポータルの下部にある **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このラボで作成したすべてのリソース グループを一覧表示します:

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab02')]".name --output tsv
    ```

3. 出力に、この実習ラボで作成したリソース グループのみが含まれていることを確認します。 これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループ を削除します。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、押して、**Enter**このラボで作成したリソース グループを削除します :

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab02')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

2. ポータルの下部にある **Cloud Shell** プロンプトを閉じます。


> **校閲**: このエキササイズでは、このラボで使用したリソースを削除しました。
