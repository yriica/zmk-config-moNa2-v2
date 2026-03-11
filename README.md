# zmk-config-moNa2-v2

[moNa2-v2](https://github.com/sayu-hub/zmk-config-moNa2.0) 用の ZMK ファームウェア設定リポジトリです。

## 概要

| 項目 | 内容 |
|------|------|
| キーボード | moNa2-v2（42キー分割キーボード） |
| MCU | Seeeduino XIAO BLE |
| トラックボールセンサ | PAW3222（19mm トラックボール変換キット） |
| ZMK バージョン | v0.3.0 |
| ZMK Studio | 有効 |

## レイヤー構成

| レイヤー | 用途 |
|----------|------|
| 0 (default) | 通常入力 |
| 1 | 数字・記号 |
| 2 | ウィンドウ操作・テンキー |
| 3 | マウス・BT 接続管理・矢印キー（トラックボールスクロールモード） |
| 4 | 予備 |
| 5 | 予備 |

## トラックボール設定

- **センサ**: PAW3222（`mona2_r.overlay` で設定）
- **CPI**: 800
- **軸補正**: X軸・Y軸ともに反転（`mona2.dtsi` の `trackball_central_listener`）
- **スクロールモード**: Layer 3 ホールド中にトラックボールでスクロール操作（X軸反転で自然な方向に対応）

## ビルド

GitHub にプッシュすると GitHub Actions で自動ビルドされます。
ビルド成果物（`.uf2` ファイル）は Actions の Artifacts からダウンロードできます。

### ビルド対象

| ボード | シールド | 備考 |
|--------|----------|------|
| seeeduino_xiao_ble | mona2_r + rgbled_adapter | 右手（セントラル）+ ZMK Studio |
| seeeduino_xiao_ble | mona2_l + rgbled_adapter | 左手（ペリフェラル） |
| seeeduino_xiao_ble | settings_reset | 設定リセット用 |

## ファイル構成

```
config/
  mona2.keymap          # キーマップ定義
  mona2.json            # Keymap Editor 用レイアウト定義
  mona2_r.conf          # 右手側設定（エンコーダ、バッテリー、LED、ZMK Studio、BT）
  mona2_l.conf          # 左手側設定（エンコーダ、バッテリー）
  west.yml              # ZMK モジュール定義（PAW3222 ドライバ等）
boards/shields/mona2/
  mona2.dtsi            # 共通デバイスツリー（マトリクス、トラックボールリスナー）
  mona2_r.overlay       # 右手側オーバーレイ（SPI、PAW3222、スクローラー）
  mona2_l.overlay       # 左手側オーバーレイ
  Kconfig.defconfig     # Kconfig 設定
  Kconfig.shield        # シールド定義
  mona2.zmk.yml         # ZMK メタデータ
```

## 外部モジュール

| モジュール | 用途 |
|------------|------|
| [zmk-driver-paw3222](https://github.com/sekigon-gonnoc/zmk-driver-paw3222) | PAW3222 トラックボールドライバ |
| [zmk-rgbled-widget](https://github.com/caksoylar/zmk-rgbled-widget) | RGB LED バッテリー・レイヤー表示 |
| [zmk-input-processor-keybind](https://github.com/zettaface/zmk-input-processor-keybind) | 入力プロセッサ拡張 |
