---
title: LCPを縮める最初の一手は「ヒーロー画像のpreload」
emoji: "\U0001F680"
type: tech
topics:
  - Performance
  - CoreWebVitals
  - フロントエンド
published: true
---

LCP（Largest Contentful Paint）が遅いページの多くは、ファーストビューの大きな画像が原因になっています。最初の一手として効くのが、その画像の `preload` です。

## なぜ効くのか

ブラウザは HTML を上から解析します。CSS の中の `background-image` や後段にある `img` は、発見が遅れて読み込みが後回しになりやすいです。`preload` を `head` の早い位置に置くと、ブラウザは画像を早期に取得できます。

```html
<link rel="preload" as="image" href="/hero.avif" type="image/avif" />
```

レスポンシブで複数候補があるなら `imagesrcset` を併用します。

```html
<link
  rel="preload"
  as="image"
  imagesrcset="/hero-800.avif 800w, /hero-1600.avif 1600w"
  imagesizes="100vw"
/>
```

## 計測してから入れる

preload は強力ですが、入れすぎると他のリソースと帯域を奪い合います。Lighthouse や WebPageTest で LCP 要素を特定し、その1枚だけに絞るのが安全です。効果は実機で計測して確かめてください。
