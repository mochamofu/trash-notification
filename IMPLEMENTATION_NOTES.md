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

## Notification Policy

iOSの通知は`UNUserNotificationCenter`を使う。

初回セットアップ完了時、または通知設定を有効化した時点で通知許可を求める。

通知文の例:

- 前日夜: `明日はプラスチックごみの日です`
- 当日朝: `今日は可燃ごみの日です`

## MVP Scope

最初に作るもの:

- 週間カレンダー
- ゴミ種別スタンプ
- 周期編集シート
- 今日/明日の予定
- ローカル通知

後回し:

- 地域自動設定
- ユーザーテンプレート共有
- 家族共有
- ウィジェット
- Apple Watch対応

## GitHub Repository Name

候補:

- `gomi-stamp`
- `trash-stamp`
- `gomiday`

現時点では`gomi-stamp`を第一候補にする。
