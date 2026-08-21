---
# kimuz-web DESIGN.md — C:\dev\ の PWA / Web アプリ UI 用デザイン契約 v1
# 出典構造: google-labs-code/design.md（YAMLトークン＋順序固定の散文）を最小化。
# 使い方: 各プロジェクトの root に DESIGN.md としてコピー →「これに従って作って」と指示。
#         値を変えるのは下の YAML だけ。散文は「なぜ／どう適用するか」を述べるだけにする。
version: v1
name: kimuz-web
theme: light                 # 既定。dark を作る場合は colors を別ブロックで上書き
colors:
  surface:    "#FAFAF8"      # 背景（紙のような暖白）
  surface-2:  "#F1F0EC"      # カード・沈んだ面
  on-surface: "#1A1A1A"      # 本文インク（純黒 #000 は使わない）
  muted:      "#6B6B6B"      # 補助テキスト
  primary:    "#2F6F6A"      # 主要操作・リンク（落ち着いた teal）
  accent:     "#C2693B"      # アクセント1色＝CTA 限定
  border:     "#E5E5E0"      # hairline
  success:    "#2E7D52"
  warning:    "#B5860B"
  error:      "#B3261E"      # 純赤 #FF0000 は使わない
typography:
  family:  'system-ui, -apple-system, "Hiragino Sans", "Noto Sans JP", sans-serif'
  body:    { size: 16px, weight: "400", line: "1.7" }   # ≥16px（iOS の入力ズーム回避）
  caption: { size: 14px, weight: "400", line: "1.5" }
  title:   { size: 22px, weight: "700", line: "1.4" }
  display: { size: 28px, weight: "700", line: "1.3" }
spacing:
  unit: 4px                  # 4px グリッド。実用ステップ: 8 / 12 / 16 / 24 / 32
  page-padding: 16px
rounded:
  sm:   0.375rem
  md:   0.75rem              # ボタン・入力の既定（12px）
  lg:   1rem                 # カード・ボトムシート
  full: 9999px               # チップ・アバター
elevation:
  hairline: "1px solid {colors.border}"
  card:     "0 1px 4px rgba(40,60,60,0.12)"            # カード・主ボタンの控えめな影
  overlay:  "0 8px 24px rgba(0,0,0,0.12)"               # モーダル／シート専用
---

## Overview
落ち着いた・実用本位・モバイル前提・日本語 UI。情報は素直に縦に積み、装飾より可読性と片手操作を優先する。kimuz の個人ツール群で見た目をそろえるための共通契約であり、プロジェクトごとに `name` と `accent` だけ変えれば固有色を出せる。

## Colors
`on-surface` を本文、`muted` を補助に。主要操作・主ボタン・リンクは `primary`。**`accent` は小面積の強調だけ**（重要数値・選択中・ソロ表示など。希少性を保つ）。色だけで意味を伝えない（成功/警告/エラーはテキストかアイコンも併記）。純黒・純赤・原色べた塗りは使わない。

## Typography
`family` は OS の日本語システムフォント。本文は 15–16px。**入力欄は必ず 16px 以上**（iOS で入力にフォーカスすると自動ズームするのを防ぐ。これが 16px ルールの本来の目的）。`title`/`display` は太字でメリハリ。行間は本文 1.7 とゆったり。

## Layout
モバイルファースト。単一カラム基調、最大幅 480–640px で中央寄せ。外周は `page-padding` 16px。ブレークポイントは 1 つ（≥768px で必要なら 2 カラム）。主要操作は親指が届く下部に置く。

## Elevation & Depth
奥行きは `hairline`（1px ボーダー）基調。カード・主ボタンには控えめな影（`card`）まで可、モーダル・シートは `overlay`。濃い影・多重影は使わない。コントラストは WCAG-AA 以上、タッチターゲットは **44px 目標（最低 40px）**。

## Shapes
角丸は `md` 基調（ボタン・入力 `md`、カード・シート `lg`、チップ・アバター `full`）。鋭角は使わない。

## Components
色・寸法は必ずトークン参照で書き、生 hex を直書きしない。
- **Primary ボタン**: bg `{colors.primary}` / text `#fff` / radius `{rounded.md}` / min-height 44px（主要操作・1画面1つ。`accent` は使わない）
- **Secondary ボタン**: bg `{colors.surface}` / border `{elevation.hairline}` / text `{colors.primary}`
- **カード**: bg `{colors.surface-2}` / radius `{rounded.lg}` / border `{elevation.hairline}`
- **入力**: font-size `{typography.body.size}` / radius `{rounded.md}` / min-height 44px
- **ボトムシート/モーダル**: radius `{rounded.lg}` / shadow `{elevation.overlay}`

## Do's and Don'ts
- ✅ タッチターゲットは 44px 目標（最低 40px）。重要操作は下部・親指圏に。
- ✅ 入力フィールドは font-size 16px 以上（iOS 自動ズーム回避）。
- ✅ 画像/Blob は IndexedDB に **ArrayBuffer で保存**（iOS の Blob 不具合回避）。objectURL は使用直後に **1つずつ** revoke。
- ✅ 共有・コピー・カメラは https 前提。時間のかかる処理は必ず処理中インジケータを出す。
- ✅ 「完成」と言う前に実機 or 実ブラウザで操作確認（verify-in-real-browser）。
- ❌ 純黒 #000 / 純赤 #FF0000 / 原色べた塗りを UI・本文に使わない。
- ❌ テキストや CTA にグラデーションをかけない（背景の淡いグラデは可）。
- ❌ hover 前提の UI（タッチに hover は無い）。重要操作を hover 隠しにしない。
- ❌ objectURL の一括 revoke（表示中の画像が消える事故）。
- ❌ 濃い影・多重影・グロー乱用。控えめな影（`card`）は可だが、奥行きは hairline 基調。

## Known Gaps（対象外・正直に）
- 本書は C:\dev\ の **実用・フォーム型 PWA / Web アプリ UI 専用**。
- **没入系・キャンバス/ダーク系**（例: 水滴ピアノのような ASMR・アート系）は対象外。独自のデザイン言語を持つ（必要なら将来 dark/没入プリセットを別途追補）。
- 学術 HTML（レポート・図表）は `DESIGN.md` v1.0「Calm Academic」が正本。PPT は 15色システム（build-pptx）が正本。**混ぜない**。
- アニメーションの詳細・フォーム検証の細かな状態遷移・メール HTML は未規定（プロジェクト側で決める）。
