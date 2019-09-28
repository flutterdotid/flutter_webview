# flutter_webview
Menampilkan Website di Android atau iOS dengan Flutter

<image src="http://flutter.id/wp-content/uploads/2019/09/webview23.gif">
  
  
<b># Add Package</b>

<code>
webview_flutter: ^0.3.14+1
</code>

<b># Add File ProgressHud.dart</b>

<code>
library modal_progress_hud;
import 'package:flutter/material.dart';
class ProgressHUD extends StatelessWidget {
  final Widget child;
  final bool inAsyncCall;
  final double opacity;
  final Color color;
  final Animation<Color> valueColor;
  ProgressHUD({
    Key key,
    @required this.child,
    @required this.inAsyncCall,
    this.opacity = 0.3,
    this.color = Colors.grey,
    this.valueColor,
  }) : super(key: key);
  @override
  Widget build(BuildContext context) {
    List<Widget> widgetList = new List<Widget>();
    widgetList.add(child);
    if (inAsyncCall) {
      final modal = new Stack(
        children: [
          new Opacity(
            opacity: opacity,
            child: ModalBarrier(dismissible: false, color: color),
          ),
          new Center(
            child: new CircularProgressIndicator(
              valueColor: valueColor,
            ),
          ),
        ],
      );
      widgetList.add(modal);
    }
    return Stack(
      children: widgetList,
    );
  }
}
  </code>
  
<b>#Change File main.dart</b>

<code>
import 'package:flutter/material.dart';
import 'package:webview_flutter/webview_flutter.dart';
import 'package:flutter_webview/progress_hud.dart';
void main() => runApp(MyApp());
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter.id',
      debugShowCheckedModeBanner: false,
      home: MyHomePage(title: 'Flutter.id'),
    );
  }
}
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
class _MyHomePageState extends State<MyHomePage> {
  bool _isLoading = true;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text(widget.title,style: TextStyle(color:Colors.white),),
        ),
        body: ProgressHUD(
          child: Padding(
            padding: EdgeInsets.all(10.0),
            child: Stack(
              children: <Widget>[
                WebView(
                  initialUrl:"https://flutter.id/",
                  javascriptMode: JavascriptMode.unrestricted,
                  onPageFinished: pageFinishedLoading,
                ),
              ],
            ),
          ),
          inAsyncCall: _isLoading,
          opacity: 0.0,
        ));
  }
  void pageFinishedLoading(String url) {
    setState(() {
      _isLoading = false;
    });
  }
}
  </code>
