# jqtranslate

jqtranslate is a quick way to implement multilanguage in cordova/phonegap app or web based applications.

jqtranslate is a jquery based plugin.

Discussions, comments, feedback here: https://www.facebook.com/jqtranslate

## Usage 1 - Translating all page elements

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
</head>
<body>
	<p t='Welcome message' tv='{"username":"chattambulo"}'></p>

	<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
	<script src="jqtranslate.min.js"></script>
	<script>
		jqtranslate.init({language:"en"});
	</script>
</body>
</html>
```

Localization strings are located in "lang/`language code`.json" (for example "lang/en.json", "lang/it.json", ...).

```
{
	"Welcome message" : "Welcome {{username}}!"
}
```
Result:
```
Welcome chattambulo!
```

## Usage 2 - Translating single element
```
alert(jqtranslate.get('Welcome message',{"username":"chattambulo"}));
```

or using shortcut
```
alert(_('Welcome message',{"username":"chattambulo"}));
```

Example:
```
var user = "chattambulo";
alert(_('Welcome message',{"username":(user)}));
```

## Plural forms

As specified in the [Unicode plural rules](http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html) 
you can choose a plural form as `zero | one | two | few | many | other`, depending on the param `n` and the current language.

##### Working language codes

The plugin is under development, so language patterns are NOT COMPLETED.

Actually the plural form works with the following language codes:

2016-04-26, added: be, bn

2016-04-15, added: as, ast, asa, az, eu

2016-04-14, added: af, ak, sq, am, ar, hy, nl, fr, en, it

EXAMPLE 1

lang/en.json

```
{
	"Total messages"  : {
		"one" : "You have {{n}} message.",
		"other" : "You have {{n}} messages."
		}
}
```

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
</head>
<body>

	<p t="Total messages" tv='{"n":"0"}'></p>
	<p t="Total messages" tv='{"n":"1"}'></p>
	<p t="Total messages" tv='{"n":"2"}'></p>

	<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
	<script src="jqtranslate.min.js"></script>
	<script>
		jqtranslate.init({language:"en"});
	</script>
</body>
</html>
```

RESULT

```
You have 0 messages.

You have 1 message.

You have 2 messages.
```

In addition to this (and have priority against `zero | one | two | few | many | other` pattern) you can specify a ` n` value for better traslating context.

EXAMPLE 2

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
</head>
<body>

	<p t="Total messages" tv='{"n":"0"}'></p>
	<p t="Total messages" tv='{"n":"1"}'></p>
	<p t="Total messages" tv='{"n":"2"}'></p>

	<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
	<script src="jqtranslate.min.js"></script>
	<script>
		jqtranslate.init({language:"en"});
	</script>
</body>
</html>
```

lang/en.json

```
{
	"Total messages"  : {
		"one" : "You have {{n}} message.",
		"other" : "You have {{n}} messages.",
		"0" : "Sorry, no messages.",
		"1" : "Only one message."
		}
}
```

RESULT

```
Sorry, no messages.

Only one message.

You have 2 messages.
```

EXAMPLE 3

html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title></title>
</head>
<body>

	<p t='Welcome with messages' tv='{"n":"1","username":"chattambulo"}'></p>

	<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
	<script src="jqtranslate.min.js"></script>
	<script>
		jqtranslate.init({language:"en"});
	</script>
</body>
</html>
```

lang/en.json

```
{
	"Welcome with messages"  : {
		"one"   : "Welcome {{username}}, you have {{n}} message.",
		"other" : "Welcome {{username}}, you have {{n}} messages.",
		"0"     : "Welcome {{username}}, sorry, no messages.",
		"1"     : "Welcome {{username}}, only one message."
		}
}
```

RESULT

```
Welcome chattambulo, only one message.
```


## Working in cordova/phonegap project

First you must have the cordova globalization plugin installed:

`$ cordova plugin add cordova-plugin-globalization`

Remember to include jqtranslate.min.js in your project and have jquery loaded.

So, you can use a javascript to detect device language:

```javascript
document.addEventListener("deviceready", startGlobalization, false);

var lang;                 // Current language
var dlang = "en";         // Default language
var langs = ["en", "it"]; // Defined languages

document.addEventListener("deviceready", startGlobalization, false);

function startGlobalization() {
	navigator.globalization.getPreferredLanguage(
	function (language) {
		lang = language.value;
		var parts = lang.split('-');
		lang= parts[0];
		if(langs.indexOf(lang)<0) lang = dlang;
		jqtranslate.init({"language":lang});
	},
	function () {alert('Error getting language\n');}
	);
}
```

END