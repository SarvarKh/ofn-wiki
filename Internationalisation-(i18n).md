## Internationalisation (i18n) and Transifex

The OFN is available in [several languages](http://community.openfoodnetwork.org/t/localisation-ofn-in-your-language/397). You can help translating at [Transifex](https://www.transifex.com/open-food-foundation/open-food-network/). You can just sign up and start translating in your language. If your language doesn't exist yet, one of the Transifex admins has to add it. Also ask the Transifex admins if you would like to be a reviewer or maintainer of a certain language. You find the Transifex admins in the `#multilingual` channel on [Slack](https://openfoodnetwork.org/slack-invite):

- maikel
- sigmundpeterson

The next section explains in more details how the translations in Transifex make it back to the OFN code.

## Transifex and Github integration

Our code is on Github and that is where our source locale `en` is maintained. To enable people to translate it easily via Transifex, we need to feed it into Transifex and then get the translations back into Github again. Here is the story of the current process. Our main actors are Github, Transifex and our Continuous Integration (CI) server.

Quite regularly, a developer changes some code that involves changing text. Text is changed in the file `config/locales/en.yml`. Once that change has made it into our production code, the master branch on Github, it needs translating into all the other languages. Transifex is configured to check [en.yml on Github](https://raw.githubusercontent.com/openfoodfoundation/openfoodnetwork/master/config/locales/en.yml) every day and make any changes available for translation.

Translators may notice that their language is not 100% translated due to changes of the source file. Then they translate and once a language reaches 100% again, Transifex notifies our CI server.

Our CI server is running [txgh](https://github.com/openfoodfoundation/txgh/blob/ofn/README.ofn.md) to listen to Transifex. When a translation reaches 100%, it will download it from Transifex and push it to Github on the "transifex" branch. It will create the branch if it doesn't exist. It also opens a pull request so that we can easily merge the changes into master for the next [[release|Releasing#how-to-make-a-release-spiral_notepad-white_check_mark]].

Please note that once a translation reached 100%, Transifex doesn't notify our CI server about any further changes. You can trigger another notification by reviewing the translation to 100%. Any updates after the review have to be downloaded and committed manually.

The [Transifex client](https://github.com/transifex/transifex-client) is a command line tool that helps with manually downloading translations. After installing, you can replace all locales with the latest Transifex version:

```sh
# cd openfoodnetwork
tx pull --force
```

## Development

You are welcome to fix typos, add missing translations and remove unused ones in `config/locales/en.yml`. Don't change the other locales as that creates conflicts with our Transifex integration described above.

Here are some guidelines to make sure that the source locale file `en.yml` is becoming more beautiful
with every change we do.

* Change text: No problem. Fix the typo. And please enclose the text in quotes
  to avoid any accidents.

  - `example_1: "When you're using double quotes, they look like \"this\""`
  - `example_2: "When you’re using double quotes, they look like “this”"`

  The second example uses unicode to make it look prettier and avoid backslashes.

* Add translations: Cool, every bit of text in the application should be here.

  Use the nested structure. If you add a translation for a view or mailer, please make use of the nested
  structure.

  Use "lazy" lookup. See: http://guides.rubyonrails.org/i18n.html#looking-up-translations
  In order to use the lazy lookup you need to know where you are (for example if you are using deface in a spree view). To do this, you can use lazy lookup '.your_translation_key' and go to the page where the translation is used, you will have an error on the page (or you can look in the DOM for an element with class "translation_missing"): the error message will tell you where in the nested structure of the en.yml file you will have to put your key in order to use lazy lookup. For example (en.spree.admin.general_settings.edit.legal_settings):
    - `<span class="translation_missing" title="translation missing: en.spree.admin.general_settings.edit.legal_settings">Legal Settings</span>.`
  
  If you are on a Angular template, use nested structure but not the lazy lookup (not possible). You cannot use lazy lookup but you should use the nested structure as well so that translation keys are well organised.

  Avoid global keys. There are a lot already. And some are okay, for example
  "enterprises" should be the same everywhere on the page. But in doubt,
  create a new translation and give it a meaningful scope.

  Don't worry about duplication. We may use the same word in different contexts,
  but another language could use different words. So don't try to re-use
  translations between files.

  Don't move big parts around or rename scopes with a lot of entries without
  a really good reason. In some cases that causes a lot of translations in
  other languages to be lost. That causes more work for translators.

* Remove translations: Be careful. If you are sure that a translation is not used anywhere,
  please remove them. But be aware that some translations are looked up with
  variables. For example https://github.com/openfoodfoundation/openfoodnetwork/blob/deb04c3075e35a1d839b58206a5363dd99e1403d/app/views/admin/contents/_fieldset.html.haml#L6 looks
  up labels for preferences. Unfortunately, they don't have a scope.

  It's also possible that the look up is in a gem. We still use a lot of Spree's views. So just because you can't find a translation key in the OFN code, it doesn't mean that the translation is not needed.

There are also some common problems people run into described in the next sections.

### Scopes must be unique

Scopes/keys must be unique at all levels.  If duplicated, they may work in the browser but can result in unrelated failing tests.

**Bad**
```
  shop:
    vegetables:
      carrot: 'carrot'
...
  shop:
    fruit:
      apple: 'apple'
```
**Good**
```
  shop:
    vegetables:
      carrot: 'carrot'
    fruit:
      apple: 'apple'...
```

### JavaScript translations: Prefer Angular Filter over global `t` function

In JavaScript it's better to use our Angular filter for translations:

Bad 
```join(" " + t('shop.products.filter_properties_or' ) + " ")```

Good: 
```join(" {{ 'shop.products.filter_properties_or' | t }} ")```

In some cases we had problems with the global `t` function not working when called directly. I think it depends on the scope in which the code is running. Using the `t` filter is the safer option and we should try to be consistently using only that one in JavaScript.

## Further reading

* [Transifex Maintainers and Governance](https://community.openfoodnetwork.org/t/transifex-maintainers-and-governance/867)
* [Original feature project notes](http://community.openfoodnetwork.org/t/internationalisation-project-notes/312)
