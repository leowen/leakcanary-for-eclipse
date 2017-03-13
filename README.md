# leakcanary-for-eclipse
leakcanary lib src code for eclipse

Add this project to your eclipse project as a lib as follow:
1. Config in AndroidManifest.xml
        <service
            android:name="com.squareup.leakcanary.internal.HeapAnalyzerService"
            android:enabled="false"
            android:process=":leakcanary" />
        <service
            android:name="com.squareup.leakcanary.DisplayLeakService"
            android:enabled="false" />


        <activity
            android:name="com.squareup.leakcanary.internal.DisplayLeakActivity"
            android:enabled="false"
            android:taskAffinity="com.squareup.leakcanary" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />


                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

2. Add the follow codes to your application, you may ignore "enabledStrictMode".
@Override
	public void onCreate() {
		super.onCreate();
		if (LeakCanary.isInAnalyzerProcess(this)) {
			// This process is dedicated to LeakCanary for heap analysis.
			// You should not init your app in this process.
			return;
		}
		enabledStrictMode();
		LeakCanary.install(this);
	}

	private static void enabledStrictMode() {
		StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder() //
				.detectAll() //
				.penaltyLog() //
				.penaltyDeath() //
				.build());
	}
