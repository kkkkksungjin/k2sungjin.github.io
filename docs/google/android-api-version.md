---
layout: default
title: Android release notes
parent: Android
grand_parent: Google
nav_order: 1
---
# Android バージョン変更内容

<hr/>

## __14(Upside Down Cake) -> 15(Not name yet)__{: style="color: #00c0ef"}
[Android Developers Blog](https://android-developers.googleblog.com/2024/03/the-second-developer-preview-of-android-15.html)

__MediaProcessingForegroundService導入__{: style="color: #e26716"}

主な目的
- ビデオ/オーディオ変換など、時間のかかるメディア関連タスクの処理。
- 作業の進行状況をユーザーに通知しながら安定的に実行。
- 6時間制限があり、長時間の作業も安定して実行可能。

このサービスは、オーディオやビデオなどのメディアファイルの変換や圧縮を行う際に非常に便利です。

__SQLite API 改善__{: style="color: #e26716"}

Android 15では、SQLite APIが改善され、読み取り専用トランザクションなどのパフォーマンス最適化をサポートする新しいメソッドが追加されました。 読み取り専用トランザクションを使用する場合、パフォーマンスをより効率的に管理することができます。
~~~java
db.beginTransactionReadOnly()
try {
    // データ検索＆処理
    db.setTransactionSuccessful()
} finally {
    db.endTransaction()
}
~~~

__Virtualizerの代わりにSpatializerを使用__{: style="color: #e26716"}　<br/>

~~~java
val spatializer = audioManager.spatializer
if (spatializer.isAvailable) {
    spatializer.setEnabled(true)
}
~~~

__画面録画検出機能の追加__{: style="color: #e26716"} <br/>
Android 15では、画面録画を検出する機能が追加されました。機密情報を扱うアプリでは、画面が録画されているかどうかを検出し、必要に応じて保護機能を追加することができます。

__部分的な画面録画のサポート__{: style="color: #e26716"} <br/>
Android 15では、部分的な画面録画機能が追加されました。特定のアプリのみを録画できるように設定する機能をサポートする場合、この機能に対応するコードを追加する必要があります。

<hr/>

## __13(Tiramisu) -> 14(Upside Down Cake)__{: style="color: #00c0ef"}

__サイドロードアプリの権限制限__{: style="color: #e26716"}: Playストア外部からインストールされたアプリの権限制限。
~~~java
<!-- 例えば, 以下のような高リスク権限が制限されます -->
<uses-permission android:name="android.permission.READ_SMS" />
<uses-permission android:name="android.permission.CALL_LOG" />
~~~

__動的権限許可ポリシーの変更__{: style="color: #e26716"}: 「今回のみ許可」オプションによる権限管理が必要。
~~~java
ActivityCompat.requestPermissions(
    this,
    arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
    REQUEST_CODE
)
~~~

__写真ピッカー機能の拡大__{: style="color: #e26716"}: 写真の権限がなくても安全に写真を選択可能。<br/>

Android 13では写真セレクタが導入されましたが、Android 14ではこの機能がさらに拡充され、ユーザーはアプリがギャラリーに直接アクセスせずに安全に写真を選択できるようになりました。これにより、写真の権限を完全にリクエストする必要がなく、写真を選択できるようになり、アプリが不要な権限をリクエストしないようにコードを修正する必要がある場合があります。

~~~java
val photoPickerLauncher = registerForActivityResult(
    ActivityResultContracts.PickVisualMedia()) { uri ->
    if (uri != null) {
        imageView.setImageURI(uri)
    }
}
val pickVisualMediaRequest = PickVisualMediaRequest(
    ActivityResultContracts.PickVisualMedia.ImageOnly)
photoPickerLauncher.launch(pickVisualMediaRequest)
~~~

__バッテリー最適化強化__{: style="color: #e26716"}: Backgroud作業とバッテリー消費の最適化が必要。

__暗号化強化__{: style="color: #e26716"}: アプリのデータを暗号化して安全に保存する必要があります。<br/>

Android 14では、アプリのデータ暗号化に対する要件が強化されました。機密データを暗号化し、安全に保存することが必須とされています。アプリで機密データを保存する際には、強力な暗号化技術を使用し、暗号化に関するライブラリやAPIを適切に活用する必要があります。
~~~java
val keyGenerator = KeyGenerator.getInstance(KeyProperties.KEY_ALGORITHM_AES, "AndroidKeyStore")
keyGenerator.init(
    KeyGenParameterSpec.Builder(
        "MyKeyAlias",
        KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT
    )
    .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
    .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
    .build()
)
val secretKey = keyGenerator.generateKey()
~~~

__UIとアクセシビリティの改善__{: style="color: #e26716"}: Font scalingが200%サポートとアクセシビリティ機能強化。
~~~java
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="18sp"
    android:autoSizeTextType="uniform"
    android:autoSizeMinTextSize="12sp"
    android:autoSizeMaxTextSize="112sp" />
~~~

__ネットワーク制約__{: style="color: #e26716"}: HTTPの代わりにHTTPS通信を使用する必要があります。
~~~java
<!-- network_security_config.xml -->
<network-security-config>
    <domain-config cleartextTrafficPermitted="false">
        <domain includeSubdomains="true">example.com</domain>
    </domain-config>
</network-security-config>
~~~

<hr/>

## __12(Snow Cone) -> 13(Tiramisu)__{: style="color: #00c0ef"}

__Notification 権限追加(POST_NOTIFICATIONS)__{: style="color: #e26716"}

通知を送る為はこの権限を追加する必要がある。 <br/>

~~~java
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />

OR

if (ContextCompat.checkSelfPermission(this, Manifest.permission.POST_NOTIFICATIONS)
    != PackageManager.PERMISSION_GRANTED) {
    ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.POST_NOTIFICATIONS), REQUEST_CODE)
}
~~~

__Media File 接続権限変更__{: style="color: #e26716"}<br/>

~~~java
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />
<uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />
~~~

既存の**READ_EXTERNAL_STORAGE**はもはやすべてのメディアファイルにアクセスできないので、各ファイルタイプごとに権限を個別に要求する必要があります。

__プッシュ通知におけるPendingIntentのセキュリティの強化__{: style="color: #e26716"}<br/>
Android 12で導入されたPendingIntentのFLAG_IMMUTABLEまたはFLAG_MUTABLEフラグの使用要件はAndroid 13でも継続され、特にプッシュ通知のセキュリティが強化されました。 すべてのPendingIntentは**不変(immutable)**に設定することが推奨されます。
~~~java
val pendingIntent = PendingIntent.getActivity(
    this, 
    0, 
    intent, 
    PendingIntent.FLAG_IMMUTABLE
)
~~~

__Bluetooth 権限強化__{: style="color: #e26716"}

BluetoothはScan時に新しい権限**NEARBY_WIFI_DEVICES**が追加されました。 この権限は、Wi-Fiデバイスの検索とScanに必要です。
~~~java
<uses-permission android:name="android.permission.NEARBY_WIFI_DEVICES" />
~~~

__Themed App Iconsサポート__{: style="color: #e26716"}
~~~java
<!-- AndroidManifest.xml -->
<adaptive-icon android:icon="@drawable/ic_launcher">
    <monochrome android:icon="@drawable/ic_launcher_monochrome"/>
</adaptive-icon>
OR
<adaptive-icon xmlns:android="http://schemas.android.com/apk/res/android">
    <background android:drawable="@drawable/ic_launcher_background" />
    <foreground android:drawable="@mipmap/ic_launcher_foreground" />
    <monochrome android:icon="@drawable/ic_launcher_monochrome"/>
</adaptive-icon>
~~~

__アプリ毎の言語設定をサポート__{: style="color: #e26716"}： アプリで言語設定機能をサポートするための追加コードが必要。<br/>
__バッテリー最適化とバックグラウンドタスクの制限__{: style="color: #e26716"}: WorkManagerとJobSchedulerの使用を推奨。


<hr/>

## __11(Red Velvet Cake) -> 12(Snow Cone)__{: style="color: #00c0ef"}

<br/>
__新しい権限 (Bluetooth)__{: style="color: #e26716"} <br/>
Android 12からBluetooth機能を使用するために新しい権限が必要です。BLUETOOTH_CONNECT、BLUETOOTH_SCAN、そしてBLUETOOTH_ADVERTISE権限を追加する必要があります。

~~~java
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
<uses-permission android:name="android.permission.BLUETOOTH_SCAN" />
<uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE" />
~~~
<br/>
__正確な位置情報権限__{: style="color: #e26716"} <br/>
Android 12では、位置情報を要求する際、ACCESS_FINE_LOCATIONと一緒に**正確でない位置(ACCESS_COARSE_LOCATION)**を提供することができます。もしアプリが正確な位置情報を要求しない場合は、ACCESS_COARSE_LOCATIONのみを要求する必要があります。
~~~java
<!-- AndroidManifest.xml에서 -->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
~~~

__ForegroundServiceの通知要求__{: style="color: #e26716"}　<br/>
Android 12から、ForegroundServiceはユーザーに遅延通知を送信する前に、最大10秒の遅延を置くことができるようになりました。アプリがフォアグラウンドサービスを開始したときに通知をすぐに表示したくない場合は、Service.startForeground()を呼び出す前にpostDelayed()で通知遅延を追加することができます。

~~~java
val handler = Handler(Looper.getMainLooper())
handler.postDelayed({
    startForeground(NOTIFICATION_ID, notification)
}, 10000) 
~~~

__Exportedプロパティ__{: style="color: #e26716"}　

Android 12では、Activity、Service、BroadcastReceiverを明示的にexported属性を指定する必要があります。もし、アプリに外部から呼び出し可能なコンポーネントがある場合、AndroidManifest.xmlにexported=「true」またはexported=「false」を指定する必要があります。 この設定がないと、アプリが実行されない場合があります。

~~~java
<activity
    android:name=".MainActivity"
    android:exported="true" />  <!-- アプリ外部から呼び出し可能 -->
    
<service
    android:name=".MyService"
    android:exported="false" />  <!-- アプリ内部のみ呼び出し可能 -->

~~~

__Material Youテーマの適用__{: style="color: #e26716"}　：システムテーマの色に合わせてアプリテーマを設定。<br/>
__PendingIntent__{: style="color: #e26716"}　: **FLAG_MUTABLE**または**FLAG_IMMUTABLE**フラグの追加が必要。<br/>
__BackgroundServiceの制限強化__{: style="color: #e26716"}　: WorkManagerへの移行を検討。<br/>
__Toastスタイル__{: style="color: #e26716"}　: 基本スタイルを維持することを推奨。<br/>

