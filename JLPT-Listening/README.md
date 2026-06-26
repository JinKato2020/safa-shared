# JLPT-Listening 共有層（safa-shared）

**まいにちJLPT ↔ 聞いて話せる(App B) の2アプリ専用の共有コード/データ。**
両アプリが本リポジトリを **git submodule** として取り込み、**同一ファイルを共有**する（コピー/同期ではない）。

> App A / App C はこの共有層を使わない（App B は A/C から完全独立・2026-06-26方針）。

## 中身
- `design/` … 共有デザインシステム（色トークン/テーマ/汎用部品/日次進捗フック）。
  - `tokens.ts` `theme.tsx` `components.tsx` `useDailyProgress.ts` `index.ts`
  - 使い方: `import { DesignThemeProvider } from '<submodule>/JLPT-Listening/design'`。ルートで `<DesignThemeProvider scheme={scheme}>`。
- `dict/` … 共有辞書。
  - `dictRemote.ts` … 辞書の取得＋端末キャッシュ層（`DICT_BASE_URL` から fetch・`loadSharedDict()`/`syncDictCache()`・web対応・オフラインfallback）。アプリ固有依存なし＝そのまま共有可。
  - `data/ja-*.json` … 共有辞書データ正本（語彙8033/漢字1974/類義/例文/漢字例文＋manifest）。出典 JMdict・KANJIDIC2（EDRDG, CC BY-SA）／日本語WordNet（NICT）＝**謝辞画面に帰属表示必須**。
  - ランタイム配信: 同データを GitHub Pages `https://jinkato2020.github.io/safa-JLPT/dict/` でも配信中。アプリは通常 dictRemote 経由でURL取得＋キャッシュ。

## 共有しないもの（各アプリ固有）
- 聴解音声 / 設定画面 / ホーム画面 / 各アプリのロジック・文言・状態。

## 正本・更新
- 正本ビルドは まいにちJLPT 側 `data-build/`。辞書データ更新時はこの submodule へ反映し、両アプリが pull。
- ⚠️ 各アプリは submodule 内を直接書き換えない（共有リポへ commit して両者へ反映）。
