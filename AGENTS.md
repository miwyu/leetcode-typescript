# AGENTS.md

このファイルは、本リポジトリのコードを扱うAIコーディングアシスタントへのガイドラインです。常に日本語で応答してください。

## プロジェクト概要

TypeScript ベースの LeetCode ソリューションリポジトリです。厳密な型安全性と包括的なテストを備えています。

## 基本コマンド

### 開発コマンド

```bash
# 全テスト実行
npm test

# ウォッチモードでテスト実行（開発用）
npm run test:watch

# 特定の問題のテスト実行
npm test -- src/problems/0001-two-sum/solution.test.ts
npm test -- -t "Two Sum"

# TypeScript ビルド
npm run build

# コードフォーマット（コミット前に必須）
npm run format

# リント実行（CI で必須）
npm run lint

# 型チェック（CI で必須）
npm run typecheck

# ファイルを変更せずにフォーマットチェック
npm run format:check

# 全チェック実行（フォーマット、リント、型チェック、テスト）
npm run check
```

### CI 要件

コミット前に、以下がすべてパスすることを確認してください:

1. `npm run format:check` - フォーマットチェック
2. `npm run lint` - ESLint チェック
3. `npm run typecheck` - 型チェック
4. `npm test` - 全テスト

または、すべてのチェックを一括実行できます:

```bash
npm run check
```

## プロジェクト構成

### 問題の構成

各 LeetCode 問題は以下の構成に従います:

```
src/problems/[問題番号(4桁形式)]-[問題名]/
├── implementations/   # 各アルゴリズムの実装
│   ├── brute-force.ts # 総当たり法
│   ├── hash-map.ts    # 最適化されたアプローチ
│   └── ...            # その他の実装
├── solution.ts        # 最適なソリューションをインポート・エクスポート
└── solution.test.ts   # エッジケースを含む包括的なテスト
```

`implementations/` フォルダには各アルゴリズムアプローチごとに個別のファイルを格納します。`solution.ts` は最適なソリューションをデフォルトエクスポートするメインエントリーポイントです。

### TypeScript 設定構成（Solution Style）

- ルート `tsconfig.json`: 共有の compilerOptions のみ（`module/moduleResolution: "nodenext"`、strict フラグ、`types: ["node"]`）、`files: []`、および以下のサブプロジェクトへの `references` を定義します。`tsc -p tsconfig.json` を直接実行しないでください。
- `tsconfig.app.json`: アプリコードのビルドターゲット（`rootDir ./src`、`outDir ./dist`、`types: ["node"]`、テスト除外）。
- `tsconfig.vitest.json`: テスト専用ターゲット（`include: ["src/**/*.test.ts"]`、`types: ["vitest/globals", "node"]`、`noEmit: true`）。
- `tsconfig.node.json`: ツール/設定ターゲット（`include: ["vitest.config.ts"]`、`types: ["node"]`、`noEmit: true`）。
- コマンド対応: `npm run build` → アプリ、`npm run test`/`test:watch` → Vitest、`npm run typecheck` → アプリ + vitest + node。

### TypeScript 設定

本プロジェクトでは非常に厳密な TypeScript 設定を使用しています:

- `strict: true` ですべてのストリクトフラグを有効化
- `noUncheckedIndexedAccess: true` - 配列/オブジェクトアクセス時に undefined チェックが必須
- `exactOptionalPropertyTypes: true` - オプショナルプロパティへの undefined 代入を禁止
- `noImplicitOverride: true` - 明示的な override キーワードが必須

## テストガイドライン

### テストを変更しないこと

テストは仕様であり、提案ではありません。実装に合わせて既存のテストを変更しないでください。テストが失敗する場合は、テストではなく実装を修正してください。

### テスト変更が許可される場合

テストコードの変更が許可されるのは以下の場合のみです:

1. 新しいテストケースの追加を明示的に依頼された場合
2. テストインフラの修正を明示的に依頼された場合
3. シンタックスエラーの修正
4. フォーマットの適用
5. テスト仕様が矛盾している場合（独断で判断せず、確認を求めてください）

### テスト設計原則

1. 制約を定数（UPPER_SNAKE_CASE）としてテストファイルの先頭に定義します
2. 派生値をテスト内の変数（lowerCamelCase）として算出します
3. 最悪ケースのシナリオを考慮します（アルゴリズムに負荷をかける O(n²) や指数的なケースを特定します）
4. 単に「大きな」値ではなく、実際の制約境界値でテストします
5. 浮動小数点問題では数値精度を考慮します
6. テストの組み合わせを最適化します（冗長なテストを避けます）
7. 特定のテスト入力に最適化せず、アルゴリズムの効率性を最適化します

## 実装ガイドライン

### 実装設計原則

1. 各アプローチごとに `implementations/` フォルダに個別の実装ファイルを作成します
2. 各実装ファイルには、計算量分析を含む詳細な TSDoc を日本語で記述します
3. 重要なロジックや説明には日本語コメントを使用します
4. メインの `solution.ts` は実装からインポートしてエクスポートします:
   - デフォルトエクスポート: 最適なソリューション
   - 名前付きエクスポート: 比較用の代替アプローチ（必要な場合）

### 実装をテストデータから独立させること

- 特定のテスト値をハードコードする実装を書かないでください
- テストケースの期待入力をハードコードしないでください
- ソリューションは、テストデータだけでなく、制約内の任意の有効な入力に対して動作する必要があります

## コード品質基準

- Prettier によるフォーマット（シングルクォート、末尾カンマ）
- ESLint による TypeScript 推奨ルールおよび JSDoc/TSDoc 強制
  - Non-null アサーション（`!` 演算子）は禁止
  - solution.ts での関数の直接再エクスポートは、適切なドキュメントを確保するために禁止
- すべてのコードは厳密な型チェックをパスする必要があります

## 新しい問題の追加

新しい LeetCode 問題を実装する際の手順:

1. ディレクトリを作成: `src/problems/[番号(4桁形式)]-[名前]/`
2. `solution.test.ts` にすべての制約境界をカバーする包括的なテストを作成
   - テストの説明（describe、it ブロック）には日本語を使用
   - テストケースと制約の説明に日本語コメントを追加
3. 各アプローチ用に `implementations/` サブフォルダを作成:
   - 最もシンプルなソリューションとして `brute-force.ts` から開始
   - 最適化されたアプローチを追加（例: `hash-map.ts`、`two-pointers.ts`）
4. 各実装ファイルに日本語で完全な TSDoc ドキュメントを記述
   - 内部実装の TSDoc フォーマットに従う
   - 重要なロジックに日本語のインラインコメントを追加
5. 最適なアプローチをデフォルトエクスポートする `solution.ts` を作成
   - パブリック API の TSDoc フォーマットに従う（日本語）
6. コミット前にすべての CI チェックがパスすることを確認:
   - フォーマット: `npm run format`
   - 全チェック実行: `npm run check`
