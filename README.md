# "透明化全脳画像の情報解析パイプライン" 補足資料

本レポジトリーでは、実験医学別冊 "ライトシート顕微鏡活用ガイド" に掲載された総説"透明化全脳画像の情報解析パイプライン"(真野智之著)の補足資料を公開している。

## コンテンツ

* `README.md` -- この文章
* `whole_brain_analysis_example.ipynb` -- 全脳データの定量化・グラフ化のためのプログラム例 (Python)

## コマンド

総説の中で解説したコマンドを以下に示す。
コピーアンドペーストなどの目的で役立ててほしい。

```bash
$ export fixImg="./average_template_50.nii"

$ export movImg="./532nm_50.nii"

$ export outDir="./registration_result"

$ antsRegistration --dimensionality 3 --float 0 --interpolation Linear --winsorize-image-intensities [0.05,1.0] --use-histogram-matching 0 --verbose 1 --output [$outDir/F2M_,$outDir/Warped.nii.gz,$outDir/InvWarped.nii.gz] --transform Affine[0.1] --metric MI[$fixImg,$movImg,1,128,Regular,0.5] --convergence [1000x1000x1000,1e-5,15] --shrink-factors 8x4x2 --smoothing-sigmas 3x2x1vox --transform SyN[0.1,3.0,0.0] --metric CC[$fixImg,$movImg,1,4] --convergence [500x500x100x20,1e-6,10] --shrink-factors 8x4x2x1 --smoothing-sigmas 3x2x1x0vox
```

```bash
$ export InputCsv=”./cells.csv”

$ export OutputCsv=”./cells_transformed.csv”

$ antsApplyTransformsToPoints -d 3 -i $InputCsv -o $OutputCsv -t [$outDir/F2M_0GenericAffine.mat,1] -t $outDir/F2M_1InverseWarp.nii.gz
```
