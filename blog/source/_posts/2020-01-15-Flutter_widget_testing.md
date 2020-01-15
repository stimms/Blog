---
layout: post
title: Flutter Widget Testing
authorId: simon_timms
date: 2020-01-15 9:00

---

I'm sure I don't have to tell you how important automated tests are to the software development process. Even if they didn't have other advantages I'd write tests just so I could have that sense of security when changing things in an application. Most every framework and language has some sort of testing ability built into it and Flutter is no different. However the documentation for widget testing in flutter is pretty poor. This post should get you over some of the gotchas I faced.

<!-- more -->

If you're new to Flutter then one of the key observations is that everything is a widget. Widgets are composed of other widgets and it's widgets all the way down. Frequently we'll have some complex user interaction in these widgets. This is something we want to test. 

To give us something to work with let me show this data entry screen (super rough right now).

![LAMP stack - Linux, Apache, MySQL and PHP/Perl/Python](/images/flutter-testing/app.png)

This is a pretty common sort of interaction where we want people to enter a 4 digit code and as they enter each digit the focus moves to the next one. Let's test this thing. 

## Getting Set Up

First thing we need is a test file. I'm keeping these in the test folder in the flutter project and in this case I named my file `codeEntryWidgetShould.dart`

```dart
import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter_test/flutter_test.dart';
import '../../lib/widgets/codeEntryWidget.dart';

void main() {

}
```

This sets up the basic structure of the test suite. Within this main method start with 

```dart
testWidgets("Should set code", (WidgetTester tester) async {

});
```

This [testWidgets](https://api.flutter.dev/flutter/flutter_test/testWidgets.html) holster will  hook into the test framework so that the test is run when running `flutter test`. Next to create the widget we call 

```dart
await tester.pumpWidget(CodeEntryWidget(onChange: (value) => code = value, code: code));
```

[PumpWidget](https://api.flutter.dev/flutter/flutter_test/WidgetTester/pumpWidget.html) will create the widget for you to test. Now if we were to run this with my particular widget there would be terrible errors. (Incedentally, you can run just this test class with `flutter test .\test\widgets\codeEntryWidgetShould.dart`)

```
══╡ EXCEPTION CAUGHT BY WIDGETS LIBRARY ╞═══════════════════════════════════════════════════════════
The following assertion was thrown building Text("Dealer Code"):
No Directionality widget found.
RichText widgets require a Directionality widget ancestor.
The specific widget that could not find a Directionality ancestor was:
  RichText
The ownership chain for the affected widget is: "RichText ← Text ← Align ← ConstrainedBox ←
  Container ← Row ← Column ← Padding ← DealerCodeEntryWidget ← [root]"
Typically, the Directionality widget is introduced by the MaterialApp or WidgetsApp widget at the
top of your application widget tree. It determines the ambient reading direction and is used, for
example, to determine how to lay out text, how to interpret "start" and "end" values, and to resolve
EdgeInsetsDirectional, AlignmentDirectional, and other *Directional objects.

The relevant error-causing widget was:
  Text
  file:///C:/code/inventive/assurant/pocketdrive-mobile-app/flutter_dev/lib/widgets/codeEntryWidget.dart:103:24
```

The problem is that this widget uses TextField which assumes some structure is set up for it. In particular here the problem is that flutter doesn't know which direction text should be going, left-to-right or right-to-left. We could wrap this in a `Directionality` widget but that's actually not going to be enough here. If we do we'll still have errors like 

```
══╡ EXCEPTION CAUGHT BY WIDGETS LIBRARY ╞═══════════════════════════════════════════════════════════
The following assertion was thrown building TextField-[<'field1'>](controller:
TextEditingController#b38cf(TextEditingValue(text: ┤0├, selection: TextSelection(baseOffset: 0,
extentOffset: 1, affinity: TextAffinity.downstream, isDirectional: false), composing:
TextRange(start: -1, end: -1))), focusNode: FocusNode#57ade, decoration: InputDecoration(counter:
Offstage(offstage: true), border: OutlineInputBorder()), maxLength: 1, textCapitalization:
characters, textAlign: center, scrollPadding: EdgeInsets.zero, dirty, state: _TextFieldState#e0808):
No Material widget found.
TextField widgets require a Material widget ancestor.
```

We need to be inside of a material app to get this widget to work properly. So if we change our widget creation to 

```dart
await tester.pumpWidget(MaterialApp(home: Scaffold(body: CodeEntryWidget(onChange: (value) => code = value, code: code))));
```

That will nicely create the widget for us. Notice that there is a lambda being passed into the onchange. This allows us to surface change events back to the test.

## First Test

Now we've got the widget what do we do with it? Well our first test should be if the value that comes back from the widget is actually the right one. To do this we'd like to click on each text box enter a value and be assured that the result is correct. 

First we need to find the boxes to enter text into. This is done using a finder. I gave each text field in my widget a `key` so it is easy to find. You don't have to do this but I'm thinking of the key as being like an `id` in HTML so it is a very powerful tool for aiding in getting the right control

```dart
var field1Finder = find.byKey(Key("field1"));
var field2Finder = find.byKey(Key("field2"));
var field3Finder = find.byKey(Key("field3"));
var field4Finder = find.byKey(Key("field4"));

//optionally test if we can find the widget
expect(field1Finder, findsOneWidget);
expect(field2Finder, findsOneWidget);
expect(field3Finder, findsOneWidget);
expect(field4Finder, findsOneWidget);
```

Next we'll need to enter text into each field.

```dart
await tester.enterText(field1Finder, "1");
await tester.enterText(field1Finder, "2");
await tester.enterText(field1Finder, "3");
await tester.enterText(field1Finder, "4");
```

and finally test if the code has been properly updated

```dart
expect(code, "1234");
```

Awesome, that wasn't too hard. 

## Testing Widget State

We now want to check to make sure that focus changes properly after entering data in each widget. The set up here is much the same

```dart
await tester.pumpWidget(MaterialApp(home: Scaffold(body: DealerCodeEntryWidget(onChange: (value) => code = value, code: code))));
var field1Finder = find.byKey(Key("field1"));
var field2Finder = find.byKey(Key("field2"));
var field3Finder = find.byKey(Key("field3"));
var field4Finder = find.byKey(Key("field4"));
```

It can be kind of tricky finding the focused element, fortunately we've set a [FocusNode](https://api.flutter.dev/flutter/widgets/FocusNode-class.html) on the text field. We just need to get it. To do this we have to call `evaluate()` on the finder and take the first result. Then we can cast that and check if the focusNode has focus

```dart
await tester.enterText(field1Finder, "1");
expect((field2Finder.evaluate().first.widget as TextField).focusNode.hasFocus, true);
await tester.enterText(field2Finder, "1");
expect((field3Finder.evaluate().first.widget as TextField).focusNode.hasFocus, true);
await tester.enterText(field3Finder, "1");
expect((field4Finder.evaluate().first.widget as TextField).focusNode.hasFocus, true);
```

And that's that. 

To my mind there is a lot to be desired here around the ease of widget testing. Finders are a little crude and the assertions available are crude. The setup requiring a `MaterialApp` and a `Scaffold` is awkward and likely to stop some people from testing. Ideally this process should be as easy as possible so no barriers exist for people. 