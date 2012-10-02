UnitHosting API仕様書 ver 1.3.2
===============================


このドキュメントは、株式会社ユニットホスティングが提供する、*ユニットホスティングWebサービスAPI*(以下、UHAPIと記述)のリファレンスマニュアルです。UHAPIでは以下のAPIサービスを提供します。

* サーバAPIサービス

単一のサーバインスタンスの制御を行います。サーバAPIを利用するためには、サーバのインスタンスIDと有効なAPIキーを入手する必要があります。

> **NOTE**  
> サーバのAPIキーはユニットホスティングのサイトより取得出来ます。

* サーバグループAPIサービス

単一のサーバグループとその所属するサーバ・リソースの制御を行います。サーバグループAPIを利用するためには、サーバのインスタンスIDとAPIキーを入手する必要があります。サーバグループAPIを用いることで個別のサーバの有効なAPIを取得することができます。

> **NOTE**  
> サーバグループのAPIキーはユニットホスティングのサイトより取得出来ます。

### XML-RPCエンドポイント

UHAPIサービスを利用するためのリクエストの送信先は以下のURIとなります。

> https://cloud.unit-hosting.com/xmlrpc


サーバAPI
--------

### vm.getStatus

現在のサーバのステータスを返します。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

##### 戻り値

*   running : 動作中
*   halted : 停止中

### vm.start

停止しているサーバを起動します。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   string result : 実行結果(成功なら success)

### vm.shutdown

起動しているサーバをシャットダウン（ソフトシャットダウン)します。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   string result : 実行結果(成功なら success)

### vm.powerOff

起動しているサーバをシャットダウン（ハードシャットダウン)します。vm.powerOff(ハードシャットダウン)はなるべくせずに可能な限りvm.shutdown(ソフトシャットダウン)を利用してください。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   string result : 実行結果(成功なら success)

### vm.reboot

起動しているサーバを再起動します。メモリ・CPUコア・ディスクの変更があれば反映します。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   string result : 実行結果(成功なら success)

### vm.destroy

サーバを削除します。削除は停止(halted)時にしか行うことができません。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   string result : 実行結果(成功なら success)

### vm.renew

サーバの初期化。初期化は停止(halted)時にしか行うことができません。

#### 引数

*   string display_name : 表示名
*   string rootpw : rootパスワード
*   string ssh_key : sshのキー
*   string user_script : ユーザスクリプトURI

以下の引数はオプションです:

*   string op_user : user名(ユーザスクリプト内で利用)
*   string op_mail : メールアドレス(ユーザスクリプト内で利用)

#### 戻り値

*   string result : 実行結果(成功なら success)


### vm.getIpInfo

サーバに割り当てられているIPドレスについての情報を返します。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   array 
    *   ip: IP
    *   route_to: グローバルIP(静的NATがある場合)

### vm.getMemoryUnitSize

現在、追加されているメモリのサイズ(MB)を取得します。

> **NOTE**  
> 追加前にプランによりベースメモリ(256MB or 512MB)が存在しているので、実際のメモリサイズはこの*メソッドの返り値+ベースメモリサイズ*となります。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   integer 追加されているメモリサイズ(MB)

### vm.setMemoryUnitSize

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー
*   integer size : セットする追加メモリサイズ

#### 戻り値

*   string result : 実行結果(成功なら success)

#### 説明

追加するメモリサイズを設定します。

> **NOTE**  
> 追加を反映するためには、リブートする必要があります。

### vm.getCpuUnitNum


現在、追加されているCPUコア数を取得します。

> **NOTE**  
> 追加前に1コア存在しているので、実際のコア数はこの*メソッドの返り値+1*となります。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   integer CPUのコア数

### vm.setCpuUnitNum

追加するCPUコア数を設定します。

> **NOTE**  
> 追加を反映するためには、リブートする必要があります。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー
*   integer num : セットする追加CPUのコア数

#### 戻り値

*   string result : 実行結果(成功なら success)

### vm.replicate

サーバの複製を行います。

#### 引数

*   string instance_id : 複製元のサーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   string instance_id : 複製されたサーバのインスタンスID
*   string result : 実行結果(成功なら success)

### vm.getCrushRecover

クラッシュリカバリ機能の現在の設定状態を取得します。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー

#### 戻り値

*   boolean: 設定結果(true=>設定されている)


### vm.setCrushRecover

クラッシュリカバリ機能の設定をします。

#### 引数

*   string instance_id : サーバのインスタンスID
*   string api_key : サーバのAPIキー
*   boolean crush_recover : 設定値(設定する=>true)

#### 戻り値

*   string result : 実行結果(成功なら success)


### vm.plugVif

仮想NICを導入します。

#### 引数

*   string network_uuid: Network UUID
*   integer device: NICのデバイスナンバー(1,2,3のいづれかを指定する)


#### 戻り値

*   string result : 実行結果(成功なら success)


### vm.getVifs

サーバにに導入されている仮想NICの情報を得ます。

### 引数

なし

### 戻り値

* VIF
  *    string  MAC: MACアドレス
  *    integer device: デバイス番号
  *    string  uuid: 仮想NICのUUID
  *    string  network: ネットワークのUUID

### vm.unplugVif

仮想NICを取り外します。

### 引数

*  string vif_uuid: 仮想NICのUUID

#### 戻り値

*   string result : 実行結果(成功なら success)

サーバグループAPI
---------------

### vmGroup.getVms

サーバグループが所有するサーバの一覧を返します。この機能により、VmのAPIキーが取得できます。

#### 引数

*   string instance_id : サーバグループのインスタンスID
*   string api_key : サーバグループのAPIキー

#### 戻り値

*   array  
    instance_id : サーバのインスタンスID
    api_key: サーバのAPIキー

### vmGroup.createVm

サーバを作成します。

#### 引数

*   string instance_id : サーバグループのインスタンスID
*   string api_key : サーバグループのAPIキー
*   string display_name : 表示名
*   string rootpw : rootパスワード
*   string ssh_key : sshのキー
*   string user_script : ユーザスクリプトURI

以下の引数はオプションです:

*   string op_user : user名(ユーザスクリプト内で利用)
*   string op_mail : メールアドレス(ユーザスクリプト内で利用)
*   string plan_id : プランID
*   string instance_id : サーバグループのインスタンスID

#### 戻り値

*   string instance_id : サーバのインスタンスID
*   string result : 実行結果(成功なら success)

### vmGroup.getPlans

現在利用可能なプランの一覧を取得します。

#### 引数

*   string instance_id : サーバグループのインスタンスID
*   string api_key : サーバグループのAPIキー

#### 戻り値

*   integer id : plan_id
*   string name : プラン名
*   string cycle :　課金サイクル(hourly,monthly)
*   float point : 課金ポイント(cycle毎)


サンプルコード
-------------

Ruby,PHP,Python,Perlの各言語によるスケールアップのサンプルプログラムは以下のとおりです。

#### Ruby

<script src="https://gist.github.com/3812405.js?file=gistfile1.rb"></script>

#### PHP

<script src="https://gist.github.com/3812417.js?file=gistfile1.php"></script>

#### Python

<script src="https://gist.github.com/3812422.js?file=gistfile1.py"></script>

#### Perl

<script src="https://gist.github.com/3812432.js?file=gistfile1.pl"></script>

### 変更履歴

*   1.0 APIマニュアル初版公開
*   1.1 <code class="snippet">vm.getIpInfo</code>の追加
*   1.3 <code class="snippet">vm.getCrushRecover</code> <code class="snippet">vm.setCrushRecover</code>の追加、その他修正
*   1.3.1 エンドポイントのURLを https://cloud. ...に修正
*   1.4 vm.plugVif/vm.unplugVif/vm.getVifs 追加

