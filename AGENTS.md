# ComposeLab の開発ガイド

ComposeLab は Kotlin と Jetpack Compose を使う Android アプリです。アプリのコードとテストは `app` にあります。依存関係のバージョンは `gradle/libs.versions.toml` で管理します。

## サブエージェントとの分担

- 親エージェントが要件整理、設計の確定、作業の切り分け、差分の統合、最終検証を担当する。
- 設計・実装の依頼では、独立して進められる調査、実装、レビューがある場合にサブエージェントを使う。小さな変更や順序依存の強い作業は親がまとめて進める。
- 同時に使うサブエージェントは原則2つまでとする。各エージェントに目的、担当範囲、期待する成果、必要な検証を伝える。調査とレビューは読み取り専用とし、根拠となるファイルや行番号を添えて簡潔に報告させる。
- 実装を並行させる場合は編集対象のファイルを分ける。共有ファイルに変更が必要なら親が取り込む。ビルドやテストは、同じ作業ディレクトリでの重複実行を避け、親がまとめて行う。

## ローカル環境と検証

- Gradle Wrapper を使う。Gradle デーモンには JDK 21、コンパイルには Android SDK 37 が必要。
- Android Studio でプロジェクトを開き、SDK Manager で Android SDK 37 を用意する。端末ごとの SDK パスは `local.properties` で管理し、コミットしない。
- ターミナルでは `JAVA_HOME` に JDK 21 を指定する。macOS で標準の場所に Android Studio をインストールしている場合は、同梱の JDK 21 を次のように指定できる。

  ```sh
  export JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home"
  ```

- 変更に応じて必要なチェックを選ぶ。デバッグビルド、単体テスト、Lint をまとめて実行するには次のコマンドを使う。

  ```sh
  ./gradlew :app:assembleDebug :app:testDebugUnitTest :app:lintDebug
  ```

- Android のインストルメンテーションテストには実機またはエミュレーターを接続し、`./gradlew :app:connectedDebugAndroidTest` を実行する。
