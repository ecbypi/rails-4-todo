# Rails 4 Upgrade Notes and TODOs

The following is a list of changes that aren't necessarily documented / useful
to have a reference for.

Be sure you've read the official
[guides](http://guides.rubyonrails.org/upgrading_ruby_on_rails.html) and
[release notes](http://guides.rubyonrails.org/4_0_release_notes.html).

Also review the `CHANGELOG`s in the [GitHub repository](https://github.com/rails/rails).

## Pre-upgrade

Recommended changes before upgrading. These are not necessary, but can be made
while still on Rails 3.2 and will ease the amount of work required to upgrade.

### Gems

* Review your dependencies on http://www.ready4rails4.net/
* Switch to using the `strong_parameters` gem prepare for the removal of
  `attr_accessible` and/or `attr_protected`. `attr_accessible` and
  `attr_protected` have been extracted to a gem if this isn't feasible.
* Update to the latest version of `simple_form`. This will require removing all
  HTML markup from `simple_form`'s i18n file. You will still have to update
  `simple_form` as part of the Rails update process, but you won't have to deal
  with HTML markup in i18n for labels.
* Update to the latest version of `devise` so you'll have `strong_parameters`
  support out of the box. Also forces you to deal with some of the upstream
  changes made to `devise` separately from updating Rails.
* Ensure you're using the latest `database_cleaner` gem to include the
  following fix: https://github.com/DatabaseCleaner/database_cleaner/pull/253

### Asset Pipeline

* Switch `[asset-type]-url` helpers in any `sass` files to `asset-url('file',
  [asset-type])`.
* Add `vendor/images` to precompile path
* Remove `:assets` group from Gemfile and remove `RAILS_GROUP` functionality
  from `Bundler.require` in `config/application.rb`. NOTE: To avoid bloating
  you application start-up time, consider vendoring assets that are provided
  as-is by a gem without additional functionality.

### `ActiveModel`

* Use multi-line anchors for regular expressions when using the `ActiveModel`
  `:format` validator.

### `ActiveRecord`

* Use `to_a` when you want an `Array` instead of an `ActiveRecord::Relation`.
* Wrap all relations passed to `scope` in a `lambda`.


### Configuration

* Specify `config.eager_load` in all environments.
* Use http://railsdiff.org/ to review upcoming changes to the code generated
  by `rails new` to see any additional configuration changes that may affect you.

## Post-Update

Changes to make after updating to Rails 4. The above changes are all included
if you didn't make them before updating.

### Gems

* Remove version requirements for `sass-rails`, `coffee-rails` and `uglifier`
* If using `haml`, use `haml-rails` to correclty configure `cache_digests`
* Remove `strong_parameters`, `cache_digests` and `sextant` from the `Gemfile`
  if present.

### Asset Pipeline

* Switch all `-url` `sass` helpers to be `asset-url('file.ext')`.

### Configuration

* Specify compressors for CSS and JavaScript.
```ruby
# remove
config.assets.compress = true

# add
config.assets.js_compressor = :uglifier
config.assets.css_compressor = :sass
```

### Misc

* Convert `db/schema.rb` to use 1.9 hash syntax using `rake db:schema:dump`.
  This makes commits for the first migration under Rails 4 readable since it
  won't include coverting the syntax of the schema file.

### Syntax (optional / deprecations)

* In controllers, use `(before|after)_action` instead of
  `(before|after)_filter`
* Switch to `Model#update` over `Model#update_attributes`
* Use `Model.all` instead of `Model.scoped`.

