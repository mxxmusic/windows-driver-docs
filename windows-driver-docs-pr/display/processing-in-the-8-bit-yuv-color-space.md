---
title: Processing in the 8-bit YUV Color Space
description: Processing in the 8-bit YUV Color Space
ms.assetid: fbf62dc6-b5bf-43f6-baa8-c6d1cee80f9b
keywords: ["ProcAmp WDK DirectX VA , YUV color space", "YUV formats WDK DirectX VA", "Y processing WDK DirectX VA", "UV processing WDK DirectX VA", "color space conversions WDK DirectX VA"]
---

# Processing in the 8-bit YUV Color Space


## <span id="ddk_processing_in_the_8_bit_yuv_color_space_gg"></span><span id="DDK_PROCESSING_IN_THE_8_BIT_YUV_COLOR_SPACE_GG"></span>


Working in the YUV color space simplifies the calculations involved for ProcAmp adjustment control of a video stream.

### <span id="Y_Processing"></span><span id="y_processing"></span><span id="Y_PROCESSING"></span>Y Processing

To perform ProcAmp adjustment for the Y component, subtract 16 from the Y value to position the black level at zero. This removes the DC offset so that adjusting the contrast does not vary the black level. Because Y values might be less than 16, negative Y values should be supported at this point. Contrast is adjusted by multiplying the YUV pixel values by a constant. If U and V are not adjusted, a color shift will result whenever the contrast is changed. The brightness property value is added (or subtracted) from the contrast adjusted Y values; this is done to avoid introducing a DC offset due to adjusting the contrast. Finally, the value 16 is added to reposition the black level at 16.

The following equation summarizes the steps described in the previous paragraph. C is the contrast value and B is the brightness value.

```
Y&#39; = ((Y - 16) x C) + B + 16
```

### <span id="UV_Processing"></span><span id="uv_processing"></span><span id="UV_PROCESSING"></span>UV Processing

To perform ProcAmp adjustment for the U and V components, subtract 128 from both U and V values to position the range around zero. The hue property is implemented by mixing the U and V values together as shown in the following equations. H is the desired hue angle:

```
U&#39; = (U-128) x Cos(H) + (V-128) x Sin(H)
V&#39; = (V-128) x Cos(H) - (U-128) x Sin(H)
```

Saturation is adjusted by multiplying U' and V' by a pair of constants, and then by adding 128 to each. The combined processing of hue and saturation on the UV data is shown in the following equations. H is the desired hue angle, C is the contrast value, and S is the saturation value:

```
U&#39;&#39; = (((U-128) x Cos(H) + (V-128) x Sin(H)) x C x S) + 128
V&#39;&#39; = (((V-128) x Cos(H) - (U-128) x Sin(H)) x C x S) + 128
```

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Processing%20in%20the%208-bit%20YUV%20Color%20Space%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



