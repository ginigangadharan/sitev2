source "https://rubygems.org"
ruby RUBY_VERSION

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#

# If you have any plugins, put them here!
gem 'wdm', '>= 0.1.0' if Gem.win_platform?
gem "jekyll-sass-converter", "~> 3.0.0"
# refer forestry.io/blog/how-i-reduced-my-jekyll-build-time-by-61/
# gem "liquid-c"

group :jekyll_plugins do
    gem 'jekyll-feed'
    gem 'jekyll-sitemap'
    gem 'jekyll-paginate'
    gem 'jekyll-seo-tag'
    gem "kramdown-parser-gfm"
    gem "kramdown", ">= 2.3.1"
    gem 'jekyll-admin'
end