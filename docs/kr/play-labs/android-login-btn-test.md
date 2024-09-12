---
layout: default
title: Btn test
parent: android
grand_parent: play-labs
nav_order: 7
---

소개
---
~~~ java
//Add lib
naviagtion = "2.7.5"
material-icons-extended = "1.5.4"

[libraries]
androidx-naviagtion = { group = "androidx.navigation", name = "navigation-compose", version.ref="naviagtion" }
androidx-material-icons-extended = { group = "androidx.compose.material", name = "material-icons-extended", version.ref="material-icons-extended" }
~~~


로그인화면
<hr/>
<img src="../../../../assets/images/compose/login_screen.jpg" width="350" height="550"/>
<br/>

<!-- ~~~ java
@Composable
fun LoginScreen(navController: NavController) {

    var _email by remember { mutableStateOf("") }
    var _password by remember { mutableStateOf("") }
    var isPasswordVisible by remember { mutableStateOf(false) }

    val textColor = Color.Black
    Surface(
        modifier = Modifier
            .fillMaxSize()
            .background(Color.White)
            .padding(padding32),
        color = Color.White
    ) {

        Column(modifier = Modifier.fillMaxSize()) {
            Column(
                horizontalAlignment = Alignment.CenterHorizontally,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(top = padding32)

            ) {
                Text(text = "LOGIN", fontWeight = FontWeight.Bold, fontSize = 32.sp)
            }
            Spacer(modifier = Modifier.padding(padding16))
            Column(
                horizontalAlignment = Alignment.CenterHorizontally,
                modifier = Modifier.fillMaxSize(),
                ) {
                Column {
                    OutlinedTextField(
                        leadingIcon = {
                            Icon(
                                imageVector = Icons.Default.Email,
                                contentDescription = "email",
                                tint = textColor
                            )
                        },
                        maxLines = 1,
                        value = _email,
                        onValueChange = { _email = it },
                        label = { Text(text = "email") },
                        modifier = Modifier
                            .background(Color.White)
                            .border(color = Color(0x005B96), width = 3.dp),

                        )
                    Spacer(modifier = Modifier.padding(padding8))
                    OutlinedTextField(
                        leadingIcon = {
                            Icon(
                                imageVector = Icons.Default.Lock,
                                contentDescription = "password",
                                tint = textColor
                            )
                        },
                        trailingIcon = {
                            IconButton(
                                onClick = {
                                    isPasswordVisible = !isPasswordVisible
                                },
                            ) {
                                val visibleIconAndText =
                                    Pair(first = Icons.Filled.Visibility, second = "VisibilityOn")
                                val hiddenIconAndText = Pair(
                                    first = Icons.Filled.VisibilityOff, second = "VisibilityOff"
                                )

                                val passwordVisibilityIconAndText =
                                    if (isPasswordVisible) visibleIconAndText else hiddenIconAndText

                                // Render Icon
                                Icon(
                                    imageVector = passwordVisibilityIconAndText.first,
                                    contentDescription = passwordVisibilityIconAndText.second,
                                    tint = Color.White
                                )
                            }
                        },
                        maxLines = 1,
                        value = _password,
                        onValueChange = { _password = it },
                        label = { Text(text = "password") },
                        modifier = Modifier
                            .border(color = Color(0x005B96), width = 3.dp),
                    )
                    Spacer(modifier = Modifier.padding(2.dp))
                    Row(
                        verticalAlignment = Alignment.CenterVertically
                    ){
                        Text(text = "Don't have an account yet?")
                        TextButton(onClick = { navController.navigate(AppScreen.SIGNUP.name)}) {
                            Text(text = "Signup", color = Color.Blue)
                        }
                    }

                }
                Column(
                    horizontalAlignment = Alignment.CenterHorizontally,
                    modifier = Modifier.fillMaxWidth()
                ) {
                    Button(
                        onClick = { onClickLogin() },
                        modifier = Modifier
                            .fillMaxWidth(0.7f)
                            .height(50.dp)
                    ) {
                        Text(
                            text = "Login",
                            style = TextStyle(Color.White),
                            fontWeight = FontWeight.Bold
                        )
                    }
                    Row(
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(padding8), verticalAlignment = Alignment.CenterVertically
                    ) {
                        Divider(
                            modifier = Modifier
                                .fillMaxWidth()
                                .weight(1f),
                            thickness = 1.dp,
                            color = GrayColor
                        )
                        Text(
                            text = "Or",
                            modifier = Modifier.padding(10.dp),
                            fontSize = 20.sp
                        )
                        Divider(
                            modifier = Modifier
                                .fillMaxWidth()
                                .weight(1f),
                            thickness = 1.dp,
                            color = GrayColor
                        )
                    }
                    Column {
                        Button(
                            onClick = { /*TODO*/ },
                            colors = ButtonDefaults.buttonColors(Color.Transparent),
                            modifier = Modifier.fillMaxWidth()
                        ) {
                            Image(
                                painter = painterResource(id = R.drawable.fb_svg),
                                contentDescription = "fb Logo",
                            )
                        }
                        Spacer(modifier = Modifier.width(10.dp))
                        Button(
                            onClick = { /*TODO*/ },
                            colors = ButtonDefaults.buttonColors(Color.Transparent),
                            modifier = Modifier.fillMaxWidth(),
                        ) {
                            Image(
                                painter = painterResource(id = R.drawable.google_svg),
                                contentDescription = "Google Logo",
                            )
                        }
                        Spacer(modifier = Modifier.width(10.dp))
                        Button(
                            onClick = { /*TODO*/ },
                            colors = ButtonDefaults.buttonColors(Color.Transparent),
                            modifier = Modifier.fillMaxWidth()
                        ) {
                            Image(
                                painter = painterResource(id = R.drawable.apple_svg),
                                contentDescription = "apple Logo",
                            )
                        }
                        Spacer(modifier = Modifier.width(10.dp))
                    }
                }
            }
        }
    }
}
~~~ -->
<br/>
