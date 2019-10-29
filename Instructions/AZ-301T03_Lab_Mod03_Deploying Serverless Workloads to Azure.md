---
lab:
    title: 'サーバーレス ワークロードを Azure にデプロイする'
    module: 'モジュール 3: Azure でのサーバーレス アプリケーションの作成'
---

# Azure でのサーバーレス アプリケーションの作成

# ラボの回答キー: サーバーレス ワークロードを Azure にデプロイする

## 始める前に

1. 次の資格情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認します:

    - ユーザ名：**Admin**

    - パスワード: **Pa55w.rd**

1. Windows 10 デスクトップの下部にあるタスクバーを確認します。タスクバーには、ラボで使用する一般的なアプリケーションのアイコンが含まれています:

    - Microsoft Edge

    - ファイル エクスプローラ

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Windows上でのUbuntu上のバッシュ

    - Windows PowerShell で

    > **注記**: これらのアプリケーションへのショートカットは、 **スタート メニュー** で見つけることができます。

## 演習 1: Web アプリを作成します

#### タスク 1: Azure Portal を開く

1. タスクバーで、**Microsoft Edge**アイコンをクリックします。

2. 開いているブラウザ ウィンドウで、**Azure Portal**(<https://portal.azure.com>)に移動します。

3. プロンプトが表示された場合は、このラボで使用する Azureサブスクリプションの所有者役割を持つユーザ アカウントで認証します。

#### タスク 2: Azure Cloud Shell

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。 

    > **注記**: **Cloud Shell** アイコンは、 *より大きい* *文字とアンダースコア*文字の組み合わせで構成されるシンボルです。

2. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、 初めて使用する **Cloud Shell** を構成するウィザードが表示されます。プロンプトが表示された場合は、**Azure Cloud Shell へようこそ** ペインで、**Bash (Linux)** をクリックします。

    > **注記**: **Cloud Shell** の構成オプションが表示されない場合 は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。その場合は、次のタスクに直接進みます。 

3. **マウントされたストレージがありません** ペインで、**詳細設定を表示** をクリックし、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリは、既定値に設定したままにします。

    - **Cloud Shell リージョン** ドロップダウン リストで、Azure リージョンの一致、またはこのラボでリソースをデプロイする場所の近くを選択します

    - **リソース グループ** セクションで **新規作成** オプションが選択されていることを確認し、下のテキスト ボックス で、**AADesignLab0901-RG** と入力します。

    - **ストレージ アカウント** セクションで **新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに、3 - 24 字の、文字と数字の組み合わせで構成されるユニークな名前を入力します。 

    - **ファイル共有** セクションで **新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに **cloudshell**を入力します。

    - **ストレージの作成** ボタンをクリックします。

4. **Cloud Shell** が初回セットアップ操作が完了するのを待ってから、次のタスクに進みます。

#### タスク 3: App Service プランの作成

1. ポータルの下部にある **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、このエキササイズで使用するリソース グループの名前を指定する変数を作成します:

    ```sh
    RESOURCE_GROUP_APP='AADesignLab0502-RG'
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイメントに使用する Azure リージョンを指定する変数を作成します (プロンプトが表示されたときはリージョンの名前を入力します):

    ```sh
    read -p 'Region: ' LOCATION
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してリソース グループを作成します:

    ```sh
    az group create --name $RESOURCE_GROUP_APP --location $LOCATION
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して新しい App Service プランを作成します:

    ```sh
    az appservice plan create --is-linux --name "AADesignLab0502-$LOCATION" --resource-group $RESOURCE_GROUP_APP --location $LOCATION --sku B2
    ```

#### タスク 4: Web App インスタンスの作成

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Linux ベースの App Service Web アプリ インスタンスで実行時間の可能性のある一覧を表示します: 

    ```sh
    az webapp list-runtimes --linux
    ``` 

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、新しい Web アプリの名前として使用するランダムに生成された文字列である新しい変数を作成します:

    ```sh
    WEBAPPNAME1=webapp05021$RANDOM$RANDOM
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してユニークな名前を使用して新しい Web アプリを作成します:

    ```sh
    az webapp create --name $WEBAPPNAME1 --plan AADesignLab0502-$LOCATION --resource-group $RESOURCE_GROUP_APP --runtime "DOTNETCORE|2.1"
    ```

    > **注記**: Web アプリ名の重複が原因でコマンドが失敗した場合は、コマンドが正常に完了するまで、最後の 2 つの操作を再実行します

4. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。

#### タスク 5: デプロイメント結果を表示します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

2. **Resource groups** ブレードで、**AADesignLab0502-RG** をクリックします。

3. **AADesignLab0502-RG** ブレードで、このエキササイズで前に作成した Azure Web アプリを表すエントリをクリックします。

4. Web アプリ ブレードで、 ブレードの上部にある **参照** ボタンをクリックします。

5. Azure App サービスによって生成された既定のページを確認します。

6. 新しいブラウザ タブを閉じて、Azure Portal を表示するブラウザ タブに戻ります。

> **校閲**: このエキササイズでは、空白の Web アプリを含む Linux ベースのApp Service プランを作成しました。 


## 演習 2: Webアプリ コードのデプロイ

#### タスク 1: Azure Resource Manager テンプレートと GitHub を使用して Web アプリ拡張機能を使用してコードをデプロイする

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このエキササイズで使用するリソース グループの名前を指定する変数を作成します:  

    ```sh
    RESOURCE_GROUP_APP='AADesignLab0502-RG'
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、デプロイメントに使用する Azure リージョンを指定する変数を作成します:

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0502-RG'」.location" --output tsv)
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、新しい Web アプリの名前として使用するランダムに生成された文字列である新しい変数を作成します:

    ```sh
    WEBAPPNAME2=webapp05022$RANDOM$RANDOM
    ```

5. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してユニークな名前を使用して新しい Web アプリを作成します:

    ```sh
    az webapp create --name $WEBAPPNAME2 --plan AADesignLab0502-$LOCATION --resource-group $RESOURCE_GROUP_APP --runtime "NODE|9.4"
    ```

    > **注記**: Web アプリ名の重複が原因でコマンドが失敗した場合は、コマンドが正常に完了するまで、最後の 2 つの操作を再実行します


6. **Cloud Shell** ウィンドウで **ファイルのアップロード/ダウンロード** アイコンをクリックし、ドロップダウンメニューで **アップロード** をクリックします。 

7. **Open** ダイアログ ボックスで、 **\\allfiles\\AZ-301T03\\Module_03\\Labfiles\\Starter\\** フォルダに移動し、**github.json** ファイルを選択し、**Open** をクリックします。このファイルには、次の Azure Resource Managerテンプレートが含まれています:

    ```json
    {
        "$schema": "http://schemas.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "webAppName": {
                "type": "string"
            },
            "repositoryUrl": {
                "type": "string"
            },
            "branch": {
                "type": "string",
                "defaultValue": "master"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/sites",
                "name": "[parameters('webAppName')]",
                "location": "[resourceGroup().location]",
                "properties": {},
                "resources": [
                    {
                        "apiVersion": "2015-08-01",
                        "name": "web",
                        "type": "sourcecontrols",
                        "dependsOn": [
                            "[resourceId('Microsoft.Web/Sites', parameters('webAppName'))]"
                        ],
                        "properties": {
                            "RepoUrl": "[parameters('repositoryUrl')]",
                            "branch": "[parameters('branch')]",
                            "IsManualIntegration": true
                        }
                    }
                ]
            }
        ]
    }
    ```

8. **Cloud Shell** ウィンドウで **ファイルのアップロード/ダウンロード** アイコンをクリックし、ドロップダウンメニューで **アップロード** をクリックします。 

9. **Open** ダイアログ ボックスで、**\\allfiles\\AZ-301T03\\Module_03\\Labfiles\\Starter\\** フォルダに移動し、**parameters.json** ファイルを選択 し、 **Open** をクリックします。このファイルには、以前にアップロードした Azure Resource Manager のテンプレートの次のパラメーターが含まれています:

    ```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "webAppName": {
          "value": "$WEBAPPNAME2"
        },
        "repositoryUrl": {		
          "value": "$REPOSITORY_URL"
        },
        "branch": {
          "value": "master"
        }
      }
    }
    ```

10. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Web アプリ コードをホストする GitHub リポジトリの名前を指定する変数を作成します:

    ```sh
    REPOSITORY_URL='https://github.com/Azure-Samples/nodejs-docs-hello-world'
    ```

11. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、Web アプリ コードをホストする GitHub リポジトリの名前を指定する変数を作成し、URL 含む可能性のある特別な文字を考慮する変数を作成します:

    ```sh
    REPOSITORY_URL_REGEX="$(echo $REPOSITORY_URL | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
    ```

    > **注記**: これは、**sed** ユーティリティを使用して、この文字列を Azure Resource Manager のテンプレート パラメーター ファイルに挿入するために必要です。または、ファイルを開いて URL 文字列をファイルに直接入力することもできます。

12. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、**webAppName** パラメーターの値のプレースホルダを パラメーター ファイル内の  **$WEBAPPNAME2** 変数の値に置き換えます:

    ```sh
    sed -i.bak1 's/"$WEBAPPNAME2"/"'"$WEBAPPNAME2"'"/' ~/parameters.json
    ```

13. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、**repositoryUrl** パラメーターの値のプレースホルダを,パラメーター ファイルの  **$REPOSITORY_URL** 変数の値に置き換えます:

    ```sh
    sed -i.bak2 's/"$REPOSITORY_URL"/"'"$REPOSITORY_URL_REGEX"'"/' ~/parameters.json
    ```

14. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、プレースホルダがパラメーター ファイルで正常に置き換えられたことを確認します:

    ```sh
    cat ~/parameters.json
    ```

15. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キー を押して、ローカルの Azure Resource Manager のテンプレートとローカル パラメーター ファイルを使用して、GitHub 常駐 Web アプリ コードをデプロイします:

    ```sh
    az group deployment create --resource-group $RESOURCE_GROUP_APP --template-file github.json --parameters @parameters.json
    ```

16. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。

    > **注記**: デプロイメントには約 1 分かかります。

#### タスク 2: デプロイメント結果を表示します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

2. **Resource groups** ブレードで、 **AADesignLab0502-RG** をクリックします。

3. **AADesignLab0502-RG** ブレードで、前のタスクで作成した Azure Web アプリを表すエントリをクリックします。

4. Web アプリ ブレードで、 ブレードの上部にある **参照** ボタンをクリックします。

5. GitHub からデプロイされた Node.js Web アプリケーションのサンプルを確認します。

6. 新しいブラウザ タブを閉じて、Azure Portal を表示するブラウザ タブに戻ります。

#### タスク 3: Docker Hub コンテナ イメージを使用してコードをデプロイする

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このタスクで使用するリソース グループの名前を指定する変数を作成します:

    ```sh
    RESOURCE_GROUP_CONTAINER='AADesignLab0502-RG'
    ```

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、デプロイメントに使用する Azure リージョンを指定する変数を作成します:

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0502-RG'」.location" --output tsv)
    ```

3. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、 **Enter** キーを押して、新しい Web アプリの名前として使用するランダムに生成された文字列である新しい変数を作成します:

    ```sh
    WEBAPPNAME3=webapp05023$RANDOM$RANDOM
    ```

4. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してユニークな名前を使用して新しい Web アプリを作成します:

    ```sh
    az webapp create --name $WEBAPPNAME3 --plan AADesignLab0502-$LOCATION --resource-group $RESOURCE_GROUP_CONTAINER --deployment-container-image ghost
    ```

    > **注記**: Web アプリ名の重複が原因でコマンドが失敗した場合は、コマンドが正常に完了するまで、最後の 2 つの操作を再実行します

5. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。

    > **注記**: 通常 1 分未満でメッセージが表示されます。

6. **Cloud Shell** ペインを閉じます。


#### タスク 4: デプロイメント結果を表示します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

2. **Resource groups** ブレードで、 **AADesignLab0502-RG** をクリックします。

3. **AADesignLab0502-RG** ブレードで、前のタスクで作成した Azure Web アプリを表すエントリをクリックします。

4. Web アプリ ブレードで、 ブレードの上部にある **参照** ボタンをクリックします。

    > **注記**: アプリケーションが表示されない場合は、Web アプリ ブレードに切り替え、ブレードの上部にある **再起動** ボタンをクリックし、もう一度 **参照** をクリックします。 

5. Docker Hub からデプロイされたブログ アプリケーションを確認します。 

6. 新しいブラウザ タブを閉じて、Azure Portal を表示するブラウザ タブに戻ります。

> **校閲**: このエキササイズでは、Azure Resource Manager のテンプレートと Docker Hub イメージを使用して、アプリ サービス Web アプリにコードをデプロイしました。


## 演習 3: Function App のデプロイ

#### タスク 1: Azure Resource Manager のテンプレートを使用してコードを使用して Function App をデプロイする

1. Azure Portal の左上隅にある **リソースの作成** をクリック します。

2. **New** ブレードの上部の **マーケットプレースの検索** テキスト ボックスで、**テンプレートのデプロイメント** と入力し、**Enter** を押します。

3. **Everything** ブレードの、検索結果で **テンプレートのデプロイメント** をクリックします。

4. **Template deployment** ブレードで、 **作成** ボタンをクリックします。

5. **Custom deployment** ブレードで、**エディターで独自のテンプレートを作成** リンクをクリックします。

6. **テンプレートの編集** ブレードで、**クイックスタート テンプレート** リンクをクリックします。

7. **クイック スタート テンプレートを読み込む** ペインで、 **テンプレートの選択** ドロップダウン リストで、**201-function-app-dedicated-github-deploy** テンプレートを選択します。

8. **OK** ボタンをクリックします。

9. **テンプレートの編集** ブレードに戻 り、**保存** ボタンをクリックしてテンプレートを保持します。

10. **Custom deployment** ブレードに戻り、次のタスクを実行します:

    - **サブスクリプション** ドロップダウン リスト エントリは、既定値に設定したままにします。

    - **リソース グループ** セクションで、 **新規作成** オプションが選択されていることを確認し、下のテキスト ボックスで **AADesignLab0503-RG** と入力します。

    - **アプリ名** テキスト ボックスに、新しい Function App のユニークな名前を入力します。

    - **Sku** ドロップダウン リストで、**Basic** オプションを選択します。

    - **ワーカー サイズ** ドロップダウン リストは既定値に設定したままにします。

    - **ストレージ アカウントの種類** ドロップダウン リストは、既定値に設定したままにします。

    - **Repo URL** フィールドは既定値のままにします。

    - **Branch** テキスト ボックスは既定値のままにします。

    - **Location** テキスト ボックスは既定値のままにします。

    - **利用規約** セクションで、 **上記の利用規約に同意する** を選択します。

    - **購入** ボタンをクリックします。

11. 次のタスクに進む前に、デプロイメントが完了するのを待ちます。

    > **注記**: デプロイメントには約 1 分かかります。


#### タスク 2: デプロイメント結果を表示します。

1. Azure Portal のハブ メニューで、**リソース グループ** をクリックします。

2. **Resource groups** ブレードで、 **AADesignLab0502-RG** をクリックします。

3. **AADesignLab0503-RG** ブレードで、前のタスクで作成した Function App を表すエントリをクリックします。

4. Function App ブレードで **Url** エントリを検索し、下のハイパーリンクをクリックして、新しいブラウザ タブで Function App のランディング ページが表示します。

5. 新しいブラウザ タブを閉じて、Azure Portal を表示するブラウザ タブに戻ります。

> **校閲**: このエキササイズでは、Azure Resource Manager のテンプレートを使用して Function App とコードをデプロイしました。


## 演習 4: ラボ リソースの削除

#### タスク 1: Azure Cloud Shell

1. ポータルの上部にある **Cloud Shell** アイコンをクリックして、Cloud Shell ペインを開きます。

2. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押して、このラボで作成したすべてのリソース グループを一覧表示します:

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab05')]".name --output tsv
    ```

3. 出力に、この実習ラボで作成したリソース グループのみが含まれていることを確認します。これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループの削除

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** キーを押してこのラボで作成したリソース グループを削除します:

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab05')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

2. ポータルの下部にある **Cloud Shell** プロンプトを閉じます。


> **校閲**: このエキササイズでは、このラボで使用したリソースを削除しました。
