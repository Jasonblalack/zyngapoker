You can easily enable translations in all Jasy projects by adding translation data files to a "translation" folder in your application. You have to use specific functions/signatures to let Jasy generate the matching data for your actual code base.

## Adding translations

Translations are defined in files named by using the standardized locale identifiers. Jasy currently supports Gettext formatted files only. These files are plain text with some special formatting. The file name needs to end with `.po`.

## What is a locale?

A locale is a short abbreviation of the language of the user and the country code. Examples look like:

- `de_AT`: German, Austria
- `en_US`: English, USA
- `fr_FR`: French, France

Locales are also valid and possible to match against the system when the county code is missing:

- `de`: German
- `en`: English
- `fr`: French

Depending on your projects 