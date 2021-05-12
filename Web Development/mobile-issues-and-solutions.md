# Mobile Issues and Solutions

## Auto Zoom into Text Fields on Focus
### Issue
- Browser automatically zooms in if the font-size is less than 16px (default is usually 11px).
### Solution
1. Viewport Meta `maximum-scale=1`
  - `<meta name="viewport" content="width=device-width, initial-scale=1.0", maximum-scale=1/>`
  - Caveat
    - Disables user zoom on Android devices.
    - So, add `maximum-scale=1` only if device is iOS.
    ```
    const addMaximumScaleToMetaViewport = () => {
      const el = document.querySelector('meta[name=viewport]');

      if (el !== null) {
        let content = el.getAttribute('content');
        let re = /maximum\-scale=[0-9\.]+/g;

        if (re.test(content)) {
          content = content.replace(re, 'maximum-scale=1.0');
        } else {
            content = [content, 'maximum-scale=1.0'].join(', ')
        }
        
        el.setAttribute('content', content);
        }
      };

    const disableIosTextFieldZoom = addMaximumScaleToMetaViewport;

    // https://stackoverflow.com/questions/9038625/detect-if-device-is-ios/9039885#9039885
    const checkIsIOS = () => /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;

    if (checkIsIOS()) {
      disableIosTextFieldZoom();
    }
    ```
2. CSS
    ```
    #myTextArea:active {
      font-size: 16px;
    };
    ```
3. Temporarily switch font size to 16px when typing.

### Reference
[Disable Auto Zoom in Input "Text" tag | Stack Overflow](https://stackoverflow.com/questions/2989263/disable-auto-zoom-in-input-text-tag-safari-on-iphone#:~:text=36%20Answers&text=Additionally%2C%20the%20select%20element%20needs,the%20focus%20pseudo%2Dclass%20attached.&text=You%20can%20prevent%20Safari%20from,attribute%20suggested%20in%20other%20answers.)
[Stop iPhones From Zooming in on Input Form Fields](https://www.warrenchandler.com/2019/04/02/stop-iphones-from-zooming-in-on-form-fields/)

## Dismiss the Keyboard when Return Key is Pressed

