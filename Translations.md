You can easily enable translations in all Jasy projects by adding translation data files (named by the exact locale identifier) to your application folder. You also have to use specific functions/signatures to let Jasy generate the matching data for your actual code base. 

## Setup locales

You have to setup and configure locale support in your Jasy project to make translations working. Take a look at the [Locales] documentation for details.

## Adding translations

Translations are defined in files named by using the standardized locale identifiers. Jasy currently supports Gettext formatted files only. These files are plain text with some special formatting. The file name needs to end with `.po`. Depending on your projects you put these either in `source/translation` or `translation` (or define them via a manual `content` section in your configuration file).

- `source/translation/de.po`: German texts
- `source/translation/en.po`: English texts
- `source/translation/fr.po`: French texts

You have to setup locales to make translations work.