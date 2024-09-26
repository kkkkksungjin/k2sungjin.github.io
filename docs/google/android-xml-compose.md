---
layout: default
title: XML vs Compose
parent: Android
grand_parent: Google
nav_order: 5
---

Android XMLと Jetpack Composeは 両方Android UIを開発する方法だけど, 機能に置いて違いがあり. 

M __V__{: style="color:RED"} C <br/>
M __V__{: style="color:RED"} P <br/>
M __V__{: style="color:RED"} VM <br/>
M __V__{: style="color:RED"} I <br/>


# VIEW GROUP

XML: LinearLayout (Horizontal/Vertical) / ScrollView / HorizontalScrollView

~~~ java
<LinearLayout
    android:orientation="vertical"
    ... >
    <TextView ... />
    <Button ... />
</LinearLayout>
~~~

COMPOSE: __Colum,Row__{: style="color: #e26716"}
~~~ java
Column(Modifier.verticalScroll(rememberScrollState())) { //Vertical, ScrollView
    Text("Hello")
}
Row(Modifier.horizontalScroll(rememberScrollState())){ //Horizontal, HorizontalScrollView
    Text("Hello")
}
~~~
<br/>

XML: RelativeLayout 
~~~ java
<RelativeLayout>
    <TextView
        android:id="@+id/title"
        android:layout_alignParentTop="true"/>
    <TextView
        android:layout_below="@id/title" />
</RelativeLayout>
~~~

COMPOSE: __Box__{: style="color: #e26716"}
~~~ java
Box {
    Text("Title", Modifier.align(Alignment.TopCenter))
    Text("Subtitle", Modifier.align(Alignment.Center))
}
~~~
<br/>

XML: ConstraintLayout 
~~~ java
<ConstraintLayout>
    <TextView
        android:layout_constraintTop_toTopOf="parent"
        android:layout_constraintStart_toStartOf="parent" />
</ConstraintLayout>
~~~

COMPOSE: __ConstraintLayout__{: style="color: #e26716"}
~~~ java
ConstraintLayout {
    val (text1, text2) = createRefs()
    Text("Title", Modifier.constrainAs(text1) {
        top.linkTo(parent.top)
        start.linkTo(parent.start)
    })
    Text("Subtitle", Modifier.constrainAs(text2) {
        top.linkTo(text1.bottom)
        start.linkTo(parent.start)
    })
}
~~~
<br/>


XML: FrameLayout 
~~~ java
<FrameLayout>
    <ImageView android:src="@drawable/image" />
    <TextView android:text="Overlay Text" />
</FrameLayout>
~~~

COMPOSE: __Box__{: style="color: #e26716"}
~~~ java
Box {
    Image(painter = painterResource(id = R.drawable.image), contentDescription = null)
    Text("Overlay Text", Modifier.align(Alignment.Center))
}
~~~
<br/>
<hr/>

# POPUP VIEW

XML: Toast
~~~ java
Toast.makeText(context, "This is a Toast", Toast.LENGTH_SHORT).show()
~~~ 
COMPOSE:__Toast__{: style="color: #e26716"}
~~~ java
val context = LocalContext.current
Button(onClick = {
    Toast.makeText(context, "This is a Toast", Toast.LENGTH_SHORT).show()
}) {
    Text("Show Toast")
}
~~~ 
<br/>

XML: Snackbar
~~~ java
Snackbar.make(view, "This is a Snackbar", Snackbar.LENGTH_SHORT).show()
~~~ 
COMPOSE:__Snackbar__{: style="color: #e26716"}
~~~ java
val scaffoldState = rememberScaffoldState()
val coroutineScope = rememberCoroutineScope()
Scaffold(
    scaffoldState = scaffoldState
) {
    Button(onClick = {
        coroutineScope.launch {
            scaffoldState.snackbarHostState.showSnackbar("This is a Snackbar")
        }
    }) {
        Text("Show Snackbar")
    }
}
~~~ 
<br/>

XML: ProgressBar
~~~ java
<ProgressBar
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
~~~ 
COMPOSE:__ProgressBar__{: style="color: #e26716"}
~~~ java
CircularProgressIndicator()
-- or --
LinearProgressIndicator(progress = 0.5f) // 0.0f ~ 1.0f
~~~ 
<br/>

XML: Dialog
~~~ java
AlertDialog.Builder(context)
    .setTitle("Dialog Title")
    .setMessage("This is a dialog")
    .setPositiveButton("OK") { dialog, which ->
        ...
    }
    .setNegativeButton("Cancel", null)
    .show()
~~~ 
COMPOSE:__Dialog__{: style="color: #e26716"}
~~~ java
var showDialog by remember { mutableStateOf(false) }

if (showDialog) {
    AlertDialog(
        onDismissRequest = { showDialog = false },
        title = { Text(text = "Dialog Title") },
        text = { Text("This is a dialog message") },
        confirmButton = {
            Button(onClick = { showDialog = false }) {
                Text("OK")
            }
        },
        dismissButton = {
            Button(onClick = { showDialog = false }) {
                Text("Cancel")
            }
        }
    )
}

Button(onClick = { showDialog = true }) {
    Text("Show Dialog")
}
~~~ 
<br/>
<hr/>

# VIEW
XML margin -> COMPOSE X
~~~ java
// COMPOSE Similar functionality
Modifier.padding
~~~ 
XML Visibility -> COMPOSE X
~~~ java
// COMPOSE Similar functionality
Text(
    "Invisible Text",
    Modifier.alpha(0f) 
)
~~~ 

XML focusable, clickable, touchable -> COMPOSE Modifier.clickable(), Modifier.focusable()
~~~ java
// COMPOSE Similar functionality
Text(
    "Clickable Text",
    Modifier.clickable { /*  */ }
)
~~~ 

XML Elevation -> COMPOSE Modifier.shadow() or elevation
~~~ java
// COMPOSE Similar functionality
Card(
    elevation = 4.dp,
    modifier = Modifier.padding(16.dp)
) {
    Text("Card with elevation")
}
~~~ 

XML Weight -> COMPOSE Modifier.weight()
~~~ java
// COMPOSE Similar functionality
Row(Modifier.fillMaxWidth()) {
    Text("Item 1", Modifier.weight(1f))
    Text("Item 2", Modifier.weight(1f))
}
~~~ 
<br/>
<hr/>


# Image Load

XML: Glide
~~~ java
Glide.with(context)
    .load("https://example.com/image.jpg")
    .into(imageView)
~~~ 

COMPOSE: __Coil__{: style="color: #e26716"}(Coroutines image load)
~~~ java
Image(
    painter = rememberImagePainter("https://xxxx.com/image.jpg"),
    contentDescription = "Loaded image"
)
~~~ 
- Kotlin-firstライブラリで、Composeと自然に統合されています。
- Kotlinコルーチンを使用するため、非同期の画像読み込みが直感的でパフォーマンスに優れています。
- より軽量で、高速な初期化速度を提供し、Composeベースのプロジェクトでパフォーマンスの利点があります。
- Compose環境に合わせて、より簡単に使用できるAPIを提供しています。
<br/>

