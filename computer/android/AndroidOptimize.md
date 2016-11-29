> Android性能优化笔记

# 1、使用static内部类

在使用内部类的时候一定要使用静态内部类。避免保存外部类的引用导致内存泄漏。

##　1.1、使用弱引用的Handler

```java
package com.pix;
import java.lang.ref.WeakReference;
import android.os.Handler;
import android.os.Looper;
import android.os.Message;
public abstract class WeakReferenceHandler<T> extends Handler {
	private WeakReference<T> mReference;
	public WeakReferenceHandler(T reference) {
        super();
		mReference = new WeakReference<T>(reference);
	}
    public WeakReferenceHandler(T reference,Looper looper) {
        super(looper);
        mReference = new WeakReference<T>(reference);
    }
	@Override
	public void handleMessage(Message msg) {
		if (mReference.get() == null)
			return;
		handleMessage(mReference.get(), msg);
	}
	protected abstract void handleMessage(T reference, Message msg);
}
```

