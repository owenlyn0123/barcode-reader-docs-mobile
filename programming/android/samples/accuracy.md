---
layout: default-layout
title: Dynamsoft Barcode Reader for Android - Accuracy-First Settings Samples
description: This is the Accuracy-First Settings Sample page of Dynamsoft Barcode Reader for Android SDK.
keywords: android, samples, speed
needAutoGenerateSidebar: false
breadcrumbText: Accuracy-First Settings
---

# Android Accuracy-First Settings Sample

The Accuracy-First setting sample illustrates how to use DBR APIs to improve the barcode reading accuracy. Normally the barcode misreading is caused by decoding the terribly printed barcodes or blurry images. On this page, you can find the guides on how to deal with these issues to avoid misreading when using Dynamsoft Barcode Reader.

View the sample

- [Speed-first Settings Sample (Android Java)]()

## Regular Accuracy Settings

Normally, the misreading is caused by the low-quality source. The low-quality source can be terribly printed barcodes or blurry images/frames. These low-quality barcodes can be recognized and localized. However, the barcode reader will take a lot of time to further process these barcodes and it is difficult to guarantee the accuracy of the results. It would be better to optimize the barcode source rather than try decoding on these low-quality barcodes.

### Frame Filter

Generally, the quality of input video streaming is determined by the performance of hardware. However, we still have solutions to deal with the blurry frames in the video streaming from the software end. `Dynamsoft Camera Enhancer` (DCE) is an SDK that provides video streaming pre-processing APIs which enable users to avoid decoding barcodes on low-quality frames. By enabling the frame filter feature of DCE, the sharpness of each frame will be detected and the low sharpness frames will be skipped in the barcode decoding process.

### Confidence

For every barcode result returned by DBR, it has a confidence value. The higher the confidence is, the more possible it is to be correct. Currently, the barcode reader will always return the barcode result with the highest confidence and the confidence filter is enabled as well. The default value of the confidence filter is 30，which means the result with lower confidence will not be returned.

**Related APIs**

`MinResultConfidence`

### Multi-frame Confirmation

The multi-frame confirmation is a solution that eliminates misreading on oneD barcodes by double-checking the barcode results between multiple video frames before the results are returned. Since this feature will halve the average reading speed, please enable this feature when the misreading is intolerable.

- Use [`enableResultVerification`]({{ site.android_api }}primary-result.html#enableresultverification) to enable the result confirmation.

## Advanced Accuracy Settings

### Specify the Barcode Format

Barcode format is one of the most typical settings of a barcode reader. This will help you to improve the speed and accuracy of your barcode reading program by excluding the uninterested formats. You can update the barcode format settings in the struct/class `PublicRuntimeSettings` via API `updateRuntimeSettings`.

**Related APIs**

- Struct/class `PublicRuntimeSettings`
- API `BarcodeReader.updateRuntimeSettings`
- Enum `BarcodeFormat` and `BarcodeFormat_2`

### Skip Small-Sized Barcodes

It is always a harsh task for a generally configured barcode reader to recognize a small-size barcode. DBR has `scaleupMode` which is specially designed for improving the read rate of small-size barcodes but contributes little to the accuracy. As a result, if your decoding program is designed for general usage, it is suggested to skip decoding on these small-size barcodes. You can configure the `FormatSpecification` parameters `BarcodeHeightRangeArray` and `BarcodeWidthRangeArray` to define the smallest acceptable barcode size of your barcode reading program. In addition, by configuring the range of barcode height and width, you can also filter out some incompatible shaped barcodes before decoding.

**Related APIs**

- Parameters `BarcodeHeightRangeArray` and `BarcodeWidthRangeArray`.

### Exclude the Non-interest Results

For some scenarios, there might have some common features that can be applied to verify the correctness of the barcode result texts.

**The Text Length**

Set the minimum and maximum length of the barcode text to filter out the uninterested results. This feature can be configured via `FormatSpecification` parameter [`BarcodeTextLengthRangeArray`]({{ site.parameters_reference }}format-specification/format-control.html#barcodetextlengthrangearray).

**The Regular Expression Pattern**

Set the regular expression pattern of the barcode text to filter out the uninsterested results. This feature can be configured via `FormatSpecification` parameter [`BarcodeTextRegExPattern`]({{ site.parameters_reference }}format-specification/format-control.html#barcodetextregexpattern).
