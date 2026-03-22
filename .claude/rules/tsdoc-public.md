---
paths:
  - "src/problems/**/solution.ts"
---

# TSDoc の記述規約 - パブリック API エクスポート

各 solution.ts ファイルには、以下の仕様に従って日本語で詳細な TSDoc コメントを記述する必要があります。

焦点: **何をするのか、どう使うのか** - API 利用者のため

- 関数の簡潔な1行説明（日本語）
- `@param` タグに日本語の説明（型アノテーションなし）
- `@returns` タグに戻り値の形式を日本語で記述
- `@throws` タグは関数が実際に例外をスローする場合のみ追加（日本語）
- `@example` セクションに異なるケースを示す3つ以上の使用例（コメントは日本語）
- `@see` タグに LeetCode 問題の URL（TSDoc リンク形式、パイプ区切り）
- `@public` タグを付与

構造の例:

````typescript
/**
 * 配列内で合計が目標値となる2つの数値を見つける関数
 *
 * @param nums - 整数の配列
 * @param target - 目標となる合計値
 * @returns 合計が目標値となる2つの数値のインデックス [i, j]（i < j）
 * @throws 合計が目標値と一致するペアが見つからない場合はエラー
 *
 * @example
 * ```typescript
 * twoSum([2, 7, 11, 15], 9);  // nums[0] + nums[1] = 9 のため [0, 1] を返す
 * twoSum([3, 2, 4], 6);       // nums[1] + nums[2] = 6 のため [1, 2] を返す
 * twoSum([3, 3], 6);          // nums[0] + nums[1] = 6 のため [0, 1] を返す
 * ```
 *
 * @see {@link https://leetcode.com/problems/two-sum/ | LeetCode Problem}
 *
 * @public
 */
````
