---
summary: 開発環境をクリーンな状態に保ち、アプリケーション開発の停止や遅延を防止します。
tags: 
en_title: Best Practices for a Tidy and Clean Development Environment
---

# 整理されたクリーンな開発環境のベストプラクティス

<pre class="script-css">
.subtopic {
    font-size: 20px;
    font-weight: bold;
}
</pre>

アプリケーションの開発インフラが大きくなるほど、データベースの使用領域も増加します。データベースはOutSystemsプラットフォームの中心的な要素であるため、整理されたクリーンな開発環境を保ち、チームの作業速度が低下しないようにすることが重要です。処理能力を向上することはいつでも可能ですが、通常は追加コストが発生し、インフラタスクを実行するためにSysAdminプロファイルの関与も必要になります。

開発環境が整理されていない場合、以下が発生する可能性があります。

* パブリッシュ中のライムアウトエラー
* モジュールを開く間のタイムアウト
* 開発の遅延
* 複数の環境（開発およびアプリケーション）にわたる全体的なパフォーマンスの低下

この記事では、データベース領域を無駄遣いする様々なタイプのデータを確認し、その消去方法について説明します。


## モジュールバージョン

モジュールバージョンの管理は、アプリケーション開発の複数のタスク（モジュールを開く、参照の追加/削除、パブリッシュ）に影響を及ぼすため、最も関連性が高いアクションの1つです。

モジュールをパブリッシュするたびに、環境に新しいバージョンが作成されます。バージョンは、前の開発段階にロールバックする必要があるときに特に便利です。
しかし、アプリケーションは拡張し続けるため、パブリッシュを頻繁に行っているとすぐに数百または数千のバージョンができ、日常の開発タスクで速度の低下を感じ始めることがあります。

そのため、Development環境で不要になった古いモジュールバージョンを定期的にクリーンアップすることが重要です。古いモジュールバージョンを削除するには、Service Centerを使用して直接削除する方法と、データベース領域を解放する機能を提供するDbCleaner APIを使用する方法があります。

<div class="subtopic">Service Centerを使用して古いモジュールバージョンを削除する</div>

1.  [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform)（https://<ご利用の環境>/ServiceCenter）に移動します
1. ［Factory］タブをクリックします
1. ［Modules］サブメニューオプションをクリックします。次に、［Check Old Module Versions to Delete］リンクをクリックします
1. 削除する期間を選択して、［Check Versions to Delete］ボタンをクリックします
1. モジュールバージョンのリストが表示されます（リストのレコード数は最大100件）
1. ［Delete Displayed Versions］をクリックして続行します

![](images/servicecenter-old-module-versions.png?width=800)

<div class="subtopic">DbCleaner API</div>

DbCleaner APIのアクションを使用して、より柔軟に古いモジュールをクリーンアップできる独自のソリューションを実装することもできます。

* [ModuleVersion_ListOldest](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#ModuleVersion_ListOldest):  データベース内に保存されている、指定された日時より前にパブリッシュされたモジュールバージョンのリストを返します。このアクションでは、現在パブリッシュされているモジュールバージョンと、アプリケーションまたはソリューションのタグ付けされたバージョンで使用されているモジュールバージョンは返されません。
* [ModuleVersion_Delete](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#ModuleVersion_Delete):  指定されたモジュールの指定されたモジュールバージョンをデータベースから削除します。
* [ModuleVersion_DeleteAll](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#ModuleVersion_DeleteAll):  指定された日時より前にパブリッシュされたモジュールバージョンを削除します。このアクションでは、現在パブリッシュされているモジュールバージョンと、アプリケーションまたはソリューションのタグ付けされたバージョンで使用されているモジュールバージョンは削除されません。


<div class="info" markdown="1">
いずれの場合も、タグ付けされたアプリケーションバージョンやソリューションバージョンに関連付けられていないモジュールバージョンのみが削除されます。これにより、アプリケーションを前のバージョンにロールバックする必要があるときにコードが失われていないことが保証されます。
</div>


## ソリューションバージョン

ソリューションとは、環境内のモジュールのセットを集約したものです。作成したソリューションバージョンが不要になった場合は、Service Centerで削除できます。

1. [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform)（https://<ご利用の環境>/ServiceCenter）に移動します
1. ［Factory］タブをクリックします
1. ［Solutions］サブメニューオプションをクリックします
1. 特定のソリューションの詳細画面に移動します
1. ［Versions］タブで、削除するバージョンを選択します
1. ［Delete］ボタンをクリックして続行します

![](images/servicecenter-solution-version-delete.png?width=800)

ソリューションバージョンを削除した後、[「モジュールバージョン」](#Module_versions)セクションの説明に従って、そのバージョンに関連付けられたモジュールバージョンを削除することができます。

## タグ付けされたアプリケーションバージョン

特定の[タグ付けされたバージョン](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Deploy_Applications/Tag_a_Version)へのロールバックが不要であることが確実な場合、アプリケーションバージョンを削除することもできます。
LifeTime管理コンソール2019年2月リリース版以降、[LifeTime Deployment API v2](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/LifeTime_Deployment_API_v2)で「InUse」以外のアプリケーションバージョンを削除するためのメソッドが提供されています。

` DELETE /lifetimeapi/rest/v2/applications/{ApplicationKey}/versions/{VersionKey}/ `

タグ付けされたアプリケーションバージョンを削除した後、[「モジュールバージョン」](#Module_versions)セクションの説明に従って、そのバージョンに関連付けられたモジュールバージョンを削除することができます。


## 一時テストモジュール

チームでアプリケーションを開発しているときに、一時テストモジュールや概念実証（PoC）を作成する必要がある場合や、Forgeコンポーネントのデプロイやテストをよく行う場合があります。
これらのモジュールはどれも実装フェーズでは役立ちますが、すぐに使用しなくなり、開発環境で不要なコードになってしまいます。
ベストプラクティスでは、定期的な開発インフラ管理の一環として、これらのモジュールを環境から消去/削除します。アプリケーション全体または個別のモジュールを削除することができます。

<div class="subtopic">アプリケーションを削除する</div>

1. [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform)（https://<ご利用の環境>/ServiceCenter）に移動します
1. ［Factory］タブをクリックします
1. ［Applications］サブメニューオプションをクリックします
1. 特定のアプリケーションの詳細画面に移動します
1. ［Delete］ボタンをクリックして続行します

![](images/servicecenter-application-delete.png?width=800)

<div class="subtopic">モジュールを削除する</div>

1. [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform)（https://<ご利用の環境>/ServiceCenter）に移動します
1. ［Factory］タブをクリックします
1. ［Modules］サブメニューオプションをクリックします
1. 特定のモジュールの詳細画面に移動します
1. ［Delete Module］ボタンをクリックして続行します

![](images/servicecenter-module-delete.png?width=800)


## アプリケーションデータ

アプリケーションデータは、基本的にアプリケーションのデータベースエンティティに保存されているすべての情報です。これには、単体テスト中に追加するデータのほか、データモデルの削除されたエンティティやアトリビュートが含まれます。

<div class="subtopic">テストデータ</div>

特に開発環境では、テスト/ダミーデータを保持しているのが一般的です。これは、開発者が開発中に行う様々な機能のテストの結果です。このデータは、アプリケーションを実行するうえでは重要ではありませんが、増加して不整合が生じる場合がよくあるため、定期的に消去することが有効です。 
OutSystemsでは、これらの消去操作を実行する際にデータベースにアクセスする必要はありません。独自のデータ削除ロジックの実装の詳細については、[「エンティティからのデータの削除」](https://success.outsystems.com/Documentation/Development_FAQs/How_to_delete_data_from_Entities)をご覧ください。

<div class="subtopic">エンティティおよびアトリビュート</div>

アプリケーションでエンティティやアトリビュートを削除しても、OutSystemsではデータベース内の対応するテーブルや列は削除されません。アプリケーションをロールバックするときに備え、データは安全に保存されます。エンティティやアトリビュートを使用する予定がない場合、ベストプラクティスではそれらを削除し、データベース領域を解放します。そのために、OutSystemsではDbCleaner APIメソッドが提供されています。

* [Attribute_ListDeleted](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Attribute_ListDeleted):  モジュールのメタモデルからは削除されているものの、データベース内に物理的にまだ存在しているアトリビュートのリストとその情報を返します。
* [Attribute_DropColumn](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Attribute_DropColumn):  指定されたエンティティアトリビュートに関連付けられたデータベーステーブルの列を物理的に削除します。エンティティアトリビュートがモジュールのメタモデル内にまだ存在している場合、削除操作は実行されません。
* [Entity_ListDeleted](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Entity_ListDeleted):  モジュールのメタモデルからは削除されているものの、データベース内に物理的にまだ存在しているエンティティのリストとその情報を返します。
* [Entity_DropTable](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Entity_DropTable):  指定されたエンティティに関連付けられたデータベーステーブルを物理的に削除します。エンティティがモジュールのメタモデル内にまだ存在している場合、削除操作は実行されません。


## プロセス

ビジネスプロセステクノロジー（BPT）プロセスがデータベース領域を無駄遣いしている場合もあります。開発中は、機能をテストするためにいくつかのプロセスを起動することが一般的です。
ログに記録された古いプロセスの情報をすべて消去するために、OutSystemsでは[BPT API](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/BPT_API)メソッドが提供されています。

* [Process_Delete](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/BPT_API#Process_Delete):  上位レベルのプロセスインスタンスのログ情報をすべて削除します。このプロセスインスタンスはターミネートまたは終了している必要があります。
* [Process_BulkDelete](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/BPT_API#Process_BulkDelete):  指定された日付より前にターミネートまたは終了し上位レベルのプロセスのインスタンスのログ情報をすべて削除します。

これらを参照としてアプリケーションに追加し、それらを呼び出して古いインスタンスを削除します。


## メール

プロセスと同様に、メールがデータベース領域を無駄に消費している場合もあります。メールは、時には1MBを超える巨大な添付ファイルを含まれることがあるため、特に関係があります。
メール情報を消去するために、OutSystemsでは[Emails API](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/Emails_API)が提供されています。これを使用すると、消去メカニズムを簡単に実装することができます。


## ログ

[ログ情報](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Monitor_and_Troubleshoot/Logging_database_and_architecture)には、エンドユーザーまたは外部システムによるアプリケーションへのアクセスのライフサイクルが記録されます。以下に示す2種類のログがあります。

* 上位レベルのログ（Screen、Integration、Mobile Request、Cyclic Job）
* ドリルログ（Error、 General、Integration、Extension）

ログモデルは、アプリケーションの実行の妨げに極力ならないように設計されており、Development環境でも[ログのローテーション](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Monitor_and_Troubleshoot/Logging_database_and_architecture/The_log_tables_and_views#The_rotation_of_the_logs)によって何らかの形で影響が制御されます（毎週、ログサービスがいずれかの週次テーブルを破棄してデータベースの領域を空け、そのテーブルを再利用する時期が来たら新しいログを挿入することができます）。

OutSystems 11以降ではログの削除アクションはサポートされていませんが、[ログデータを別のデータベースに保存](https://success.outsystems.com/Support/Enterprise_Customers/Upgrading/Keep_OutSystems_log_data_in_a_separate_database)できます。これにより、アプリケーションデータにアクセスしている間のログ書き込み操作が実行中のアプリケーションに与える影響を減らすことができます。 


## その他の考慮事項

この記事では、Development環境のデータベース領域を使用する複数のコンポーネントを示し、データベース自体に直接アクセスすることなく不要なデータを消去できる方法について説明します。

これらのベストプラクティスは個別に適用することができ、制限はありませんが、オンプレミスインストールタイプの++++でも考慮する必要があり、今回限りの考慮事項ではないことに留意してください。

### クラウドとオンプレミスの比較

**上記のクリーンアップオプションはどちらのインストールタイプでも有効**ですが、オンプレミスではお客様がインフラを管理する必要があるため、実行できるアクションがほかにもいくつかあります。

**オンプレミス**でインストールされた開発環境の場合、データベースの柔軟性を高める（データを破棄できることなど）ほかに、サーバーのファイルシステムの定期的な保守を考慮する必要があります。詳細については、[「OutSystems Platform Serverのディスク領域の使用と管理に関するガイド」](http://www.outsystems.com/goto/disk-space-usage-guide)をご覧ください。

### クリーンアップを自動化する

Development環境のクリーンアップは定期的に考慮する必要があり、問題が発生した場合に1回だけ行うタスクではありません（「転ばぬ先の杖」）。
これを念頭に置いたうえで可能な方法として、（[OutSystemsのタイマー](https://success.outsystems.com/Documentation/11/Developing_an_Application/Use_Timers/Create_and_Run_Timers)を使用して）定期的なクリーンアップロジックを実装し、この記事で説明されている様々なコンポーネントを処理することができます。

クリーンアップソリューションを一から作成したくない場合は、OutSystems Forgeにあるコンポーネントをいつでも探すことができます（「cleaner」や「database space」で検索すると、OutSystemsコミュニティのメンバーによる様々なコンポーネントが見つかります）。
