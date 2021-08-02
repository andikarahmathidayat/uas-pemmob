{
  "project_info": {
    "project_number": "815219239415",
    "project_id": "esgul-flutter-firebase",
    "storage_bucket": "esgul-flutter-firebase.appspot.com"
  },
  "client": [
    {
      "client_info": {
        "mobilesdk_app_id": "1:815219239415:android:4c576a227df91bfd10a6f2",
        "android_client_info": {
          "package_name": "com.example.hello_world"
        }
      },
      "oauth_client": [
        {
          "client_id": "815219239415-4kmm58vba5c3820oo0mpnbtavn1aq03l.apps.googleusercontent.com",
          "client_type": 3
        }
      ],
      "api_key": [
        {
          "current_key": "AIzaSyAhx0kdRUfM054gQo7dbMtXA7Yv5pNmSAU"
        }
      ],
      "services": {
        "appinvite_service": {
          "other_platform_oauth_client": [
            {
              "client_id": "815219239415-4kmm58vba5c3820oo0mpnbtavn1aq03l.apps.googleusercontent.com",
              "client_type": 3
            }
          ]
        }
      }
    }
  ],
  "configuration_version": "1"
} 
 android/build.gradle 
 lib/controller/auth_controller.dart 
@@ -0,0 +1,43 @@
import 'package:firebase_auth/firebase_auth.dart';

class AuthController {
  static FirebaseAuth _auth = FirebaseAuth.instance;

  static Future<User> signInAnonymous() async {
    try {
      UserCredential userCredential = await _auth.signInAnonymously();
      return userCredential.user;
    } catch (e) {
      print(e.toString());
      return null;
    }
  }

  static Future<User> signUp(String email, String password) async {
    try {
      UserCredential userCredential = await _auth
          .createUserWithEmailAndPassword(email: email, password: password);
      return userCredential.user;
    } catch (e) {
      print(e.toString());
      throw e;
    }
  }

  static Future<User> signIn(String email, String password) async {
    try {
      UserCredential userCredential = await _auth.signInWithEmailAndPassword(
          email: email, password: password);
      return userCredential.user;
    } catch (e) {
      print(e.toString());
      throw e;
    }
  }

  static Future<void> signOut() async {
    _auth.signOut();
  }

  static Stream<User> get userStream => _auth.authStateChanges();
}
 lib/main.dart 
@@ -1,168 +1,24 @@
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
// import 'package:hello_world/pages/stful_screen.dart';

// void main() => runApp(new MyApp());

// class MyApp extends StatelessWidget {
//   @override
//   Widget build(BuildContext context) {
//     return MaterialApp(
//       title: 'Tugas 3',
//       theme: ThemeData(
//         brightness: Brightness.light,
//       ),
//       darkTheme: ThemeData(
//         brightness: Brightness.dark,
//       ),
//       themeMode: ThemeMode.dark,
//       home: StfulScreen(),
//     );
//   }
// }

// import 'package:flutter/material.dart';
void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  _MyAppState createState() => _MyAppState();
import 'package:hello_world/controller/auth_controller.dart';
import 'package:hello_world/wrapper.dart';
import 'package:provider/provider.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

class _MyAppState extends State<MyApp> with SingleTickerProviderStateMixin {
  Animation<double> animation;
  AnimationController controller;

  @override
  void initState() {
    super.initState();
    controller =
        AnimationController(duration: const Duration(seconds: 10), vsync: this);
    animation = Tween<double>(begin: 0.0, end: 1.0).animate(controller);
    controller.forward();
  }

  // This widget is the root of your application.
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    controller.forward();
    return MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        home: MyHomePage(
          title: 'Product layout demo home page',
          animation: animation,
    return StreamProvider.value(
        initialData: null,
        value: AuthController.userStream,
        child: MaterialApp(
          debugShowCheckedModeBanner: false,
          home: Wrapper(),
        ));
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }
}

class MyHomePage extends StatelessWidget {
  MyHomePage({Key key, this.title, this.animation}) : super(key: key);
  final String title;
  final Animation<double> animation;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: Text("Product Listing")),
        body: ListView(
          shrinkWrap: true,
          padding: const EdgeInsets.fromLTRB(2.0, 10.0, 2.0, 10.0),
          children: <Widget>[
            FadeTransition(
                child: ProductBox(
                    name: "iPhone",
                    description: "ini adalah hp iphone",
                    price: 1000,
                    image:
                        "https://d2pa5gi5n2e1an.cloudfront.net/webp/global/images/product/mobilephones/Apple_iPhone_11_Pro_Max/Apple_iPhone_11_Pro_Max_L_1.jpg"),
                opacity: animation),
            MyAnimatedWidget(
                child: ProductBox(
                    name: "Pixel",
                    description: "ini adalah hp pixel",
                    price: 800,
                    image:
                        "https://www.mytrendyphone.eu/images/Google-Pixel-5-128GB-Black-PIXEL-5-128B-16102020-01-p.jpg"),
                animation: animation),
            ProductBox(
                name: "Samsung",
                description: "ini adalah hp samsung",
                price: 900,
                image:
                    "https://d2pa5gi5n2e1an.cloudfront.net/webp/global/images/product/mobilephones/Samsung_Galaxy_S21_Ultra_5G/Samsung_Galaxy_S21_Ultra_5G_L_1.jpg"),
            ProductBox(
                name: "Huawei",
                description: "Ini adalah hp huawei",
                price: 700,
                image:
                    "https://cf.shopee.co.id/file/5031b26fe3fa0378f46f2bbda7363cd7"),
            ProductBox(
                name: "Xiaomi",
                description: "Ini adalah hp xiaomi",
                price: 500,
                image:
                    "https://images.tokopedia.net/img/cache/500-square/VqbcmM/2021/3/31/f83d6810-1b2c-42b4-b6e0-38deab3e6012.jpg.webp?ect=4g"),
            ProductBox(
                name: "Vivo",
                description: "Ini adalah hp vivo",
                price: 600,
                image:
                    "https://rlifgk317qjw.cdn.shift8web.com/wp-content/uploads/2020/12/Vivo-X60-Pro-5G.png"),
          ],
        ));
  }
}

class ProductBox extends StatelessWidget {
  ProductBox({Key key, this.name, this.description, this.price, this.image})
      : super(key: key);
  final String name;
  final String description;
  final int price;
  final String image;
  Widget build(BuildContext context) {
    return Container(
        padding: EdgeInsets.all(2),
        height: 140,
        child: Card(
            child: Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: <Widget>[
              Image.network(image),
              Expanded(
                  child: Container(
                      padding: EdgeInsets.all(5),
                      child: Column(
                        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                        children: <Widget>[
                          Text(this.name,
                              style: TextStyle(fontWeight: FontWeight.bold)),
                          Text(this.description),
                          Text("Price: " + this.price.toString()),
                        ],
                      )))
            ])));
  }
}

class MyAnimatedWidget extends StatelessWidget {
  MyAnimatedWidget({this.child, this.animation});
  final Widget child;
  final Animation<double> animation;

  Widget build(BuildContext context) => Center(
        child: AnimatedBuilder(
            animation: animation,
            builder: (context, child) => Container(
                  child: Opacity(opacity: animation.value, child: child),
                ),
            child: child),
      );
}
 lib/pages/auth/login_screen.dart 
@@ -0,0 +1,89 @@
import 'package:flutter/material.dart';
import 'package:hello_world/controller/auth_controller.dart';
import 'package:hello_world/widgets/custom_button.dart';
import 'package:hello_world/widgets/custom_input.dart';

class LoginScreen extends StatelessWidget {
  final TextEditingController _emailCtrl = TextEditingController();
  final TextEditingController _pswdCtrl = TextEditingController();
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Login Screen"),
      ),
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              CustomInput(
                child: TextFormField(
                  decoration: InputDecoration(
                      labelText: 'Email', border: InputBorder.none),
                  controller: _emailCtrl,
                ),
              ),
              SizedBox(
                height: 10,
              ),
              CustomInput(
                  child: TextFormField(
                obscureText: true,
                decoration: InputDecoration(
                    labelText: 'Password', border: InputBorder.none),
                controller: _pswdCtrl,
              )),
              SizedBox(
                height: 10,
              ),
              CustomButton(
                tulisan: 'Sign In Anonymous',
                fungsi: () async {
                  await AuthController.signInAnonymous();
                },
              ),
              SizedBox(
                height: 10,
              ),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  CustomButton(
                    tulisan: 'Sign Up',
                    fungsi: () async {
                      try {
                        await AuthController.signUp(
                            _emailCtrl.text, _pswdCtrl.text);
                      } catch (e) {
                        ScaffoldMessenger.of(context).showSnackBar(
                            SnackBar(content: Text(e.toString())));
                      }
                    },
                  ),
                  SizedBox(
                    width: 10,
                  ),
                  CustomButton(
                    tulisan: 'Sign In',
                    fungsi: () async {
                      try {
                        await AuthController.signIn(
                            _emailCtrl.text, _pswdCtrl.text);
                      } catch (e) {
                        ScaffoldMessenger.of(context).showSnackBar(
                            SnackBar(content: Text(e.toString())));
                      }
                    },
                  ),
                ],
              ),
            ],
          ),
        ),
      ),
    );
  }
}
 lib/pages/home/home_screen.dart 
@@ -0,0 +1,35 @@
import 'package:firebase_auth/firebase_auth.dart';
import 'package:flutter/material.dart';
import 'package:hello_world/controller/auth_controller.dart';
import 'package:hello_world/widgets/custom_button.dart';

class HomeScreen extends StatelessWidget {
  final User user;
  HomeScreen(this.user);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Home Screen"),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            Text(user.uid),
            SizedBox(
              height: 10,
            ),
            CustomButton(
              tulisan: 'Logout',
              fungsi: () async {
                await AuthController.signOut();
              },
            )
          ],
        ),
      ),
    );
  }
}
 lib/widgets/custom_button.dart 
@@ -0,0 +1,27 @@
import 'package:flutter/material.dart';

class CustomButton extends StatelessWidget {
  final String tulisan;
  final Function fungsi;
  final Color warna;
  CustomButton({this.tulisan, this.fungsi, this.warna});
  @override
  Widget build(BuildContext context) {
    return Material(
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(15.0),
      ),
      // shadowColor: Colors.red,
      elevation: 1,
      color: warna ?? Colors.blue,
      // clipBehavior: Clip.antiAlias,
      child: Padding(
        padding: EdgeInsets.symmetric(vertical: 1, horizontal: 40),
        child: MaterialButton(
          child: Text(tulisan, style: TextStyle(color: Colors.white)),
          onPressed: fungsi,
        ),
      ),
    );
  }
}
 lib/widgets/custom_input.dart 
@@ -0,0 +1,27 @@
import 'package:flutter/material.dart';

class CustomInput extends StatelessWidget {
  final Widget child;
  CustomInput({this.child});
  @override
  Widget build(BuildContext context) {
    return Container(
        width: 300,
        // height: 100,
        decoration: BoxDecoration(
          color: Colors.white,
          borderRadius: BorderRadius.circular(15),
          boxShadow: [
            BoxShadow(
              color: Colors.grey.withOpacity(0.1),
              spreadRadius: 1,
              blurRadius: 1,
              offset: Offset(0, 2), // changes position of shadow
            ),
          ],
        ),
        child: Padding(
            padding: const EdgeInsets.symmetric(vertical: 1.0, horizontal: 20),
            child: child));
  }
}
lib/wrapper.dart 
@@ -0,0 +1,13 @@
import 'package:firebase_auth/firebase_auth.dart';
import 'package:flutter/material.dart';
import 'package:hello_world/pages/auth/login_screen.dart';
import 'package:hello_world/pages/home/home_screen.dart';
import 'package:provider/provider.dart';

class Wrapper extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    User user = Provider.of<User>(context);
    return (user != null) ? HomeScreen(user) : LoginScreen();
  }
}
  pubspec.lock 
# Generated by pub
# See https://dart.dev/tools/pub/glossary#lockfile
packages:
  async:
    dependency: transitive
    description:
      name: async
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.6.1"
  boolean_selector:
    dependency: transitive
    description:
      name: boolean_selector
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.1.0"
  characters:
    dependency: transitive
    description:
      name: characters
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.1.0"
  charcode:
    dependency: transitive
    description:
      name: charcode
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.2.0"
  clock:
    dependency: transitive
    description:
      name: clock
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.1.0"
  collection:
    dependency: transitive
    description:
      name: collection
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.15.0"
  cupertino_icons:
    dependency: "direct main"
    description:
      name: cupertino_icons
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.0.2"
  fake_async:
    dependency: transitive
    description:
      name: fake_async
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.2.0"
  firebase_auth:
    dependency: "direct main"
    description:
      name: firebase_auth
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.4.1"
  firebase_auth_platform_interface:
    dependency: transitive
    description:
      name: firebase_auth_platform_interface
      url: "https://pub.dartlang.org"
    source: hosted
    version: "4.3.1"
  firebase_auth_web:
    dependency: transitive
    description:
      name: firebase_auth_web
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.3.1"
  firebase_core:
    dependency: "direct main"
    description:
      name: firebase_core
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.3.0"
  firebase_core_platform_interface:
    dependency: transitive
    description:
      name: firebase_core_platform_interface
      url: "https://pub.dartlang.org"
    source: hosted
    version: "4.0.1"
  firebase_core_web:
    dependency: transitive
    description:
      name: firebase_core_web
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.1.0"
  flutter:
    dependency: "direct main"
    description: flutter
    source: sdk
    version: "0.0.0"
  flutter_test:
    dependency: "direct dev"
    description: flutter
    source: sdk
    version: "0.0.0"
  flutter_web_plugins:
    dependency: transitive
    description: flutter
    source: sdk
    version: "0.0.0"
  http_parser:
    dependency: transitive
    description:
      name: http_parser
      url: "https://pub.dartlang.org"
    source: hosted
    version: "4.0.0"
  intl:
    dependency: transitive
    description:
      name: intl
      url: "https://pub.dartlang.org"
    source: hosted
    version: "0.17.0"
  js:
    dependency: transitive
    description:
      name: js
      url: "https://pub.dartlang.org"
    source: hosted
    version: "0.6.3"
  matcher:
    dependency: transitive
    description:
      name: matcher
      url: "https://pub.dartlang.org"
    source: hosted
    version: "0.12.10"
  meta:
    dependency: transitive
    description:
      name: meta
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.3.0"
  nested:
    dependency: transitive
    description:
      name: nested
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.0.0"
  path:
    dependency: transitive
    description:
      name: path
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.8.0"
  plugin_platform_interface:
    dependency: transitive
    description:
      name: plugin_platform_interface
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.0.0"
  provider:
    dependency: "direct main"
    description:
      name: provider
      url: "https://pub.dartlang.org"
    source: hosted
    version: "5.0.0"
  sky_engine:
    dependency: transitive
    description: flutter
    source: sdk
    version: "0.0.99"
  source_span:
    dependency: transitive
    description:
      name: source_span
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.8.1"
  stack_trace:
    dependency: transitive
    description:
      name: stack_trace
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.10.0"
  stream_channel:
    dependency: transitive
    description:
      name: stream_channel
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.1.0"
  string_scanner:
    dependency: transitive
    description:
      name: string_scanner
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.1.0"
  term_glyph:
    dependency: transitive
    description:
      name: term_glyph
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.2.0"
  test_api:
    dependency: transitive
    description:
      name: test_api
      url: "https://pub.dartlang.org"
    source: hosted
    version: "0.3.0"
  typed_data:
    dependency: transitive
    description:
      name: typed_data
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.3.0"
  vector_math:
    dependency: transitive
    description:
      name: vector_math
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.1.0"
sdks:
  dart: ">=2.12.0 <3.0.0"
  flutter: ">=1.16.0"
  3  pubspec.yaml 
@@ -28,6 +28,9 @@ dependencies:
  # The following adds the Cupertino Icons font to your application.
  # Use with the CupertinoIcons class for iOS style icons.
  cupertino_icons: ^1.0.2
  firebase_core: ^1.3.0
  firebase_auth: ^1.4.1
  provider: ^5.0.0

dev_dependencies:
  flutter_test:
    sdk: flutter
# For information on the generic Dart part of this file, see the
# following page: https://dart.dev/tools/pub/pubspec
# The following section is specific to Flutter.
flutter:
  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the material Icons class.
  uses-material-design: true
  # To add assets to your application, add an assets section, like this:
  # assets:
    # - images/me.jpeg
    # - images/esgul.png
  # An image asset can refer to one or more resolution-specific "variants", see
  # https://flutter.dev/assets-and-images/#resolution-aware.
  # For details regarding adding assets from package dependencies, see
  # https://flutter.dev/assets-and-images/#from-packages
  # To add custom fonts to your application, add a fonts section here,
  # in this "flutter" section. Each entry in this list should have a
  # "family" key with the font family name, and a "fonts" key with a
  # list giving the asset and other descriptors for the font. For
  # example:
  # fonts:
  #   - family: Schyler
  #     fonts:
  #       - asset: fonts/Schyler-Regular.ttf
  #       - asset: fonts/Schyler-Italic.ttf
  #         style: italic
  #   - family: Trajan Pro
  #     fonts:
  #       - asset: fonts/TrajanPro.ttf
  #       - asset: fonts/TrajanPro_Bold.ttf
  #         weight: 700
  #
  # For details regarding fonts from package dependencies,
  # see https://flutter.dev/custom-fonts/#from-packages
