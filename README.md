# OkadaField 1.0.0

**対応環境：Windows 64-bit (x64) のみ**  
**表示言語：日本語のみ**

OkadaField は，Okada (1992) の有限矩形断層 `DC3D` に基づき，半無限均質弾性体中の変位・歪み・応力変化・Coulomb 応力変化・傾斜を可視化する研究・教育向けツールです。断層運動だけでなく，opening 成分を用いたダイク・シルも扱えます。

`OkadaField.exe` をダブルクリックすると localhost 上の GUI が既定ブラウザで開きます。数値計算は PC 内で完結します。緯度経度モードで地理院背景を選んだ場合だけ，ブラウザが国土地理院の地理院タイルへアクセスします。

重要な解析では，入力値・符号規約・単位を確認し，必要に応じて独立な計算との照合を行ってください。

## 主な機能

- 1 枚の有限矩形ソース：任意断層，逆断層例，正断層例，横ずれ断層例，ダイク，シル
- strike / dip / length / width / strike-slip / dip-slip / opening を任意指定
- 水平面（地表または任意深度）と East–Depth / North–Depth 鉛直断面
- 変位：East, North, Up, horizontal magnitude, 3-D magnitude
- 歪み：εEE, εNN, εUU, εEN, εEU, εNU, dilatation
- 応力変化：ΔσEE, ΔσNN, ΔσUU, ΔσEN, ΔσEU, ΔσNU
- receiver fault 上の Δτ, Δσn, ΔCFF
- 応力表示のカラースケール：共通 / 自動 / 任意の ±MPa 指定
- Poisson 比 ν と剛性率 μ (GPa) から第1 Lamé 定数 λ を自動計算
- receiver strike / dip / rake と有効摩擦係数 μ′ を指定可能
- 「受け手断層をソース断層に合わせる」で source strike / dip / rake を自動利用
- 傾斜：∂Uup/∂E, ∂Uup/∂N
- 変位ベクトル重ね描き
- プロット領域の距離スケールを縦横等倍（1:1）で表示し，中心に対して対称なベクトル配置
- 緯度経度入力と地理院地図上への重ね描き
- 計算範囲：基準点から ±5 km / ±50 km / ±500 km
- PNG / CSV 出力
- JSON 設定ファイルの保存・読込

詳細な変更履歴は `RELEASE_NOTES.md` を参照してください。

## 実行方法

1. `OkadaField.exe` をダブルクリックします。
2. 既定ブラウザに OkadaField の画面が開きます。
3. パラメータを入力して「計算して描画」を押します。
4. 終了するときは画面右上の「終了」ボタンを押します。ブラウザのタブやウィンドウを閉じた場合も，ブラウザとの通信が約 90 秒途切れると OkadaField 本体が自動終了します。

実行ファイルはコード署名していないため，Windows SmartScreen が警告を表示する場合があります。

## 単位と座標

### ローカル座標モード

- 水平・深さ・length・width：km
- DISL1 / DISL2 / DISL3：m
- 表示変位：mm
- 表示歪み：microstrain
- 表示応力変化 / Coulomb 応力変化：MPa
- 表示傾斜：µrad
- `z` は上向きを正とし，観測深さ `d` km では `z=-d`
- strike は北から時計回り
- 右手系 (RHR) として dip direction = strike + 90°

### 弾性定数・応力変化

OkadaField では Poisson 比 `ν` と剛性率 `μ` (GPa) を入力します。等方線形弾性体として第1 Lamé 定数 `λ` は

`λ = 2 μ ν / (1 - 2ν)`

から自動計算します。歪みテンソルから応力変化を

`Δσij = 2 μ εij + λ tr(ε) δij`

で計算します。応力の符号は **引張を正** としています。

### Coulomb 応力変化

receiver fault の strike / dip / rake を指定し，応力テンソルをその面上へ分解します。

- `Δτ > 0`：指定した receiver rake 方向のすべりを促進
- `Δσn > 0`：引張 / unclamping
- `ΔCFF = Δτ + μ′ Δσn`

`μ′` は GUI で任意に入力できる **有効摩擦係数**です。間隙流体圧変化を別変数として陽に計算しているわけではありません。

`受け手断層をソース断層に合わせる` を ON にした場合，receiver strike / dip は source と同一，receiver rake は `atan2(DISL2, DISL1)` から決めます。したがって，+DISL2 の逆断層は rake = +90°，-DISL2 の正断層は rake = -90°，+DISL1 の左横ずれは rake = 0° になります。

### 緯度経度モード

断層基準点 `(lat0, lon0)` と観測点 `(lat, lon)` の差を GRS80 楕円体上の Hubeny 近似で局所 East/North 距離へ変換して計算します。

平均緯度 `φm=(φ+φ0)/2` における子午線曲率半径 `M` と卯酉線曲率半径 `N` を用いて，

- `North = M(φm) (φ-φ0)`
- `East  = N(φm) cos(φm) (lon-lon0)`

としています。

局所〜地域スケールの変形可視化を想定した近似です。±500 km のような広域では誤差が増えるため，定量解析では測地線計算や ECEF/ENU 変換の利用を検討してください。緯度経度モードは現在 **水平面表示**に対応しています。鉛直断面はローカル座標モードで使用してください。

## 矩形ソースの与え方

GUI では矩形中心を基準点として，

- `AL1 = -length/2`
- `AL2 = +length/2`
- `AW1 = -width/2`
- `AW2 = +width/2`
- `DEPTH = 矩形中心 (AW=0) の深さ`

とします。

Okada の符号規約を用います。0 < dip < 90° では，

- +DISL1 = left-lateral
- +DISL2 = reverse
- -DISL2 = normal
- +DISL3 = opening

です。

## 地図上の断層表示

出力図では表示を簡潔にするため，

- 断層面の水平投影：破線
- 矩形中心 (AW=0)：`+` 印

のみを表示します。上端 / 下端，strike，dip，dip direction などの文字情報は図中には表示しません。これらの値は GUI の入力欄で確認できます。

## 設定ファイル

「設定を保存 (.json)」では，ソース・弾性定数・receiver・座標モード・計算範囲・grid size・観測深さ / 断面・表示量・変位ベクトル・地図背景・カラースケール設定を保存します。「設定を読込」で値を復元した後に自動計算します。

`examples/normal_fault_geo.json` に緯度経度モードの正断層例を同梱しています。

## 数値計算と検証

本公開パッケージは実行形式を配布します。ソースコードは本配布物に含めていません。数値計算は Okada (1992) に基づく DC3D 有限矩形ソース解をベースとしています。元の DC3D Fortran コードおよびマニュアルは，防災科研の公式公開ページを参照してください。

- DC3D official page (NIED): https://www.bosai.go.jp/information/dc3d.html

DC3D manual Appendix-2 のテスト例に対し，移植した数値カーネルの変位出力が元 Fortran と float32 出力精度内で一致することを確認しています。また，200 組のランダムな非特異パラメータについて 12 出力成分を比較し，設定した許容差内で一致することを確認しています。

応力・Coulomb 応力については，(1) `ν=0.25, μ=30 GPa` で `λ=30 GPa` となること，(2) receiver 面へ与えた既知の normal/shear stress が Δσn, Δτ, ΔCFF に正しい符号で分解されること，(3) 地表 `z=0` で自由表面の traction 成分 ΔσUU, ΔσEU, ΔσNU が数値誤差範囲で 0 になることをテストしています。

## 地図背景について

緯度経度モードで「地理院 淡色地図」または「地理院 標準地図」を選ぶと，ブラウザが国土地理院の XYZ 地理院タイルを読み込みます。地図の出典は描画内に「地理院タイル」と表示します。

ネットワーク接続がない場合やタイル取得に失敗した場合でも，計算結果そのものは背景なしで表示されます。

## 制限

- **Windows 64-bit (x64) のみ対応**
- **GUI は日本語のみ**
- 単一の矩形ソースのみ
- 半無限・均質・等方弾性体
- 応力変化は入力した一定の ν, μ を用いた線形弾性計算
- ΔCFF は指定 receiver 面への静的応力変化の分解であり，間隙流体圧変化そのものは別途計算しない
- 地形，成層構造，粘弾性は未対応
- 緯度経度モードの鉛直断面は未対応
- Hubeny 近似は局所〜地域スケール向け
- 断層端・断層面上の特異点は無効点として扱う
- 実行ファイルは未署名

## Reference / citation

Okada, Y. (1992), Internal deformation due to shear and tensile faults in a half-space, *Bulletin of the Seismological Society of America*, **82**, 1018–1040.

OkadaField を研究・教育資料で利用する場合は，上記 Okada (1992) を引用してください。OkadaField 自体の引用情報は `CITATION.cff` を参照してください。

## Feedback / contact

OkadaField に関する不具合報告，ご意見，機能追加のご要望を歓迎します。

- Bug reports / feature requests: GitHub Issues
- Contact: mit [at] shizuoka.ac.jp

## Copyright

Copyright (c) 2026 Yuta Mitsui. All rights reserved.
