---
title: C28111
description: Warning C28111 The IRQL where the floating-point state was saved does not match the current IRQL (for this restore operation).
ms.assetid: 3573ebf0-5f5b-4b04-835a-7dba36e95e8c
keywords: ["warnings listed WDK PREfast for Drivers", "errors listed WDK PREfast for Drivers"]
---

# C28111


warning C28111: The IRQL where the floating-point state was saved does not match the current IRQL (for this restore operation).

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Additional information</strong></p></td>
<td align="left"><p>The floating Save/Restore functions require that the IRQL be the same at the time of save and the corresponding restore.</p></td>
</tr>
</tbody>
</table>

 

The IRQL at which the driver is executing when it restores a floating-point state is different than the IRQL at which it was executing when it saved the floating-point state.

Because the IRQL at which the driver runs determines how the floating-point state is saved, the driver must be executing at the same IRQL when it calls the functions to save and to restore the floating-point state.

### <span id="example"></span><span id="EXAMPLE"></span>Example

The following code example elicits this warning.

```
void driver_utility()
{
    // running at APC level
    KFLOATING_SAVE FloatBuf;
    if (KeSaveFloatingPointState(&FloatBuf))
    {
        KeLowerIrql(PASSIVE_LEVEL);
        ...
        KeRestoreFloatingPointState(&FloatBuf);
    }
}
```

The following code example avoids this warning.

```
void driver_utility()
{
    // running at APC level
    KFLOATING_SAVE FloatBuf;
    if (KeSaveFloatingPointState(&FloatBuf))
    {
        KeLowerIrql(PASSIVE_LEVEL);
        ...
        KeRaiseIrql(APC_LEVEL, &old);
        KeRestoreFloatingPointState(&FloatBuf);
    }
}
```

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[devtest\devtest]:%20C28111%20%20RELEASE:%20%2811/17/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




