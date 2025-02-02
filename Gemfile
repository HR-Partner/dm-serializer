require 'pathname'

source 'https://rubygems.org'

gemspec

SOURCE         = ENV.fetch('SOURCE', :git).to_sym
REPO_POSTFIX   = SOURCE == :path ? ''                                : '.git'
DATAMAPPER     = SOURCE == :path ? Pathname(__FILE__).dirname.parent : 'http://github.com/datamapper'
DM_VERSION     = '~> 1.3.0.beta'
DO_VERSION     = '~> 0.10.6'
DM_DO_ADAPTERS = %w[ sqlite postgres mysql oracle sqlserver ]
CURRENT_BRANCH = ENV.fetch('GIT_BRANCH', 'master')

gem 'dm-core', DM_VERSION,
  SOURCE  => "#{DATAMAPPER}/dm-core#{REPO_POSTFIX}",
  :branch => CURRENT_BRANCH

gem 'fastercsv',  '~> 1.5.4'
gem 'multi_json', '~> 1.15'
gem 'json',       '~> 1.5.4', :platforms => [ :ruby_18, :jruby ]
gem 'json_pure',  '~> 1.5.4', :platforms => [ :mswin ]

group :development do
  gem 'dm-validations', DM_VERSION,
    SOURCE  => "#{DATAMAPPER}/dm-validations#{REPO_POSTFIX}",
    :branch => CURRENT_BRANCH
end

group :test do
  gem 'nokogiri' #,    '~> 1.14.2'
  gem 'libxml-ruby' #, '~> 2.2.2', :platforms => [ :mri, :mswin ]
end

group :test do
  gem 'rspec'
end

group :datamapper do

  adapters = ENV['ADAPTER'] || ENV['ADAPTERS']
  adapters = adapters.to_s.tr(',', ' ').split.uniq - %w[ in_memory ]

  if (do_adapters = DM_DO_ADAPTERS & adapters).any?
    do_options = {}
    do_options[:git] = "#{DATAMAPPER}/do#{REPO_POSTFIX}" if ENV['DO_GIT'] == 'true'

    gem 'data_objects', DO_VERSION, do_options.dup

    do_adapters.each do |adapter|
      adapter = 'sqlite3' if adapter == 'sqlite'
      gem "do_#{adapter}", DO_VERSION, do_options.dup
    end

    gem 'dm-do-adapter', DM_VERSION,
      SOURCE  => "#{DATAMAPPER}/dm-do-adapter#{REPO_POSTFIX}",
      :branch => CURRENT_BRANCH
  end

  adapters.each do |adapter|
    gem "dm-#{adapter}-adapter", DM_VERSION,
      SOURCE  => "#{DATAMAPPER}/dm-#{adapter}-adapter#{REPO_POSTFIX}",
      :branch => CURRENT_BRANCH
  end

  plugins = ENV['PLUGINS'] || ENV['PLUGIN']
  plugins = plugins.to_s.tr(',', ' ').split.push('dm-migrations').uniq

  plugins.each do |plugin|
    gem plugin, DM_VERSION,
      SOURCE  => "#{DATAMAPPER}/#{plugin}#{REPO_POSTFIX}",
      :branch => CURRENT_BRANCH
  end

end
