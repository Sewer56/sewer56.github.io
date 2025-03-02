---
date: 2025-03-30
comments: true
draft: true
categories:
  - Compression
  - Texture Compression
  - Texture Compression in Nx2.0
---

# BC7 Transform

!!! info "Will my transform make data more compressible?"

Part 4 of [Texture Compression in Nx2.0 series][compression-nx20] series.

<!-- more -->

## Block Distribution

The distribution of said modes is roughly similar to this:

<figure markdown="span">
    ![block-distribution](./assets/bc7-block-distribution.webp){ width="600" }
    <figcaption>BC7 Block Distribution for the [following mods][test-data]</figcaption>
</figure>

| Mode | Percentage |
| ---- | ---------- |
| 0    | 46.90%     |
| 1    | 34.54%     |
| 2    | 12.36%     |
| 3    | 2.44%      |
| 4    | 2.39%      |
| 5    | 0.39%      |
| 6    | 0.98%      |
| 7    | 0.01%      |

For brevity, we will skip the actual details of how the BC7 format works today, but it is for the
most part based on the exact same [concepts as BC1](./bc1-compression.md#anatomy-of-the-bc1-dxt1-block).

You can think of `mode0` as a variant of BC1 where:

- There's 3 sets of `color0+color1` to blend. (unlike 1 set in BC1)  
- Which colours are blended is stored in `partition` field.  
    - Each `partition` is a preset known by decoder.  

---

*The sources and documentation for this project can be found at:*
- [GitHub Repository](https://github.com/Sewer56/lossless-transform-utils)
- [Documentation](https://docs.rs/lossless-transform-utils)




[bc1-compression]: ./bc1-compression.md
[compression-nx20]: ../category/texture-compression-in-nx20.md
[previous post in the series]: ./bc1-compression.md
[bc1-anatomy]: ./bc1-compression.md#anatomy-of-the-bc1-dxt1-block
[YCrCb]: https://www.computerlanguage.com/results.php?definition=YCrCb
[YCoCg-R]: https://en.wikipedia.org/wiki/YCoCg
[LZ matches]: ./bc1-compression.md#step-a-matching-patterns-in-previous-data
[test-data]: ./bc1-compression.md#a-quick-demo