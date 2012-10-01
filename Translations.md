You can easily enable translations in all Jasy projects by adding translation data files (named by the exact locale identifier) to your application folder. You also have to use specific functions/signatures to let Jasy generate the matching data for your actual code base. 

## Setup locales

You have to setup and configure locale support in your Jasy project to make translations working. Take a look at the [[Locales]] documentation for details.

## Adding translations

Translations are defined in files named by using the standardized locale identifiers. Jasy currently supports [Gettext](http://www.gnu.org/software/gettext/) formatted files only. These files are plain text files with some special formatting. The filename needs to end with `.po` (a so called PO file). Depending on your projects you put these either in `source/translation` or `translation` (or define them via a manual `content` section in your configuration file).

- `source/translation/de.po`: German texts
- `source/translation/en.po`: English texts
- `source/translation/fr.po`: French texts

**Note:** For a good introduction into Gettext also take a look at their [introduction pages](http://www.gnu.org/software/gettext/manual/gettext.html#Introduction).

Jasy automatically finds all translations for the current locale in all active projects. 

**Note:** The results from all translation data files are merged. This allows e.g. for having a shared pool of translations in a project you require in different applications. But this also means that there might be conflicts between translations with the same identifier but different translated results. This is typically not a major issue, but just keep that in mind.

## Gettext Format

A PO file is made up of many entries, each entry holding the relation between an original untranslated string and its corresponding translation. All entries in a given PO file usually pertain to a single project, and all translations are expressed in a single target language. One PO file entry has the following schematic structure:

    white-space
    #  translator-comments
    #. extracted-comments
    #: reference...
    #, flag...
    #| msgid previous-untranslated-string
    msgid untranslated-string
    msgstr translated-string

The general structure of a PO file should be well understood by the translator. When using PO mode, very little has to be known about the format details, as PO mode takes care of them for her.

A simple entry can look like this:

    #: myapp.view.Main.js:116
    msgid "Unknown system error"
    msgstr "Error desconegut del sistema"

Entries begin with some optional white space. Usually, when generated through translation tools, there is exactly one blank line between entries. Then comments follow, on lines all starting with the character `#`. There are two kinds of comments: those which have some white space immediately following the `#` - the translator comments -, which comments are created and maintained exclusively by the translator, and those which have some non-white character just after the `#` - the automatic comments -, which comments are created and maintained automatically by translation tools. Comment lines starting with `#.` contain comments given by the programmer, directed at the translator; these comments are called extracted comments because they are extracted by translation tools from the program's source code. Comment lines starting with `#:` contain references to the program's source code. Comment lines starting with `#,` contain flags; more about these below. Comment lines starting with `#|` contain the previous untranslated string for which the translator gave a translation.

All comments, of either kind, are optional.

After white space and comments, entries show two strings, namely first the untranslated string as it appears in the original program sources, and then, the translation of this string. The original string is introduced by the keyword `msgid`, and the translation, by `msgstr`.

### Context

It is also possible to have entries with a context specifier. They look like this:

    white-space
    #  translator-comments
    #. extracted-comments
    #: reference...
    #, flag...
    #| msgctxt previous-context
    #| msgid previous-untranslated-string
    msgctxt context
    msgid untranslated-string
    msgstr translated-string

The context serves to disambiguate messages with the same untranslated-string. It is possible to have several entries with the same untranslated-string in a PO file, provided that they each have a different context. Note that an empty context string and an absent `msgctxt` line do not mean the same thing.

### Plural Forms

A different kind of entries is used for translations which involve plural forms.

     white-space
     #  translator-comments
     #. extracted-comments
     #: reference...
     #, flag...
     #| msgid previous-untranslated-string-singular
     #| msgid_plural previous-untranslated-string-plural
     msgid untranslated-string-singular
     msgid_plural untranslated-string-plural
     msgstr[0] translated-string-case-0
     ...
     msgstr[N] translated-string-case-n

Such an entry can look like this:

     #: myapp.view.Sub.js:320
     msgid "found %1 fatal error"
     msgid_plural "found %1 fatal errors"
     msgstr[0] "s'ha trobat %1 error fatal"
     msgstr[1] "s'han trobat %1 errors fatals"

Here also, a context can be specified before `msgid`, like above.

## JavaScript API

TODO

## Plural Forms

TODO