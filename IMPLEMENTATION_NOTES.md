# 実装メモ

## iOS Architecture

初期版はSwiftUI単体で実装する。

想定構成:

- `WasteType`
  - ゴミ種別
  - 表示名
  - 色
  - アイコン
- `CollectionRule`
  - ゴミ種別
  - 曜日
  - 周期
  - 前日通知時刻
  - 当日通知時刻
  - 有効/無効
- `ScheduleStore`
  - 端末内保存
  - JSONまたはSwiftData
- `NotificationScheduler`
  - ローカル通知の作成、更新、削除
- `MonetizationState`
  - 広告解除済みかどうか
  - 完了後広告を表示できるタイミングかどうか
- `AdPlacementController`
  - 下部バナー広告の表示制御
  - 完了後広告の表示制御
  - 起動直後、通知タップ直後の広告抑制
- `PurchaseManager`
  - 広告解除の買い切り購入
  - 購入状態の復元

## Notification Policy

iOSの通知は`UNUserNotificationCenter`を使う。

初回セットアップ完了時、または通知設定を有効化した時点で通知許可を求める。

通知文の例:

- 前日夜: `明日はプラスチックごみの日です`
- 当日朝: `今日は可燃ごみの日です`

## Monetization Policy

無料版では広告を表示するが、ゴミ出し確認を急ぐ体験を邪魔しない。

### Ad Placements

- Bottom banner
  - 今日画面、予定画面、設定画面の下部
  - タブバーの上に固定
  - 主要ボタンや確認チェックには重ねない
- Post-action ad
  - ゴミ出し完了チェック後
  - 週間設定の保存後
  - 通知時刻変更の保存後

### Suppression Rules

以下のタイミングでは広告を出さない。

- アプリ起動直後
- 通知タップ直後
- ゴミ出し予定を確認する前
- 初回オンボーディング中
- 通知許可ダイアログの前後

### Paid Removal

広告解除は買い切り360円のアプリ内課金にする。

解除後は以下を非表示にする。

- 下部バナー広告
- ゴミ出し完了後広告
- 設定保存後広告

実装ではStoreKit 2を使い、購入状態をアプリ起動時と設定画面表示時に復元する。

## MVP Scope

最初に作るもの:

- 週間カレンダー
- ゴミ種別スタンプ
- 周期編集シート
- 今日/明日の予定
- ローカル通知
- 下部バナー広告
- ゴミ出し完了後広告
- 広告解除の買い切り購入
- クリック可能なプロトタイプで画面遷移を検証

後回し:

- 地域自動設定
- ユーザーテンプレート共有
- 家族共有
- ウィジェット
- Apple Watch対応
- 広告ABテスト

## Interactive Prototype Behavior

`gomi-stamp-interactive.html`では、実アプリ実装前の画面遷移を確認する。

シミュレーション済み:

- 今日画面のゴミ出し完了チェック
- 完了画面への遷移
- 完了後広告の表示
- 360円広告解除の疑似購入
- 広告解除後の広告非表示
- 今日/予定/設定タブの切り替え

実装時はこの状態管理をSwiftUIの`Observable`なストアへ置き換え、広告解除はStoreKit 2の購入状態に接続する。

## GitHub Repository Name

候補:

- `gomi-stamp`
- `trash-stamp`
- `gomiday`

現時点では`gomi-stamp`を第一候補にする。
