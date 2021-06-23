source 'https://rubygems.org'

require 'json'
require 'open-uri'
versions = JSON.parse(open('https://pages.github.com/versions.json').read)

gem 'github-pages', versions['github-pages']
gem "nokogiri", ">= 1.11.0.rc4"
gem "rake", ">= 12.3.3"
gem "kramdown", ">= 2.3.0"
gem "activesupport", ">= 6.0.3.1"