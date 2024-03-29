Jasy support CLDR based locales being used in any application. Using locales is super simple and works dynamically with data included in the Jasy distribution/install. There is no need for any other installations or new requirements when locales should be enabled.

## What is a locale?

A locale is a short abbreviation of the language of the user and the country code. Examples look like:

- `de_AT`: German, Austria
- `en_US`: English, USA
- `fr_FR`: French, France

Locales are also valid and possible to match against the system when the county code is missing:

- `de`: German
- `en`: English
- `fr`: French

## Unicode CLDR based

Jasy's locale support is based on data offered by the [Unicode CLDR repository](http://cldr.unicode.org).

### About

The Unicode CLDR provides key building blocks for software to support the world's languages, with the largest and most extensive standard repository of locale data available. This data is used by a wide spectrum of companies for their software internationalization and localization, adapting software to the conventions of different languages for such common software tasks as:

* Formatting of dates, times, and time zones, 
* Formatting numbers and currency values
* Sorting text
* Choosing languages or countries by name

### Transformation

Jasy dynamically translates the XML data offered by CLDR into Jasy-enabled projects. Based on the current locale the correct project is added to the project list and classes inside that project are offered during e.g. dependency calculation. 

## Enabling locales

To enable locales you have to enable the locales you want to use in your `jasyscript.py` (before the permutation loop):

```python
session.setLocales(["de_AT", "en_US", "fr_FR"])
```

This enables locales inside the permutation loop. It automatically creates locale projects and adds them to the project list automatically.

```python
for permutation in session.permutate():
    <other code>
```

The locale system uses the field name `locale` internally. You can query the current locale during the permutation loop using `permutation.get("locale")`.

**Note:** Locale projects are automatically created inside your application folder under `.jasy/locale/LOCALE`. To re-create them you need to delete the folder. These projects are using the normal project cache infrastructure and keep compiled results e.g. under that folder as well.

### No Country Code on Client Side

Unfortunately there is no clean way to figure out the exact locale of the system in the browser. The only thing you get is the language (using `navigator.userLanguage`). This means that for 100% exact locale support (following the conventions of the country the visitor lives in) you have to figure out the locale using a different approach:

* Using the request headers from a HTTP request (contains Accept-Language header) and return that to the client as a response
* Figure out the client's IP and match that IP via an IP database
* Manually ask the user to select the country (and maybe language).

Note: To support Jasy automatically selecting the locale on the client via built-in mechanisms (like `core.detect.Locale`) it is suggested to limit yourself to the language in `session.setLocales()` and not making use of full locales with country codes.

```python
session.setLocales(["de", "en", "fr"])
```

## Using locales

As locale projects are automatically added to the project list during the permutation loop you can easily use the data offered by locale projects using simple class name access in your other JavaScript code like:

```javascript
alert("Paper Size: " + locale.Measurement.PAPER_SIZE);
alert("Percent Format: " + locale.number.Format.PERCENT);
alert("Currency Format: " + locale.number.Format.CURRENCY);
```

You can also query the current locale being used using `jasy.Env.getValue("locale")`.

### Available classes

- `locale.calendar.gregorian.Date`
- `locale.calendar.gregorian.Datetime`
- `locale.calendar.gregorian.day.Abbreviated`
- `locale.calendar.gregorian.day.Narrow`
- `locale.calendar.gregorian.day.Wide`
- `locale.calendar.gregorian.Field`
- `locale.calendar.gregorian.month.Abbreviated`
- `locale.calendar.gregorian.month.Narrow`
- `locale.calendar.gregorian.month.Wide`
- `locale.calendar.gregorian.quarter.Abbreviated`
- `locale.calendar.gregorian.quarter.Narrow`
- `locale.calendar.gregorian.quarter.Wide`
- `locale.calendar.gregorian.relative.Day`
- `locale.calendar.gregorian.relative.Month`
- `locale.calendar.gregorian.relative.Week`
- `locale.calendar.gregorian.relative.Year`
- `locale.calendar.gregorian.Time`
- `locale.CalendarPref`
- `locale.Delimiter`
- `locale.display.Key`
- `locale.display.Language`
- `locale.display.Measure`
- `locale.display.Script`
- `locale.display.Territory`
- `locale.display.Type`
- `locale.display.Variant`
- `locale.Info`
- `locale.key.Full`
- `locale.key.Short`
- `locale.Measurement`
- `locale.number.CurrencyName`
- `locale.number.CurrencySymbol`
- `locale.number.Format`
- `locale.number.Symbol`
- `locale.Plural`
- `locale.Week`

The available calendar data depends on the locale. There are different ones available like `buddhist`, `chinese`, `coptic`, `ethiopic`, `gregorian`, `hebrew`, `indian`, `islamic`, `japanese`, `persian` and `roc`. The classes under this package are basically the same in each locale e.g. `locale.calendar.<type>.month.Narrow`. The `<type>` can be resolved by querying `locale.CalendarPref.ORDERING` first. This is an sorted array of calendar types to use in that locale.