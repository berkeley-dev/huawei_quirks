# Display Engine features

## XNIT
```
/**
 * @return true if XNIT is supported, false otherwise
 */
bool isXnitSupported() {
    return sDisplayEngineService.getSupported(DisplayEngineService.DE_FEATURE_XNIT) == 1;
}

/**
 * Sets XCC coefficient.
 *
 * @param red   float value in range 0.0f-1.0f
 * @param green float value in range 0.0f-1.0f
 * @param blue  float value in range 0.0f-1.0f
 * @return      0 when successful, -1 otherwise
 */
int setXnitData(float red, float green, float blue) {
    int[] xccCoef = new int[] {
        (int)(red * 32768.0f),
        (int)(green * 32768.0f),
        (int)(blue * 32768.0f)
    };

    PersistableBundle bundle = new PersistableBundle();
    bundle.putIntArray("Buffer", xccCoef);
    bundle.putInt("BufferLength", 12);

    return sDisplayEngineService.setData(DisplayEngineService.DE_DATA_TYPE_XNIT, bundle);
}
```
