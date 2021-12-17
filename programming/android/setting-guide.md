---
layout: default-layout
title: Dynamsoft Barcode Reader for Android - Advanced Usage
description: This is the Advanced Usage of Dynamsoft Barcode Reader for Android SDK.
keywords: Guide, Android, Settings
needAutoGenerateSidebar: true
needGenerateH3Content: true
noTitleIndex: true
---

# Advanced Usage

`PublicRuntimeSettings` is the struct that inclueds the majority of available barcode decoding settings. There is a series of methods for you to get, optimize or even reset the settings. On this page, you will be guide on how to initialize, optimize or reset the barcode decoding settings.

## Settings initialization

You can initialize the parameter settings for your Android barcode reader with a template or simply start with the default settings. When the template is added to the project, you can still add additional settings to the project to further optimize the performance of the barcode reader to cover your requirements.

### Initialize with Preset Template

The preset templates are listed as follow:

| Template Name | Description |
| ------------- | ----------- |
| `DEFAULT` | Reset to default barcode decoding parameters. |
| `VIDEO_SINGLE_BARCODE` | Switch to single barcode decoding template. The parameter settings will be optimized to reach a higher speed on processing single barcode. |
| `VIDEO_SPEED_FIRST` | Switch to speed first template for video streaming barcode decoding. The barcode processing speed will be improved for the majority of video barcode decoding scenarios. |
| `IMAGE_SPEED_FIRST` | Switch to speed first template for image barcode decoding. The barcode processing speed will be improved for the majority of video barcode decoding scenarios. |
| `VIDEO_READ_RATE_FIRST` | Switch to read rate first template for video streaming barcode decoding. The barcode reader will be able to successfully decode on the majority of barcodes when decoding from the video streaming. |
| `IMAGE_READ_RATE_FIRST` | Switch to read rate first template for image barcode decoding. The barcode reader will be able to successfully decode as many barcodes as possible from a single image. |

The preset templates can be update via method [`updateRuntimeSettings`](api-reference/primary-parameter-and-runtime-settings-basic.md#with-a-preset-template). When a new template is selected, all the previous barcode parameter settings are reset to the newly select template values.

### Initialize with Customized Template

Dynamsoft Technical Support can help you on providing a customized barcode decoding template. Feel free to <a href="https://www.dynamsoft.com/Company/Contact.aspx" target="_blank">contact us</a> when:

- The performance is not satisfying.
- The templates are puzzling
- You have specific requirements on the performance

The template will be a JSON data that can be uploaded via method `initRuntimeSettingsWithFile` and `initRuntimeSettingsWithString`. Similar with the preset templates, you can add additional settings to optimize the performance. The following information will help us provide you with a better template.

- The targeting barcode formats.
- The device information (hardware model, software version).
- The barcode scanning distance.
- Sample image or video for the usage scenarios.

```java
BarcodeReader reader = new BarcodeReader();
// Input a stringified JSON data in the method to initialize the template.
// You can set whether to inherit or overwrite the previous settings.
reader.initRuntimeSettingsWithString("{\"Version\":\"3.0\", \"ImageParameter\":{\"Name\":\"IP1\", \"BarcodeFormatIds\":[\"BF_QR_CODE\"], \"ExpectedBarcodesCount\":10}}", EnumConflictMode.CM_OVERWRITE);
reader.destroy();
```

## Settings update

The templates are designed to simplify the parameter settings. It might not cover the requirements. Modes parameters and other settings like barcode formats are available in the `PublicRuntimeSettings` class. You can use the following steps to change the settings. When you update the settings, only the newly configured settings will be changed and the other settings will remain the same.

**Step 1. getRuntimeSettings**

Use the following code to get the current settings.

```java
PublicRuntimeSettings runtimeSettings = barcodeReader.getRuntimeSettings();
```

The `runtimeSettings` you got from `getRuntimeSettings` is an object of `PublicRuntimeSettings`. In the object, there are the settings you previously updated via templates. You can view and directly change the parameter values in the `PublicRuntimeSettings` object.

**Step 2. Optimize the required settings**

Since you have got the `runtimeSettings`, you can add the following code to change the settings in the object. Here, `BarcodeFormats` and `LocalizationModes` will be illustrated as an example for optimizing the settings.

```java
PublicRuntimeSettings runtimeSettings = barcodeReader.getRuntimeSettings();

// You can specify the barcode formats via runtime settings.
runtimeSettings.barcodeFormatIds = EnumBarcodeFormat.BF_ONED | EnumBarcodeFormat.BF_QR_CODE;

// LocalizationModes is the modes parameter that controls how the barcodes will be located.
// Localization mode "scan directly" and "connected blocks" are enabled here.
// You can enable at most 8 modes for each modes parameter.
runtimeSettings.localizationModes = new int[]{EnumLocalizationMode.LM_SCAN_DIRECTLY,EnumLocalizationMode.LM_CONNECTED_BLOCKS};
```

**Step 3. updateRuntimeSettings**

When you have alreay completed the settings, you can update the settings via method `updateRuntimeSettings`. All the settings you have configured will be updated to the barcode reader and the other settings will remains the same.

```java
PublicRuntimeSettings runtimeSettings = barcodeReader.getRuntimeSettings();
runtimeSettings.barcodeFormatIds = EnumBarcodeFormat.BF_ONED | EnumBarcodeFormat.BF_QR_CODE;
runtimeSettings.localizationModes = new int[]{EnumLocalizationMode.LM_SCAN_DIRECTLY,EnumLocalizationMode.LM_CONNECTED_BLOCKS};
// Update all above settings.
reader.updateRuntimeSettings(runtimeSettings);
```

Please notes, the upload of the settings will fail if there is an invalid value in the newly added settings, which means none of the settings will be updated.
