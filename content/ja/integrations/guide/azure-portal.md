---
title: Azure Portal の Datadog
kind: ガイド
further_reading:
  - link: integration/azure
    tag: ドキュメント
    text: Azure インテグレーション
  - link: 'https://www.datadoghq.com/blog/azure-datadog-partnership'
    tag: ブログ
    text: Microsoft とのパートナーシップにより、Datadog を Azure Portal でネイティブに利用可能に
---
<div class="alert alert-warning">
  このガイドは、Azure で US3 を使用している Datadog のお客様を対象としています。
</div>

このガイドは、Datadog リソースを使用して Azure ポータルで Azure と Datadog のインテグレーションを管理するためのものです。Azure の Datadog リソースは、Datadog オーガニゼーションと Azure サブスクリプションの間の接続を表します。このガイドに進む前に、Azure で [Datadog リソースを作成][1]してください。

Datadog リソースを使用すると、関連付けられた Azure サブスクリプション内で以下を管理できます。
- Azure メトリクスとプラットフォームログのコレクションを構成します
- メトリクスとログを送信する Azure リソースを確認します
- API キーを表示し、Datadog リソース Agent のデプロイのキーのデフォルトを設定します
- Datadog VM Agent を Azure VM にデプロイし、実行中の Agent に関する詳細を表示します
- Datadog .NET 拡張機能を Azure Web Apps にデプロイし、インストールされている拡張機能の詳細を表示します
- シングルサインオンを再構成します
- Datadog オーガニゼーションの請求プランを変更します (Azure Marketplace のみ)
- Azure インテグレーションを有効または無効にします
- Datadog リソースを削除します

このページでは、Azure Portal の体験について説明します。CLI を使用する場合は、[Datadog 用 Azure CLI][2] を参照してください。

## 概要

左側のサイドバーで "Overview" を選択して、Datadog リソースの情報を表示します。

{{< img src="integrations/guide/azure_portal/resource-overview.png" alt="Azure US3 リソース概要" responsive="true" style="width:100%;">}}

### 重要な情報

概要ページには、リソースグループ名、場所 (地域)、サブスクリプション、タグ、Datadog オーガニゼーションリンク、ステータス、料金プラン、請求期間など、Datadog リソースに関する重要な情報が表示されます。

**注**: SSO が有効になっている場合、Datadog オーガニゼーションリンクは SAML リンクです。Datadog オーガニゼーションが Azure マーケットプレイスで作成された場合は、このリンクを初めて使用するときにパスワードを設定します。

### リンク

概要ページには、Datadog ダッシュボード、ログ、ホストマップを表示するためのリンクがあります。

### リソースサマリー

概要ページには、ログとメトリクスを Datadog に送信するリソースのサマリーテーブルが表示されます。このテーブルには、次の列が含まれています。

| 列             | 説明                                                               |
|--------------------|---------------------------------------------------------------------------|
| Resource type      | Azure リソースタイプ                                                   |
| Total resources    | リソースタイプのすべてのリソースの数                          |
| Logs to Datadog    | インテグレーションを通じて Datadog にログを送信するリソースの数    |
| Metrics to Datadog | インテグレーションを通じて Datadog にメトリクスを送信するリソースの数 |

### Disable

Azure から Datadog へのログとメトリクスの送信を停止するには、概要ページで "Disable" を選択し、"OK" をクリックします。

{{< img src="integrations/guide/azure_portal/disable.png" alt="Azure US3 のインテグレーションの無効化" responsive="true" style="width:100%;">}}

**注**: Datadog リソースを無効にすると、関連付けられたサブスクリプションの Datadog へのメトリクスとプラットフォームログの送信が停止します。Agent または拡張機能を介して Datadog に直接データを送信するサブスクリプション内のリソースは影響を受けません。

### Enable

Azure から Datadog へのログとメトリクスの送信を開始するには、概要ページで "Enable" を選択し、"OK" をクリックします。ログとメトリクスの以前のコンフィギュレーションが取得され、有効になります。

{{< img src="integrations/guide/azure_portal/enable.png" alt="Azure US3 のインテグレーションの有効化" responsive="true" style="width:100%;">}}

### 削除

Datadog リソースを削除するには、概要ページで "Delete" を選択します。`yes` と入力して削除を確認し、"Delete" をクリックします。

{{< img src="integrations/guide/azure_portal/delete.png" alt="Azure US3 の Datadog リソースの削除" responsive="true" style="width:100%;">}}

Azure Marketplace を通じて請求される Datadog オーガニゼーションの場合
- 削除された Datadog リソースが関連する Datadog オーガニゼーションにマップされた唯一の Datadog リソースである場合、ログとメトリクスは Datadog に送信されなくなり、Azure を介した Datadog のすべての請求が停止します。アカウントの次のステップを確認するために Datadog サポートがご連絡します。
- 関連する Datadog オーガニゼーションにマップされた追加の Datadog リソースがある場合、Datadog リソースを削除すると、関連する Azure サブスクリプションのログとメトリクスの送信のみが停止します。

Datadog オーガニゼーションが Azure Marketplace を通じて請求されない場合、Datadog リソースを削除すると、その Azure サブスクリプションのインテグレーションが削除されるだけです。

### Change plan

Datadog の請求プランを変更するには、概要ページで "Change Plan" を選択します。

{{< img src="integrations/guide/azure_portal/change-plan1.png" alt="Azure US3 のプランの変更" responsive="true" style="width:100%;">}}

ポータルは、テナントで利用可能なすべての Datadog プランを取得します。これには、プライベートオファーが含まれます。適切なプランを選択し、"Change Plan" をクリックします。

## Datadog org configurations
### Metrics and logs

左側のサイドバーで "Metrics and logs" を選択して、メトリクスとログのコンフィギュレーションルールを変更します。[メトリクスとログのコンフィギュレーション][3]については、Azure のドキュメントを参照してください。

### Monitored resources

左側のサイドバーで "Monitored Resources" を選択して、Datadog にログとメトリクスを送信するリソースのリストを表示します。検索を使用して、リソース名、タイプ、グループ、場所、Datadog へのログ、または Datadog へのメトリクスでリストをフィルタリングします。

{{< img src="integrations/guide/azure_portal/monitored-resources.png" alt="Azure US3 の監視対象リソース" responsive="true" style="width:100%;">}}

リソースが Datadog にログを送信している場合、"Logs to Datadog" 列には `Sending` と表示されます。それ以外の場合、このフィールドはログが送信されない理由として以下を示します。

| 理由                                    | 説明                                                                                                             |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Resource doesn't support sending logs     | Datadog にログを送信するように構成できるのは、モニタリングログカテゴリを持つリソースタイプのみです。                           |
| Limit of five diagnostic settings reached | 各 Azure リソースには、最大 5 つの診断設定を設定できます。詳細については、[診断設定][4]を参照してください。 |
| エラー                                     | リソースは Datadog にログを送信するように構成されていますが、エラーによってブロックされています。                                         |
| Logs not configured                       | Datadog にログを送信するように構成されているのは、適切なリソースタグを持つ Azure リソースのみです。                             |
| Region not supported                      | Azure リソースは、Datadog へのログの送信をサポートしていないリージョンにあります。                                         |
| Datadog Agent not configured              | Datadog Agent がインストールされていない仮想マシンは、Datadog にログを発行しません。                                        |

### Virtual machine agent

サブスクリプション内の仮想マシン (VM) のリストを表示するには、左側のサイドバーで "Virtual machine agent" を選択します。このページでは、拡張機能として Datadog Agent を VM にインストールできます。

VM ごとに、次の情報が表示されます。

| 列               | 説明                                                                                                                                                    |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resource name        | VM の名前                                                                                                                                                  |
| Resource status      | VM が停止しているか実行しているか。Datadog Agent は、実行中の VM にのみインストールできます。VM が停止している場合、Datadog Agent のインストールは無効になります。 |
| Agent バージョン        | Datadog Agent のバージョン番号                                                                                                                               |
| Agent のステータス         | Datadog Agent が VM で実行されているかどうか。                                                                                                                |
| Integrations enabled | Datadog Agent で有効なインテグレーションによって収集される主要なメトリクス。                                                                                  |
| Install method       | Chef、Azure VM 拡張機能など、Datadog Agent のインストールに使用される特定のツール。                                                                    |
| Sending logs         | Datadog Agent が Datadog にログを送信しているかどうか。                                                                                                          |

#### Install

Datadog Agent をインストールするには、適切な VM を選択し、"Install Agent" をクリックします。ポータルで、デフォルトのキーを使用して Agent をインストールすることの確認が求められます。"OK" を選択してインストールを開始します。Agent がインストールされてプロビジョニングされるまで、Azure はステータスを `Installing` (インストール中) と表示します。Datadog Agent がインストールされると、ステータスが `Installed` (インストール済み) に変わります。

#### アンインストール

Datadog Agent が Azure VM 拡張機能とともにインストールされている場合は、適切な VM を選択して、"Uninstall Agent" をクリックすることで、Agent をアンインストールできます。

Agent が別の方法でインストールされた場合、Datadog リソースを使用して Agent をデプロイまたは削除することはできませんが、Agent に関する情報は引き続きこのページに反映されます。

### App Service extension

サブスクリプション内のアプリサービスのリストを表示するには、左側のサイドバーで "App Service extension" を選択します。このページでは、Azure App Service に Datadog 拡張機能をインストールして、APM トレースとカスタムメトリクスを有効にすることができます。

アプリサービスごとに、次の情報が表示されます。

| 列            | 説明                                                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resource name     | アプリ名                                                                                                                                                                 |
| Resource status   | アプリサービスが停止しているか実行しているか。インストールを開始するには、アプリサービスが実行されている必要があります。アプリサービスが停止している場合、Datadog Agent のインストールは無効になります。 |
| App service plan  | アプリサービス用に構成された特定のプラン                                                                                                                             |
| Extension version | Datadog 拡張機能バージョン番号                                                                                                                                         |

#### Install

[Datadog 拡張機能][5]をインストールするには、適切なアプリを選択し、"Install Extension" をクリックします。ポータルで、拡張機能をインストールすることの確認が求められます。"OK" を選択してインストールを開始します。これにより、アプリが再起動し、次の設定が追加されます。

- `DD_API_KEY:<DEFAULT_API_KEY>`
- `DD_SITE:us3.datadoghq.com`
- `DD_LOGS_INJECTION:true`

Agent がインストールされてプロビジョニングされるまで、Azure はステータスを `Installing` (インストール中) と表示します。 Datadog Agent がインストールされると、ステータスが `Installed` (インストール済み) に変わります。

**注**: [サポートされているランタイム][6]でアプリに拡張機能を追加していることを確認してください。Datadog リソースは、アプリのリストを制限またはフィルタリングしません。

#### アンインストール

Datadog 拡張機能をアンインストールするには、適切なアプリを選択し、"Uninstall Extension" をクリックします。

## 設定
### Single sign-on

シングルサインオンを再構成するには、左側のサイドバーで "Single sign-on" を選択します。

Azure Active Directory を介してシングルサインオンをアクティブ化するには、"Enable single sign-on" を選択します。ポータルは、Azure Active Directory から適切な Datadog アプリケーションを取得します。アプリ名は、インテグレーションをセットアップするときに選択したエンタープライズアプリ名です。以下に示すように、Datadog アプリケーション名を選択します。

{{< img src="integrations/guide/azure_portal/sso.png" alt="Azure US3 シングルサインオン" responsive="true" style="width:100%;">}}

### API キー

左側のサイドバーで "Keys" を選択して、Datadog リソースの API キーのリストを表示します。

{{< img src="integrations/guide/azure_portal/api-keys.png" alt="Azure US3 API キー" responsive="true" style="width:100%;">}}

Azure ポータルは、API キーの読み取り専用ビューを提供します。キーを管理するには、"Datadog portal" リンクを選択します。Datadog で変更を加えた後、Azure ポータルビューを更新します。

Azure Datadog インテグレーションにより、Datadog Agent を VM またはアプリサービスにインストールできます。デフォルトのキーが選択されていない場合、Datadog Agent のインストールは失敗します。

## その他の参考資料

{{< partial name="whats-next/whats-next.html" >}}


[1]: /ja/integrations/azure/#create-datadog-resource
[2]: https://docs.microsoft.com/en-us/cli/azure/datadog?view=azure-cli-latest
[3]: /ja/integrations/azure/#metrics-and-logs
[4]: https://docs.microsoft.com/en-us/azure/azure-monitor/essentials/diagnostic-settings
[5]: /ja/serverless/azure_app_services
[6]: /ja/serverless/azure_app_services/#requirements