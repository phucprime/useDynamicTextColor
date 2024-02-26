# useDynamicTextColor
Dynamic Text Color Adjustment with React Hooks

![image](./467f6395-d43f-4412-912a-084c12ab8458.jpeg)

How It Works

1. Calculating Luminance: The hook starts by calculating the luminance of the given background color. The luminance calculation uses a specific formula that weighs the red, green, and blue components of the color differently, reflecting their varying contributions to human perception of brightness.
2. Determining Text Color: Based on the calculated luminance, the hook decides the text color. If the luminance exceeds a certain threshold (in this case, 140), it suggests that the background color is light enough that black text would offer better readability. Conversely, if the luminance is below this threshold, white text is chosen to contrast with darker backgrounds.

```javascript
// Hook to calculate luminance and decide text color
const useDynamicTextColor = (bgColor) => {
    const getLuminance = (hex) => {
        let r, g, b;
        if (hex.length === 4) {
            r = parseInt(hex[1] + hex[1], 16);
            g = parseInt(hex[2] + hex[2], 16);
            b = parseInt(hex[3] + hex[3], 16);
        } else if (hex.length === 7) {
            r = parseInt(hex[1] + hex[2], 16);
            g = parseInt(hex[3] + hex[4], 16);
            b = parseInt(hex[5] + hex[6], 16);
        }
        // Formula to calculate luminance
        return 0.2126 * r + 0.7152 * g + 0.0722 * b;
    };

    const textColor = useMemo(() => {
        const luminance = getLuminance(bgColor);
        // Default luminance threshold for deciding text color
        const luminanceThreshold = 140;
        return luminance > luminanceThreshold ? 'black' : 'white';
    }, [bgColor]);

    return textColor;
};

export default useDynamicTextColor;
```

Usage Example

To use this hook in your component, simply pass the background color (in hex format) to the useDynamicTextColor hook, and it will return the appropriate text color. Here's a quick example:

```javascript
import React from 'react';
import useDynamicTextColor from './useDynamicTextColor';

const MyComponent = ({ bgColor }) => {
    const textColor = useDynamicTextColor(bgColor);

    return (
        <TextComponent style={{ backgroundColor: bgColor, color: textColor }}>
            This text color adjusts dynamically!
        </TextComponent>
    );
};
```
