Jasy support CLDR based locales being used in any application. Using locales is super simple and works dynamically with data included in the Jasy distribution/install.

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

Jasy's locale support is based on data offered by the Unicode CLDR repository.

### About CLDR

The Unicode CLDR provides key building blocks for software to support the world's languages, with the largest and most extensive standard repository of locale data available. This data is used by a wide spectrum of companies for their software internationalization and localization, adapting software to the conventions of different languages for such common software tasks as:

* Formatting of dates, times, and time zones, 
* Formatting numbers and currency values
* Sorting text
* Choosing languages or countries by name

### Transformation

Jasy dynamically translates the XML data offered by CLDR into Jasy-enabled projects. Based on the current locale the right project is added to the project list and classes inside that project are offered during e.g. dependency calculation.


## Using locales

To enable locales you have to enable the locales you want to use in your `jasyscript.py`:

```python
session.setLocales(["de", "en"])
```


## Locales vs. Translations

Unfortunately there is no clean way to figure out the exact locale of the system in the browser. The only thing you get is the language (using `navigator.userLanguage`). This means that for 100% exact locale support (following the conventions of the country the visitor lives in) you have to figure out the locale using a different approach. One such approach could be using the request headers from a HTTP request (contains Accept-Language header) and return that to the client as a response again. Alternatively you could also figure out the IP and match that IP via a IP database.

Note: To support Jasy auto-selecting the locale on the client via built-in mechanisms (like `core.detect.Locale`) it is suggested to limit yourself to the language in `session.setLocales()` and not making use of full locales with country codes.