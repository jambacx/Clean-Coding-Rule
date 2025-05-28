## Follow coding principles

Most common and important principles:

- KISS (Keep it simple and stupid)
- DRY (Don't repeat yourself)
- YAGNI (You ain't gonna need it)
- SOLID

## Enhancing List Creation
Dart’s collection if and spread operators (... and ...?) allow for more flexible and readable list creation.

```dart
var isLoggedIn = true;
var items = [
  'Home',
  if (isLoggedIn) 'Profile',
  'Settings',
];

var extraItems = ['Help', 'Logout'];
var allItems = [
  'Home',
  ...extraItems,
  'Settings',
];
```

## Comments

```dart
“Code never lies, comments sometimes do.” — Ron Jeffries
```

- Use comments when the code is not self-explanatory.
- Prefer commenting why something is done, rather than how.
- For doc comments use ///.
- Prefer writing doc comments for public APIs.
- Use square brackets in doc comments to refer to in-scope identifiers.

```dart
 /// Throws a [StateError] if ...
  /// similar to [anotherMethod()], but ...
```

- Don't forget to update comments when you change the commented code!

## Variable declaration

### Public variable

```dart
Good: ✅
var cm = 10
```

### Private variable

```dart
Good: ✅
var _cm = 10
```

## Function declaration

### Public function

```dart
Good: ✅
void isValid()
{
}
```

### Private function

```dart
Good: ✅
void _isValid()
{
}
```

## Naming
* What is “s” in the third person singular?
* What are the 5 sentence patterns (SV/SVC/SVO/SVOO/SVOC)?
* What are the parts of speech (noun/adjective/verb)?
* What does ~ing or ~ed mean to be an adjective?

### (A) verb + at/on
When indicating the date and time, the following applies
```dart
/// at indicates date and time
updatedAt // date and time of update
importedAt // date imported

/// if on, indicates date
deletedOn // deleted date
```

### (B) adjective + noun
This is almost always the rule when assigning non-boolean values.
```dart
/// adjective + noun
specialCategory // special category

/// Adjective can be passive (~ed) or ing form of verb
importedPlayerNames // multiple imported visitor names
deletedPlayers // deleted multiple visitors
payingPlayer // payer

/// Nouns only or “noun + noun” is OK if it can be clearly indicated without adjectives
errorCode // error code
centerImageFilePath // file path of the center image
terminalIdsArray // array containing multiple terminal IDs

/// noun+without/before/after~ is also OK
userWithoutPermission // user without permission
itemTypeBeforeUpdate // item type before update
quantityAfterOrder // quantity after order
```

### (C) show + noun
The following is the case for the flag to show/hide
```dart
showConfirmationModal
// true: show confirmation modal
// false: hide confirmation modal
```

### (D) noun + enabled
The following is the case of a flag that turns on or off some function.
```dart
autoScrollEnabled
// true: auto scroll ON
// false: auto scroll OFF
```
DISABLED is also possible, but it is better to avoid it because it is difficult to understand because it is a double negative.
The noun part can also be adjective + noun, as in (A).

### (E) noun + exists
The following is the case for the flag whether it exists or not.
Note that the word order is different.
```dart
soldOutItemExists
// true: sold-out items exist
// false: sold-out item does not exist
```
The noun part can also contain an adjective + noun, as in (A).

### (F) has/contains + noun
The following is the case for flags that have/include
```dart
containsCheckedOutPlayers
// true: contains checked out visitors or
// false: does not contain checked out visitors
```
The noun part can also contain an adjective + noun, as in (A).

### (G) is + adjective
This is the basic form of the boolean flag.
```dart
isNew
isImported // Imported or not. The passive form of the verb (~ed) is the same as the adjective
isOrderable // Whether it is orderable. Verb +able is also available
```

### (H) on + noun + adjective
```dart
onRowClicked // when a row is clicked
onPaginationChanged // when the pagination is changed
onDeleteButtonClicked // when the delete button is clicked
onItemDisabled // if the item is disabled

/// If the object is clear, you can also use on+verb passive (~ed)
onClicked // when clicked
onSubmitted // when submitted
```

### (I) to + noun
It is also possible to name something in abbreviated form when converting it.
```dart
/// ❌ NG... Not quite, but long...
const convertTimeFromSecondsToMinutes = (time) => {...}

/// ✅ OK. It's clear that seconds come in the argument, so we know it's a function that converts seconds to minutes.
const toMinutes = (seconds) => {...}
```

### (J) verb + object + adjective
For functions that change the state of an object.
```dart
/// make the receipt already printed
/// receipt = printed
setReceiptPrinted 
```

### (K) verb + object
The most major function naming convention.
```dart
joinStrings // join strings
switchTableWidth // change the table width
sortCategories // sort categories
toggleArchivedItems // toggle archived items
```
As in (A), nouns alone, nouns + nouns, and adjectives + nouns are acceptable for the object.

## Variable declarations

```dart
Bad: ❌
int level, size;
int foo, fooarray[]; /// WRONG!
```

```dart
Good: ✅
int level;     /// indentation level
int size;      /// size of table
String name;   /// name of user
```

## Avoid Unneeded Contexts

```dart
Bad: ❌
const car = {
   carMake: "BMW",
   carModel: 5,
   carColor: "Black"
};
```

```dart
Good: ✅
const car = {
   make: "BMW",
   model: 5,
   color: "Black"
};
```

## Remove Dead Code

```dart
Bad: ❌
function oldRequestModule(url) {
	// ...
}
function newRequestModule(url) {
	// ...
}
const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

```dart
Good: ✅
function newRequestModule(url) {
	// ...
}
const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

## DON'T emit twice

```dart
Bad: ❌
void setVirtualCardData(bool finishAnimation) {
  _finishAnimation = finishAnimation;
  if (_dataLoaded && finishAnimation) {
    emit(state.copyWith(pointCardPoint: _pointCardPoint));
    emit(state.copyWith(Id: _Id));
    emit(state.copyWith(exampleCardMode: VirtualCardMode.success));
  }
}
```

```dart
Good: ✅
void setVirtualCardData(bool finishAnimation) {
  _finishAnimation = finishAnimation;
  if (_dataLoaded && finishAnimation) {
    emit(state.copyWith(
        pointCardPoint: _pointCardPoint,
        Id: _Id,
        exampleCardMode: VirtualCardMode.success));
  }
}
```

## Use extension when add format change

```dart
Bad: ❌
class StrUtil {
  static String convertRequestDate(DateTime date) {
    DateTime utc = date.toUtc();
    DateTime jpTime = utc.add(const Duration(hours: 9));
    DateFormat format = DateFormat('yyyyMMddHHmm', 'en_US_POSIX');
    return format.format(jpTime);
  }

  static String convertRequestDateFromServerStr(String date) {
    DateFormat format =
        DateFormat('EEE, dd MMM yyyy HH:mm:ss Z', 'en_US_POSIX');
    DateTime d = format.parse(date);
    d = d.add(const Duration(hours: 9));
    DateFormat formatSend = DateFormat('yyyyMMddHHmm', 'en_US_POSIX');
    return formatSend.format(d);
  }

  ///文字列をSHA256ハッシュ値に変換する
  ///@return SHA256ハッシュ値
  static String getSha256(String target) {
    List<int> targetInBytes = utf8.encode(target);
    Digest value = sha256.convert(targetInBytes);
    return value.toString();
  }
}
```

```dart
 Good: ✅
 extension StringParsing on String {
  static String convertRequestDateFromServerStr(String date) {
    DateFormat format =
        DateFormat('EEE, dd MMM yyyy HH:mm:ss Z', 'en_US_POSIX');
    DateTime d = format.parse(date);
    d = d.add(const Duration(hours: 9));
    DateFormat formatSend = DateFormat('yyyyMMddHHmm', 'en_US_POSIX');
    return formatSend.format(d);
  }

  ///文字列をSHA256ハッシュ値に変換する
  ///@return SHA256ハッシュ値
  static String getSha256(String target) {
    List<int> targetInBytes = utf8.encode(target);
    Digest value = sha256.convert(targetInBytes);
    return value.toString();
  }
}
```

## Do use generate image and icon paths

Add assets and path following image:

![image](https://media.github.kddi.com/user/1972/files/b549d0ec-983c-4484-90b7-cc028b16085c)

**Generate command:**
```dart
flutter clean && flutter pub get && dart run build_runner build -d
```

```dart
Bad: ❌
Lottie.asset(
    'assets/images/example/barcode_example.json',
    width: width,
    height: height * 35 / 100,
    fit: BoxFit.fill,
),
```

```dart
Good: ✅
Lottie.asset(
    Assets.images.example.barcodeExample,
    width: width,
    height: height * 35 / 100,
    fit: BoxFit.fill,
),
```

## Commit Message Header

```dart
Bad: ❌
1. optimize landing page
2. send email
3. oops
4. I think I fixed it this time?
```

```dart
Good: ✅
1. performance: optimize loading of items on landing page
2. feature: send an email to the customer when a product is shipped
3. fix: add the correct company name to the footer and replacing the dummy text
4. revert: revert a previously introduced bug in items retrieving from database
```

## Do not call toString() on nullable types directly.

```dart
Bad ❌
class Store {
  String? name;
  int? id;
}
final store = Store();
final storeName = store.name.toString(); // -> null
final storeId = store.id; // -> null
```

```dart
Good ✅
class Store {
  String? name;
  int? id;
}
final storeName = store.name ?? 'default name'; // -> 'default name'
final storeId = store.id?.toString() ?? 'default id'; // -> 'default id'
```

## Don't use Flutter icons, use icon from Adobe XD

```dart
Bad: ❌
 Icon(
   Icons.audiotrack,
   color: Colors.green,
   size: 30.0,
 ),
```

```dart
Good: ✅
SvgPicture.asset(
  Assets.images.bottomNavigation.homeNormal, 
  width: 32, 
  height: 26),
```
```dart
Good: ✅
Image.asset(
 Assets.images.loading.launchLogo.path
),
```

## Use convenient logger framework instead of print and printDebug

```dart
Bad: ❌
print('Error occurred execute parse: $date');
```

```dart
Good: ✅
Logger().e('Error occurred execute parse: $date');
```

## Don’t use `try, finally` use `try, catch`

```dart
Bad ❌
try {
  return DateFormat(format).parse(date);
} finally {
  Logger().e('Error occurred execute parse: $date');
}
```

```dart
Good ✅
try {
  return DateFormat(format).parse(date);
} catch (ex) {
  Logger().e('Error occurred execute parse: $date');
}
```

## **Use `CommonColor` for Colors**

Always use `CommonColor` instead of hardcoding color values:

```dart
/// Incorrect
this.color = const Color.fromARGB(255, 40, 5, 5); // ❌

/// Correct
this.color = const CommonColor.black1; // ✅
```

## **Page Names Should Be in Japanese**

Define route names and paths in Japanese:

```dart
/// Incorrect
class BottomNavigation extends HookConsumerWidget {
  const BottomNavigation({super.key});
  static const path = '/';
  static const name = 'Bottom Navigation'; // ❌
}

/// Correct
class HomePageRoute extends GoRouteData {
  const HomePageRoute();
  static const path = '/home';
  static const fullPath = HomePageRoute.path;
  static const name = 'ホーム - トップ画面'; // ✅
}
```

## **Hard-code Strings Only If Used Once on the Screen**

If a string is used only once on a specific screen, hard-coding is acceptable:


```dart
/// Incorrect
const todaysTransaction = 'Today's Transaction'; // ❌

/// Correct
Text("Today's Transaction") // ✅

```

## **Handle Logic Separately from UI**

Separate logic processing from the UI elements:

```dart
// Incorrect: Logic directly in the onClick handler
onClick: () {
  if (condition) {
    // Do something
  } else {
    // Do something else
  }
} // ❌

// Correct: Use a function from the logic class
onClick: () {
  logic.function();
} // ✅

```

## **Use Private Variables/Functions Where Applicable**

Make variables and functions private if they are used only within a specific class and should not be modified from other classes:

```dart
/// Incorrect
void fetchOnboardingStatus() { ... } // ❌

/// Correct
void _fetchOnboardingStatus() { ... } // ✅

```

## **Prefer Using enum Instead of String Constants**

Where applicable, use enum instead of plain string constants for better type safety and readability:

```dart
/// Example
enum StoreSelectionMode { 
    shouldSaveSelectedStore, 
    showAllStoreSelection 
    }

/// Usage
final storeSelectionMode = StoreSelectionMode.shouldSaveSelectedStore;

```

## **If using push into async function please add await**

```dart
onTap: () async {
   await const examplePageRoute().push<void>(context);
}
```

## Reference links:
- [Effective-dart/style](https://dart.dev/effective-dart/style)
- [Programming principles](https://www.makeuseof.com/tag/basic-programming-principles/)
- [Stupid to SOLID Code!](https://williamdurand.fr/2013/07/30/from-stupid-to-solid-code/)
- [infinum.com/handbook/](https://infinum.com/handbook/flutter/basics/coding-style)
- [www.mindinventory.com](https://www.mindinventory.com/blog/flutter-development-best-practices/)
- [Naming rules](https://qiita.com/YutaManaka/items/62dda256bb7ba6c08399)
- [Effective Dart](https://gautam007.medium.com/effective-dart-writing-idiomatic-dart-code-5a062ce3e62f)
- [Test Method](https://agile.kddi.com/confluence/pages/viewpage.action?pageId=761957716)
