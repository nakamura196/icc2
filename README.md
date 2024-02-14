# IIIF Multi Viewer

IIIF Collection と IIIF Curation List を引数として、ファセット検索や画像比較などの機能を提供します。

## 使い方

### 基本

以下にアクセスしてください。

https://nakamura196.github.io/icc2/

フォームに IIIF Curation List の URL を入力してください。

例: https://mp.ex.nii.ac.jp/api/curation/json/528810d2-4e28-4a46-910c-c9b517f86943

<img width="400" alt="demo" src="https://github.com/nakamura196/icc2/assets/5351691/76b34f19-e88b-4872-bda2-01bbfe79a044">

または、IIIF Curation List を IIIF Curation Viewer などで参照する URL (`?curation=`を含む URL)を入力してください。

例: http://codh.rois.ac.jp/software/iiif-curation-viewer/demo/?curation=https://mp.ex.nii.ac.jp/api/curation/json/528810d2-4e28-4a46-910c-c9b517f86943&lang=ja

https://github.com/nakamura196/icc2/assets/5351691/57543695-8928-4eda-a433-7ad58bc5ddd1

![demo](https://github.com/nakamura196/icc2/assets/5351691/99b3ffb6-59f2-4d48-8d6c-ff00733f9efe)

<!-- 以下のURLに引数 `u` に IIIF Collection URI または IIIF Curation URI を与えてください。 -->

### その他

IIIF Curation List の URL に加えて、IIIF Collection の URI も入力可能です。

または以下のように、引数 `u` に IIIF Curation List または IIIF Collection の URL を与えてください。

例: https://nakamura196.github.io/icc2/?u=https://mp.ex.nii.ac.jp/api/curation/json/528810d2-4e28-4a46-910c-c9b517f86943

## 例

- 百鬼夜行図（IIIF Curation List）
  - https://nakamura196.github.io/icc2/?u=https://mp.ex.nii.ac.jp/api/curation/json/528810d2-4e28-4a46-910c-c9b517f86943
