# qtjambi-with-nativeFilter
a patched qtjambi lib with nativeFilter support

## Origin Library
https://github.com/OmixVisualization/qtjambi

## Different

add the QAbstractNativeEventFilter to use custom nativeEventFilter

add the installNativeEventFilter and removeNativeEventFilter functions to register your nativeEventFilter

## How to Use
use qtjambi and jna
```java
        import io.qt.core.*;
        import io.qt.widgets.*;

        import com.sun.jna.Pointer;
        import com.sun.jna.Structure;
        import com.sun.jna.platform.win32.WinUser.MSG;
        
        ......
        
        var app = QApplication.initialize(new String[] {});
        app.installNativeEventFilter(new WMEventFilter());
        
        ......
        
        class WMEventFilter extends QAbstractNativeEventFilter {
            public boolean nativeEventFilter(QByteArray arg0, QNativePointer arg1, QNativePointer arg2) {
                final int WM_DISPLAYCHANGE = 0x007e;
                if (arg0.equals("windows_generic_MSG") || arg0.equals("windows_dispatcher_MSG")) {
                    var msg = Structure.newInstance(MSG.class, new Pointer(arg1.pointer()));
                    msg.autoRead();
                    if (msg.message == WM_DISPLAYCHANGE) {
                        // System.out.println("nativeEventFilter, WM_DISPLAYCHANGE, 666");
                        return true;
                    }
                }
                return false;
            }
        }
```
