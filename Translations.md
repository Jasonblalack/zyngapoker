You can easily enable translations in all Jasy projects by adding translation data files (named by the exact locale identifier) to your application folder. You also have to use specific functions/signatures to let Jasy generate the matching data for your actual code base. 

## Setup locales

You have to setup and configure locale support in your Jasy project to make translations working. Take a look at the [[Locales]] documentation for details.

## Adding translations

Translations are defined in files named by using the standardized locale identifiers. Jasy currently supports [Gettext](http://www.gnu.org/software/gettext/) formatted files only. These files are plain text files with some special formatting. The filename needs to end with `.po`. Depending on your projects you put these either in `source/translation` or `translation` (or define them via a manual `content` section in your configuration file).

- `source/translation/de.po`: German texts
- `source/translation/en.po`: English texts
- `source/translation/fr.po`: French texts

**Note:** For a good introduction into Gettext also take a look at their [introduction pages](http://www.gnu.org/software/gettext/manual/gettext.html#Introduction).

Jasy automatically finds all translations for the current locale in all active projects. 

**Note:** The results from all translation data files are merged. This allows e.g. for having a shared pool of translations in a project you require in different applications. But this also means that there might be conflicts between translations with the same identifier but different translated results. This is typically not a major issue, but just keep that in mind.

## Gettext Format
