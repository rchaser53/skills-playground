# Copilot Instructions

このワークスペースでは、ユーザーへの最終回答を返すたびに、同じ内容をログファイルとして保存する。

## Logging Rules

- 設定はルートの output-log.config.yaml に従う。
- 最終回答の直前に、logs 配下へ新しいログファイルを1つ作成する。
- ファイル名は YYYYMMDD-HHMMSS.log 形式のタイムスタンプ名にする。
- ログには少なくとも次の内容を含める。
  - Timestamp
  - User Request
  - Final Output
- ログ内容はユーザーに返す最終回答と実質的に同じ内容にする。
- 途中経過のみで最終回答を返していない段階ではログを作らない。
- ログ作成に失敗した場合は、その旨を最終回答で短く明記する。