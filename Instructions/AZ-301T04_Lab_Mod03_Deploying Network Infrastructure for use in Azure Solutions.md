---
lab:
    title: 'Azure ソリューションで使用するネットワーク インフラストラクチャのデプロイ'
    module: 'モジュール 3: Azure アプリケーション コンポーネントのネットワーク'
---

# ネットワーク Azure アプリケーション コンポーネント 

# ラボの回答キー: Azure ソリューションで使用するネットワーク インフラストラクチャのデプロイ。

## を開始する前に、

1. 次の資格情報を使用して Windows 10 ラボ仮想マシンにログインしていることを確認します。

    - ユーザー名: **Admin**

    - パスワード: **Pa55w.rd**

1. Windows 10 デスクトップの下部にあるタスク バーを確認します。 タスク バーには、ラボで使用する一般的なアプリケーションのアイコンが含まれています。

    - Microsoft Edge

    - File Explorer

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **注記**: これらのアプリケーションへのショートカットは、**スタートメニュー** で見つけることができます。

## 演習 1: ラボ環境の構成

#### タスク 1: Azure Portal を開きます。

1. タスク バーで、**Microsoft Edge** アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>)に移動します。

1. プロンプトが表示された場合は、このラボで使用する Azure サブスクリプションの所有者役割を持つユーザー アカウントを使用して認証します。

#### タスク 2: Cloud Shell を開きます。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

    > **注記**: **Cloud Shell** アイコンは、*より大きい* 文字と *アンダースコア* 文字の組み合わせで構成されるシンボルです。

1. サブスクリプションを使用して **Cloud Shell** を初めて開く場合は、初めて使用する **Cloud Shell** を構成するウィザードが表示されます。 プロンプトが表示された場合は、**Azure Cloud Shell** ペインで 、**Bash (Linux)** をクリックします。

    > **注記**: **Cloud Shell** の構成オプションが表示されない場合は、このコースのラボで既存のサブスクリプションを使用している可能性が高いと考えられます。 その場合は、次のタスクに直接進みます。 

1. **マウントされたストレージがありません** のペインで、**詳細設定** を表示し、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。

    - **Cloud Shell リージョン** ドロップダウン リストで、Azure リージョンの一致を選択するか、このラボでリソースをデプロイするロケーションの近くを選択します

    - **リソース グループ** セクションで、**既存のリソースの使用** オプションを選択し、ドロップダウン リストで **AADesignLab0901-RG** を選択します。

    - **ストレージ アカウント** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキスト ボックスに、3 - 24の小文字および数字の組み合わせで構成される一意の名前を入力します。 

    - **ファイル共有** セクションで、**新規作成** オプションが選択されていることを確認し、下のテキストボックスに **cloudshell** と入力します。

    - **ストレージの作成** ボタンを選択します。

1. **Cloud Shell** が初めてセットアップ手順を完了するのを待ってから、次のタスクに進みます。

#### タスク 3: Azure Building Blocks npm パッケージを Azure Cloud Shell にインストールします。

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

1. **Cloud Shell** コマンド プロンプトで、vi エディタ インターフェイスでファイルの一番下までスク役割し(または **G** と入力)、最後の行の右端の文字(または **「$」** と入力)、**a** と入力して **INSERT** モードを開始し、**Enter** を押して開始します。新しい行を入力し、次のように入力して、新しく作成したディレクトリをシステム パスに追加します。

    ```sh
    export PATH="$HOME/.npm-global/bin:$PATH"
    ```

1. **Cloud Shell** コマンド プロンプトで、vi エディタ インターフェイスで変更内容を保存してファイルを閉じるには、**Esc** キーを押し、**:** を押し、**「wq!」** と入力し、**Enter** を押します。

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


#### タスク 3: Building Blocks ハブ アンド スポーク パラメーター ファイル を準備します。

1. **Cloud Shell** ペインで、**ファイルのアップロード/ダウンロード** アイコンをクリックし、ドロップダウン メニューで **アップロード** をクリックします。 

1. **開く** のダイアログ ボックスで、**\\allfiles\\AZ-301T04\\Module_03\\LabFiles\\Starter\\** フォルダに移動し、**hub-nva.json**、**hub-vnet.json**、 **hub-vnet-peering.json**、 **spoke1.json** および **spoke2.json** ファイルを選択し、**開く** をクリックします。 

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminUsername** パラメーターのプレースホルダを **hub-vnet.json** Building Blocks パラメーター ファイルの値 **Student** に置き換えます。

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/hub-vnet.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminPassword** パラメーターのプレースホルダを **hub-vnet.json** Building Blocks パラメーター ファイルの値 **Pa55w.rd1234** に置き換えます。

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/hub-vnet.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、**hub-vnet.json** Building Blocks パラメータファイルでパラメータ値が正常に変更されたことを確認します。

    ```sh
    cat ~/hub-vnet.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminUsername** パラメーターのプレースホルダを **hub-nva.json** Building Blocks パラメーター ファイルの値 **Student**  に置き換えます。

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/hub-nva.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminPassword** パラメーターのプレースホルダを **hub-nva.json** Building Blocks パラメーター ファイルの値 **Pa55w.rd1234** に置き換えます。

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/hub-nva.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、**hub-nva.json** Building Blocks パラメーター ファイルでパラメータ値が正常に変更されたことを確認します。

    ```sh
    cat ~/hub-nva.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminUsername** パラメーターのプレースホルダを **spoke1.json** Building Blocks パラメーター ファイルの値 **Student** に置き換えます。

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/spoke1.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminPassword** パラメーターのプレースホルダを **spoke1.json** ビルディング　ブロック　パラメーター ファイルの値  **Pa55w.rd1234** に置き換えます。

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/spoke1.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、**spoke1.json** Building Blocks パラメーター ファイルでパラメータ値が正常に変更されたことを確認します。

    ```sh
    cat ~/spoke1.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**adminUsername** パラメーターのプレースホルダを **spoke2.json** Building Blocks パラメーター ファイルの値 **Student**  に置き換えます。

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/spoke2.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンド **Enter**、**Enter** を押して **adminPassword** パラメーターのプレースホルダを **spoke2.json** Building Blocks パラメーター ファイルの値 **Pa55w.rd1234** に置き換えます。

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/spoke2.json
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、**spoke2.json** Building Blocks パラメーター ファイルでパラメータ値が正常に変更されたことを確認します。

    ```sh
    cat ~/spoke2.json
    ```

#### タスク 4: ハブ アンド スポーク設計 のハブ コンポーネントを実装します。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、ハブ仮想ネットワークを含むリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP_HUB_VNET='AADesignLab08-hub-vnet-rg'
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します (プレースホルダを置き換えます `<Azure region>` このラボでリソースをデプロイする Azure リージョンの名前を使用します。):

    ```sh
    LOCATION='<Azure region>'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用してハブ アンド スポーク トポロジのハブ コンポーネントをデプロイします。

    ```sh
    azbb -g $RESOURCE_GROUP_HUB_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/hub-vnet.json --deploy
    ```

1. デプロイが完了するのを待たずに、次のタスクに進みます。


#### タスク 5: ハブ とスポーク 設計 のスポーク コンポーネントを実装します。

1. タスク バーで、**Microsoft Edge** アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>)に移動します。

1. プロンプトが表示された場合は、この実習ラボで前に使用したのと同じユーザー アカウントアカウントで認証します。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、最初のスポーク仮想ネットワークを含むリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP_SPOKE1_VNET='AADesignLab08-spoke1-vnet-rg'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します。

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用してハブ アンド スポーク トポロジの最初のスポーク コンポーネントをデプロイします。

    ```sh
    azbb -g $RESOURCE_GROUP_SPOKE1_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/spoke1.json --deploy
    ```

1. デプロイが完了するのを待たずに、次の手順に進みます。

1. タスク バーで、**Microsoft Edge** アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>)に移動します。

1. プロンプトが表示された場合は、この実習ラボで前に使用したのと同じユーザー アカウントアカウントで認証します。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、2 番目のスポーク仮想ネットワークを含むリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP_SPOKE2_VNET='AADesignLab08-spoke2-vnet-rg'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します。

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用してハブ アンド スポーク トポロジの 2 番目のスポーク コンポーネントをデプロイします。

    ```sh
    azbb -g $RESOURCE_GROUP_SPOKE2_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/spoke2.json --deploy
    ```

1. デプロイが完了するのを待たずに、次のタスクに進みます。

#### タスク 6: ハブとスポークの設計 の VNet ピアリングを構成します。

1. タスク バーで、**Microsoft Edge** アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>)に移動します。

1. プロンプトが表示された場合は、この実習ラボで前に使用したのと同じユーザー アカウントアカウントで認証します。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、ハブ仮想ネットワークを含むリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP_HUB_VNET='AADesignLab08-hub-vnet-rg'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します。

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用してハブ アンド スポーク トポロジ内の仮想ネットワークのピアリングをプロビジョニングします。

    ```sh
    azbb -g $RESOURCE_GROUP_HUB_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/hub-vnet-peering.json --deploy
    ```

1. デプロイが完了するのを待たずに、次のタスクに進みます。


#### タスク 7: ハブとスポーク設計 のルーティングを構成します。

1. タスク バーで、**Microsoft Edge** アイコンをクリックします。

1. 開いているブラウザ ウィンドウで、**Azure Portal** (<https://portal.azure.com>)に移動します。

1. プロンプトが表示された場合は、この実習ラボで前に使用したのと同じユーザー アカウントアカウントで認証します。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure サブスクリプションの名前を指定する変数を作成します。

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、ルーターとしてハブ ネットワーク 仮想 アプライアンス (NVA) 機能を含むリソース グループの名前を指定する変数を作成します。

    ```sh
    RESOURCE_GROUP_HUB_NVA='AADesignLab08-hub-nva-rg'
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、デプロイに使用する Azure リージョンを指定する変数を作成します。

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、Azure Building Blocks を使用してハブ アンド スポーク トポロジの NVA コンポーネントをデプロイします。

    ```sh
    azbb -g $RESOURCE_GROUP_HUB_NVA -s $SUBSCRIPTION_ID -l $LOCATION -p ~/hub-nva.json --deploy
    ```

1. 次のタスクに進む前に、デプロイが完了するのを待ちます。

    > **注記**: デプロイには約 10 分かかります。


## 演習 2: ハブスポーク トポロジを確認します。

#### タスク 1: ピアリング構成 を調べます。

1. Azure portal のハブ メニューで、**すべてのサービス** をクリックします。

1. **すべてのサービス** メニューで、**フィルター** テキスト ボックスに **仮想ネットワーク** と入力し、**Enter** キーを押します。

1. 結果の一覧で、**仮想ネットワーク**]をクリックします。

1. **仮想ネットワーク** ブレードで、**hub-vnet** をクリックします。

1. **hub-vnet** ブレードで、**ピアリング** をクリックします。

1. **hub-vnet - ピアリング** ブレードで、ピアリングとそのステータスの一覧を確認します。

1. **仮想ネットワーク** ブレードに戻り、**spoke1-vnet** をクリックします。

1. **spoke1-vnet** ブレードで、**ピアリング** をクリックします。

1. **spoke1-vnet - ピアリング** ブレードで、既存のピアリングとその状態を確認します。

1. **仮想ネットワーク** ブレードに戻り、**spoke2-vnet** をクリックします。

1. **spoke2-vnet** ブレードで、**ピアリング** をクリックします。

1. **spoke2-vnet - ピアリング** ブレードで、既存のピアリングとその状態を確認します。

#### タスク 2: ルーティング構成 を調べます。
1. **すべてのサービス** メニューで、**フィルター** テキスト ボックスに **ルート テーブル** と入力し、**Enter** キーを押します。

1. 結果の一覧で、**ルート テーブル** をクリックします。

1. **ルートテーブル** ブレードで、**spoke1-rt** をクリックします。

1. **spoke1-rt** ブレードで、ルートの一覧を確認します。 ルート **toSpoke2** の **次ホップ** エントリに注意してください。 

1. **ルート テーブル** ブレードに戻り、**spoke2-rt** をクリックします。

1. **spoke2-rt** ブレードで、ルートの一覧を確認します。 ルート **toSpoke1** の **次ホップ** エントリに注意してください。 

#### タスク 3: スポーク 間の接続性を確認します。

1. Azure portal のハブ メニューで、**すべてのサービス** をクリックします。

1. **すべてのサービス** メニューで、**フィルター** テキストボックスに **Network Watcher** と入力し、**Enter** キーを押します。

1. 結果の一覧で、**Network Watcher** をクリックします。

1. **Network Watcher** ブレードの **ネットワーク診断ツール** セクションで、**接続のトラブルシューティング** をクリックします。

1. **Network Watcher - 接続のトラブルシューティング** ブレードで、次のタスクを実行します。

    - **サブスクリプション** ドロップダウン リスト エントリをデフォルト値に設定したままにします。

    - **リソース グループ** ドロップダウン リストで、**AADesignLab08-spoke1-vnet-rg** エントリを選択します。

    - **仮想マシン** ドロップダウン リストで、デフォルトのエントリのままにします。

    - **ポート** テキスト ボックスは空白のままにします。 

    - **宛先** オプションが　**手動で指定**　に設定されていることを確認します。

    - **URI、FQDN、または IPv4** テキスト ボックスに **「10.2.0.68」** エントリを入力します。

    -  **ポート** テキストに「3389」と入力します。 

    - チェック** ボタンをクリックします。

1. 接続チェックの結果が返されるまで待ち、ステータスが **到達可能** であることを確認します。

    > **注記**: Network Watcher を初めて使用する場合は、確認に最大 5 分かかる場合があります。


## 演習 3: ラボ リソースの削除

#### タスク 1: Cloud Shell を開きます。

1. Portal の上部にある **Cloud Shell** アイコンをクリックして、[Cloud Shell] ペインを開きます。

1. **Cloud Shell** コマンドプロンプトで、次のコマンドを入力し、**Enter** を押して、このラボで作成したすべてのリソース グループを一覧表示します。

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab08')]".name --output tsv
    ```

1. 出力に、この実習ラボで作成したリソース グループのみが含まれていることを確認します。 これらのグループは、次のタスクで削除されます。

#### タスク 2: リソース グループ 1 を削除します。

1. **Cloud Shell** コマンド プロンプトで、次のコマンドを入力し、**Enter** を押して、このラボで作成したリソース グループを削除します。

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab08')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. ポータルの下部にある **Cloud Shell** プロンプトを閉じます。

> **復習**: この演習では、この演習で使用したリソースを削除しました。
