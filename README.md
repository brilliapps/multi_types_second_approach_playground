# multi_types_second_approach_playground
```dart
import 'package:flutter/material.dart';

// [Edit: What?] it cannot be used because how you imagine int object like 10 but with this operator | like 10 | 12
extension type M(Type t) implements Type {
  M operator |(Type other) {
    debugPrint('We are here1');
    return M(Type);
  }
}

extension on Type {
  M operator |(Object other) {
    debugPrint('We are here2');
    return M(this);
  }

  M operator [](Object other) {
    debugPrint('We are here3');
    return M(this);
  }

  set v(Object value) {}

  // error not recognized in any implementation attempy, but "|" operator is recognized
  M setV(Object value) => M(this);
}

//extension type Str<T extends Type>(String t) implements String {
extension type Str<T>(String t) implements String {
  M operator |(Object other) {
    debugPrint('We are here5');
    return M(Type);
  }
}

extension type Int<T>(int t) implements int {
  M operator |(Type other) {
    debugPrint('We are here6');
    return M(Type);
  }
}

/// BTW: Int instance could have a stream attached and change it's value iternally each time an event arrives.
/// TODO: SO I WOULD CALLED A VALUE TYPE OR VALUE TYPE SYNTAX (some other names, M, MultiType, MType LoadedType, PreciousType,
/// Fragmented Type, NestedType (because of nested generics with top type "natural") whatever), THAT IS TYPE WITH VALUE
/// BUT THE VALUE CAN BE OF ONE OF DECLARED TYPES BUT ALSO HAS THE FIRST TYPE "NATUAL" like Str<...... type is also String and
/// Not to do too long syntax for null as a function param you can use Str<Int<List>>? or f.e. typedef StringOrIntOrListOrNull = Str<Int<List>>?
/// Best Possibly StringOr<T>, intOr<T> (StrOr<T> Int...) with "but..." because FutureOr<T> means something different: It is Future<T> or <T> itself. however...
/// Because the limitation that the first type in the syntax Str<... is the natural type (also Int<.... , SomeType<...
/// you could use as a convention a sealed class for switch exhaustivenes checking like <Str<SomeSealedType>>
/// Never using a different convention
/// we want to achieve Str<Int<List>> that can be achieved using String | int | List syntax
/// We you've just seen it right String | ... has counterpart as Str<... not String<
/// For the time being String | int | List syntax is available only for declaring variables, the returned value is done under the hood and converted to Str<... syntax by the overriden | operator
/// also the below String | int returns an object of some Type not a Type itself. So the object is compatible with String | int | List Type is or this type
/// Below: Type object with possible value attached. see the ..v = 'Helo' part here
/// with the value or without it should go through example function definition sfddsgg(Str<Int<List>> abc) {}
///
///
/// Summary:
/// extension type new syntax sdk: '>=3.3.0-174.2.beta <4.0.0'
/// The full code
/// [https://github.com/brilliapps/multi_types_second_approach_playground/edit/main/README.md](https://github.com/brilliapps/multi_types_second_approach_playground/tree/main)
/// It at it's bottom contains some older experimentation syntaxes.
/// The real universal syntax might be Str<Int<List>> but an object may be created like Type wetyreyty3 = String | int | List
/// So a function could only by like sfddsgg(Str<Int<List>> abc) {}
/// All here is a stub, i try to deliver an idea, inspiration, it is early stage and probably will lead to nothing
/// the implementation may be much different this is just to indicate what the current syntax seems to be capable of
/// But people ask for a solution similar to union types, however i call it most often Multi Types.
/// the code below always is to generate Str<Int<List>> type that could be used as a type hind of a function param where
/// not tested but it is hoped that by access to "this" an Expando could be used to store values or additional data
/// like f.e. SomeGloballyAceessibleClass.typeexpando[this] = somedataobject;
Type wetyreyty3 =
    String | int | List; // this should return Str<Int<List>> type;
M wetyreyty4 = (String | int | List)
  ..v = 'Helo'; // with setting value - String/Str is natural type
// Type wetyreyty5 = String | int.setV('Helo') | List; //unexpected error see the definition
Type wetyreyty5 = String |
    int['Helo'] |
    List; // alternative for the above, int is to be a natural type with no need of casting to int So the outcome would be <Int<String<List>>>
M wetyreyty6 = (String |
    (int | int
      ..v = 'Helo') |
    List); // more awkward syntax
typedef StringOrIntOrListOrNull = Str<
    Int<List>>?; // it will naturally work as String, but also by comparison operator is compatible with all Str<Int<List>> objects. As you can see it's tricky

sfddsgg(Str<Int<List>> abc) {}

/// FIXME: To be solved:
/// Str<Int<List>> says that the base value is Str but int/List is accepted which is partially what you want
/// here is the point so you would have to do f.e. maybe something like this Int<Str<List>> would be accepted as Str<Int<List>> but the underlying value would be int.
/// It may be that when you use such Str< and never String you can more seamlessly accept the rest of the types instances - no need for casting just to override all operators, getters, methods properly
/// The order of creating a compatible type would accept a correct value or there could be added methods to ignore it.
/// Not needed. Also, maybe not needed but would be nice to investigate if <Str<int>> Would be accepted too.

void main() {}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // This is the theme of your application.
        //
        // TRY THIS: Try running your application with "flutter run". You'll see
        // the application has a purple toolbar. Then, without quitting the app,
        // try changing the seedColor in the colorScheme below to Colors.green
        // and then invoke "hot reload" (save your changes or press the "hot
        // reload" button in a Flutter-supported IDE, or press "r" if you used
        // the command line to start the app).
        //
        // Notice that the counter didn't reset back to zero; the application
        // state is not lost during the reload. To reset the state, use hot
        // restart instead.
        //
        // This works for code too, not just values: Most code changes can be
        // tested with just a hot reload.
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  // This widget is the home page of your application. It is stateful, meaning
  // that it has a State object (defined below) that contains fields that affect
  // how it looks.

  // This class is the configuration for the state. It holds the values (in this
  // case the title) provided by the parent (in this case the App widget) and
  // used by the build method of the State. Fields in a Widget subclass are
  // always marked "final".

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      // This call to setState tells the Flutter framework that something has
      // changed in this State, which causes it to rerun the build method below
      // so that the display can reflect the updated values. If we changed
      // _counter without calling setState(), then the build method would not be
      // called again, and so nothing would appear to happen.
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // This method is rerun every time setState is called, for instance as done
    // by the _incrementCounter method above.
    //
    // The Flutter framework has been optimized to make rerunning build methods
    // fast, so that you can just rebuild anything that needs updating rather
    // than having to individually change instances of widgets.
    return Scaffold(
      appBar: AppBar(
        // TRY THIS: Try changing the color here to a specific color (to
        // Colors.amber, perhaps?) and trigger a hot reload to see the AppBar
        // change color while the other colors stay the same.
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        // Here we take the value from the MyHomePage object that was created by
        // the App.build method, and use it to set our appbar title.
        title: Text(widget.title),
      ),
      body: Center(
        // Center is a layout widget. It takes a single child and positions it
        // in the middle of the parent.
        child: Column(
          // Column is also a layout widget. It takes a list of children and
          // arranges them vertically. By default, it sizes itself to fit its
          // children horizontally, and tries to be as tall as its parent.
          //
          // Column has various properties to control how it sizes itself and
          // how it positions its children. Here we use mainAxisAlignment to
          // center the children vertically; the main axis here is the vertical
          // axis because Columns are vertical (the cross axis would be
          // horizontal).
          //
          // TRY THIS: Invoke "debug painting" (choose the "Toggle Debug Paint"
          // action in the IDE, or press "p" in the console), to see the
          // wireframe for each widget.
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}

//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//
//import 'package:flutter/material.dart';
//
//extension type INT(int i) implements int {
//  INT operator +(int other) {
//    return Int(i + other);
//  }
//}
//
//extension type Str(String s) implements String {
//  abcsdfasfd() {
//    this.s = 10;
//  }
//}
//
//extension type U<T>(Type s) implements Type {
//  Type operator |(Type other) {
//
//// error if inside here
//    //extension type Stwr(String s) implements String {
//    //  abcsdfasfd() {
//    //    this.s = 10;
//    //  }
//    //}
////
//
//
//
//    return List;
//  }
//}
///*Type not working*/ var Int = U<int>(int); // operator or works
////typedef Uint2 = Uint;
//
//class sdfafasdfdsf {}
//
//typedef werwrwrwrwrwwrw = int as num;
//
//class UINT implements Type {
//  Type operator |(Type other) {
//    return List;
//  }
//  static Type operator |(Type other) {
//    return List;
//  }
//}
//
//
//extension Type2 on Type {
//  Type operator |(Type other) {
//    debugPrint('We are here');
//    return List;
//  }
//}
//
//
//void main() {
//  var wetyreyty = Uint | String;
//  var wetyreyty2 = UINT | String;
//  var wetyreyty3 = Type2 | String;
//
//  int a = 2;
//  Int b = Int(8); // coulndn't Int b = 8? and silently cast as Int?
//
//  b = b + a; // wrong, but Int + operator overriden returns Int
//  b = a + b; // wrong, because a is int, but couldn't be silently cast to Int
//  a = a + b; // good as expected.
//  print(b + a); // ok, somewhere toString is called for sure.
//  print(b + b);
//  print(a + b);
//}
//
//class MyApp extends StatelessWidget {
//  const MyApp({super.key});
//
//  // This widget is the root of your application.
//  @override
//  Widget build(BuildContext context) {
//    return MaterialApp(
//      title: 'Flutter Demo',
//      theme: ThemeData(
//        // This is the theme of your application.
//        //
//        // TRY THIS: Try running your application with "flutter run". You'll see
//        // the application has a purple toolbar. Then, without quitting the app,
//        // try changing the seedColor in the colorScheme below to Colors.green
//        // and then invoke "hot reload" (save your changes or press the "hot
//        // reload" button in a Flutter-supported IDE, or press "r" if you used
//        // the command line to start the app).
//        //
//        // Notice that the counter didn't reset back to zero; the application
//        // state is not lost during the reload. To reset the state, use hot
//        // restart instead.
//        //
//        // This works for code too, not just values: Most code changes can be
//        // tested with just a hot reload.
//        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
//        useMaterial3: true,
//      ),
//      home: const MyHomePage(title: 'Flutter Demo Home Page'),
//    );
//  }
//}
//
//class MyHomePage extends StatefulWidget {
//  const MyHomePage({super.key, required this.title});
//
//  // This widget is the home page of your application. It is stateful, meaning
//  // that it has a State object (defined below) that contains fields that affect
//  // how it looks.
//
//  // This class is the configuration for the state. It holds the values (in this
//  // case the title) provided by the parent (in this case the App widget) and
//  // used by the build method of the State. Fields in a Widget subclass are
//  // always marked "final".
//
//  final String title;
//
//  @override
//  State<MyHomePage> createState() => _MyHomePageState();
//}
//
//class _MyHomePageState extends State<MyHomePage> {
//  int _counter = 0;
//
//  void _incrementCounter() {
//    setState(() {
//      // This call to setState tells the Flutter framework that something has
//      // changed in this State, which causes it to rerun the build method below
//      // so that the display can reflect the updated values. If we changed
//      // _counter without calling setState(), then the build method would not be
//      // called again, and so nothing would appear to happen.
//      _counter++;
//    });
//  }
//
//  @override
//  Widget build(BuildContext context) {
//    // This method is rerun every time setState is called, for instance as done
//    // by the _incrementCounter method above.
//    //
//    // The Flutter framework has been optimized to make rerunning build methods
//    // fast, so that you can just rebuild anything that needs updating rather
//    // than having to individually change instances of widgets.
//    return Scaffold(
//      appBar: AppBar(
//        // TRY THIS: Try changing the color here to a specific color (to
//        // Colors.amber, perhaps?) and trigger a hot reload to see the AppBar
//        // change color while the other colors stay the same.
//        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
//        // Here we take the value from the MyHomePage object that was created by
//        // the App.build method, and use it to set our appbar title.
//        title: Text(widget.title),
//      ),
//      body: Center(
//        // Center is a layout widget. It takes a single child and positions it
//        // in the middle of the parent.
//        child: Column(
//          // Column is also a layout widget. It takes a list of children and
//          // arranges them vertically. By default, it sizes itself to fit its
//          // children horizontally, and tries to be as tall as its parent.
//          //
//          // Column has various properties to control how it sizes itself and
//          // how it positions its children. Here we use mainAxisAlignment to
//          // center the children vertically; the main axis here is the vertical
//          // axis because Columns are vertical (the cross axis would be
//          // horizontal).
//          //
//          // TRY THIS: Invoke "debug painting" (choose the "Toggle Debug Paint"
//          // action in the IDE, or press "p" in the console), to see the
//          // wireframe for each widget.
//          mainAxisAlignment: MainAxisAlignment.center,
//          children: <Widget>[
//            const Text(
//              'You have pushed the button this many times:',
//            ),
//            Text(
//              '$_counter',
//              style: Theme.of(context).textTheme.headlineMedium,
//            ),
//          ],
//        ),
//      ),
//      floatingActionButton: FloatingActionButton(
//        onPressed: _incrementCounter,
//        tooltip: 'Increment',
//        child: const Icon(Icons.add),
//      ), // This trailing comma makes auto-formatting nicer for build methods.
//    );
//  }
//}
//


```
