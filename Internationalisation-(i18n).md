## Internationalisation (i18n) and Transifex

The OFN is available in [several languages](http://community.openfoodnetwork.org/t/localisation-ofn-in-your-language/397). You can help translating at [Transifex](https://www.transifex.com/open-food-foundation/open-food-network/). You can just sign up and start translating in your language. If your language doesn't exist yet, one of the Transifex admins has to add it. Also ask the Transifex admins if you would like to be a reviewer or maintainer of a certain language. You find the Transifex admins in the `#multilingual` channel on [Slack](https://openfoodnetwork.org/slack-invite):

- maikel
- sigmundpetersen

The next section explains in more details how the translations in Transifex make it back to the OFN code.

## Transifex and Github integration

Our code is on Github and that is where our source locale `en` is maintained. To enable people to translate it easily via Transifex, we need to feed it into Transifex and then get the translations back into Github again. Here is the story of the current process. Our main actors are Github, Transifex and our Continuous Integration (CI) server.

Quite regularly, a developer changes some code that involves changing text. Text is changed in the file `config/locales/en.yml`. Once that change has made it into our production code, the master branch on Github, it needs translating into all the other languages. Transifex is configured to check [en.yml on Github](https://raw.githubusercontent.com/openfoodfoundation/openfoodnetwork/master/config/locales/en.yml) every day and make any changes available for translation.

Translators may notice that their language is not 100% translated due to changes of the source file. Then they translate and once a language reaches 100% again, Transifex notifies our CI server.

Our CI server is running [txgh](https://github.com/openfoodfoundation/txgh/blob/ofn/README.ofn.md) to listen to Transifex. When a translation reaches 100%, it will download it from Transifex and push it to Github on the "transifex" branch. It will create the branch if it doesn't exist. It also opens a pull request so that we can easily merge the changes into master for the next [[release|Releasing#how-to-make-a-release-spiral_notepad-white_check_mark]].

Please note that once a translation reached 100%, Transifex doesn't notify our CI server about any further changes. You can trigger another notification by reviewing the translation to 100%. Any updates after the review have to be downloaded and committed manually.

## Transifex client

The [Transifex client](https://github.com/transifex/transifex-client) is a command line tool that helps with manually downloading translations. Follow the readme file to install it. It then needs a configuration file to authenticate to the API. Obtain your [Transifex API key](https://www.transifex.com/user/settings/api/) and place the following in `$HOME/.transifexrc`:

```
[https://www.transifex.com]
hostname = https://www.transifex.com
password = 1/5e840xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxf54ca9
username = api
```

You can then download all translations with one command:

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

  Use ["lazy" lookup](https://www.rubydoc.info/gems/i18n_lazy_scope).
  In order to use the lazy lookup you need to know where you are (for example if you are using deface in a spree view). To do this, you can use lazy lookup '.your_translation_key' and go to the page where the translation is used, you will have an error on the page (or you can look in the DOM for an element with class "translation_missing"): the error message will tell you where in the nested structure of the en.yml file you will have to put your key in order to use lazy lookup. 

For example to use `t('legal_settings') in the view `/app/views/spree/admin/general_settings/edit.html.haml`, your translation key needs to be at `en.spree.admin.general_settings.edit.legal_settings` in the `en.yml` file, otherwise you will see this DOM element rendered in your template:
    - `<span class="translation_missing" title="translation missing: en.spree.admin.general_settings.edit.legal_settings">Legal Settings</span>.`
  
  NOTE: If you are on an Angular template, you cannot use lazy lookup. Instead you should use the nested structure so that translation keys are well organised.

  Avoid global keys. There are a lot already. And some are okay, for example
  "enterprises" should be the same everywhere on the page. But in doubt,
  create a new translation and give it a meaningful scope.

  Don't worry about duplication. We may use the same word in different contexts,
  but another language could use different words. So don't try to re-use
  translations between files.

* Remove translations: Be careful. If you are sure that a translation is not used anywhere,
  please remove them. But be aware that some translations are looked up with
  variables. For example https://github.com/openfoodfoundation/openfoodnetwork/blob/deb04c3075e35a1d839b58206a5363dd99e1403d/app/views/admin/contents/_fieldset.html.haml#L6 looks
  up labels for preferences. Unfortunately, they don't have a scope.

  It's also possible that the look up is in a gem. We still use a lot of Spree's views. So just because you can't find a translation key in the OFN code, it doesn't mean that the translation is not needed.

* Add missing Spree translations: you may find that a spree view or controller complains about missing translations. The fix to these problems is to look up the translation in one of the en.yml files in Spree and copy the translation entry to OFN's en.yml file.
  Spree.t() is just a warpper around t() that adds the spree namespace, for example, Spree.t('settings') is the same as t('spree.settings'). But we don't want to use Spree translations, so, all Spree.t calls should be converted to normal t() calls, if possible, using lazy loading.

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

### Rails view translations: Prefer Ruby `t` method

**Bad**

```
%input.ofn-select2.fullwidth{ blank: "{id: 0, name: ( 'js.admin.order_cycles.filters.any_enterprise' | t )}", data: 'enterprises' }
```

**Good**

```
%input.ofn-select2.fullwidth{ blank: "{id: 0, name: '#{j t(".any_enterprise")}'}", data: 'enterprises' }
```

Also note that, as in this example, you should escape JS with the j function when inserting an arbitrary Ruby string in a JS or Angular expression. See [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Code-Conventions#use-j-when-inserting-an-arbitrary-ruby-string-in-a-js-or-angular-expression) for more details.

### JavaScript translations: Prefer Angular Filter over global `t` function

In JavaScript it's better to use our Angular filter for translations:

Bad 
```join(" " + t('shop.products.filter_properties_or' ) + " ")```

Good: 
```join(" {{ 'shop.products.filter_properties_or' | t }} ")```

In some cases we had problems with the global `t` function not working when called directly. I think it depends on the scope in which the code is running. Using the `t` filter is the safer option and we should try to be consistently using only that one in JavaScript.

### Javascript translations: Put translation key under the "js." namespace

All translation keys used by the Javascript should be put under the "js." namespace. If the same text will be used elsewhere, add another translation key for this outside the "js." namespace.

See [#2721](https://github.com/openfoodfoundation/openfoodnetwork/issues/2721) and [#2722](https://github.com/openfoodfoundation/openfoodnetwork/issues/2722) for context.

## Further reading

* [Transifex Maintainers and Governance](https://community.openfoodnetwork.org/t/transifex-maintainers-and-governance/867)
* [Original feature project notes](http://community.openfoodnetwork.org/t/internationalisation-project-notes/312)
