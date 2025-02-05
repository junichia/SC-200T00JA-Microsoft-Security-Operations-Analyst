# モジュール 5 - ラボ 1-演習1 - Azure Sentinel 環境を構成する

## ラボ シナリオ

あなたは、Azure Sentinel を実装しようとしている企業で働いているセキュリティ オペレーションアナリストです。コストを最小限に抑え、コンプライアンス規制を満たし、セキュリティ チームが日常の職務を遂行するのに最も管理しやすい環境を提供するという会社の要件を満たすように、Azure Sentinel 環境を設定する責任があります。

### タスク 1: Azure Sentinel ワークスペースを初期化する。

このタスクでは、Azure Sentinel ワークスペースを作成します。

1. 管理者として WIN1 仮想マシンにログインします。パスワードは **Pa55w.rd** です。  

2. Microsoft Edge ブラウザーを開きます。

3. Microsoft Edgeブラウザーで Azure ポータルに移動します https://portal.azure.com。

4. **サインイン**ダイアログボックスで、ラボ ホスティング プロバイダーから提供された**テナントの電子メール**アカウントをコピーして貼り付け、「**次へ**」 を選択します。

5. **パスワードの入力**ダイアログ ボックスで、ラボ ホスティング プロバイダーから提供された**テナントパスワード** をコピーして貼り付け、「**サインイン**」を選択します。

6. Azure ポータルの検索バーに 「*Sentinel*」 と入力し、「**Azure Sentinel**」 を選択します。

7. 「**+ 作成**」を選択します。

8. 「**+ 新しいワークスペースの作成**」を選択します。

**注** まず、新しい Log Analytics ワークスペースを作成します。

9. 適切なサブスクリプションを選択します。

10. リソースグループの**新規作成**リンクを選択し、任意の新しいリソースグループ名を入力します。

11. 「*インスタンスの詳細*」の「名前」フィールドで、任意の LogAnalytics ワークスペースの名前を入力します。

**注:** この名前は、Azure Sentinel ワークスペース名にもなります。

12. 適切な地域を選択します。適切な地域が既定になる場合や、インストラクターがどの地域を選択するかについて具体的なアドバイスをする場合があります。  

13. 「**レビュー + 作成**」 を選択します。

14. 「Log Analytics ワークスペースの作成」 で、「**作成**」 を選択します。新しい LogAnalytics ワークスペースが 「*Azure Sentinel をワークスペースに追加*」 ページのリストに表示されるのを待ちます。  これには時間がかかることがあります。

15. 新しく作成されたワークスペースが表示されたらそれを選択し、「**追加**」を選択します。

16. 新しく作成された Azure Sentinel ワークスペースを操作して、ユーザーインターフェイスオプションに慣れてください。

### タスク 2: ウォッチリストを作成する。

このタスクでは、Azure Sentinel でWatchlist を作成します。

1. Windows 10 の画面下部の検索ボックスに *Notepad* と入力します。  結果から、「**Notepad**」を選択します。

2. １行目に *Hostname* と入力し、新しい行を入力します。

3. メモ帳の 2 行目から 6 行目に、次のホスト名を追加します。

    ```
    Host1
    Host2
    Host3
    Host4
    Host5
    ```

4. メニューから**ファイル-名前を付けて保存**を選択し、以下の通り設定します。
5. ファイルに*HighValue.csv*という名前を付けます。  ファイル タイプを **すべてのファイル(*.*)** に変更します。  次に、「**保存**」を選択します。  ファイルは PC の*ドキュメント* フォルダーに保存できます。

5. メモ帳を閉じます。

6. Azure Sentinel で、**構成** の「**ウォッチリスト**」オプションを選択します。

7. コマンド バーから「**+ 新規追加**」を選択します。

8. ウォッチリストウィザードで、次のように入力します。
    - 名前: HighValueHosts
    - 説明: High Value Hosts
    - ウォッチリストエイリアス： HighValueHosts

9. **次へ:ソース >** を選択します。

10. 「**ファイルを参照**」を選択し、作成した *HighValue.csv* ファイルを参照します。

11. 「SearchKey フィールド」で、「**Hostname**」 を選択します。

12. 「**次へ: 確認と作成 >**」を選択します。

13. 入力した設定を確認し、「**作成**」 を選びます。

14. 画面がウォッチリストの一覧に戻ります。

15. 新しいウォッチリストを選択します。  右側のタブで、「**Log Analytics で表示**」を選択します。

16. 次のKQLステートメントが自動的に実行され、結果が表示されます。

```KQL
_GetWatchlist('HighValueHosts')
```
**注** 結果が表示されるようになるまで少し時間がかかります。

独自のKQLステートメントで _GetWatchlist（'HighValueHosts'） を使用して、リストにアクセスできるようになりました。参照する列は *Hostname* になります。

### タスク 3: 脅威インジケーターを作成する。

このタスクでは、Azure Sentinel でインジケーターを作成します。

1. Azure Sentinelで、脅威管理の「**脅威インテリジェンス**」オプションを選択します。

2. コマンド バーから 「**+新規追加**」を選択します。

3. タイプドロップダウンで使用可能なさまざまなインジケータータイプを確認します。**domain-name** を選択します。ドメインボックスにイニシャルを入力します。たとえば、*fmg.com* などです。

4. **脅威の種類**として、「**anomalous-activity(悪意のあるアクティビティ)**」を選択します。

5. 名前には、ドメインに使用されているのと同じものを入力します。たとえば、*fmg.com* などです。

6. 「*有効開始日*」フィールドを今日の日付に設定します。

7. 「**適用**」を選択します。

**注** インジケーターが表示されるまで少し時間がかかる場合があります。

8. 全般領域で「**ログ**」オプションを選択します。  クエリ ウィンドウを表示するには、「常にクエリを表示する」 オプションを無効にする必要がある場合があります。

9. 以下の KQL ステートメントを実行します。

```KQL
ThreatIntelligenceIndicator
```
結果を右にスクロールして、DomainName 列を表示します。次の KQL ステートメントを実行して、DomainName 列だけを表示することもできます。  

```KQL
ThreatIntelligenceIndicator
| project DomainName
```
## これでラボは完了です。
