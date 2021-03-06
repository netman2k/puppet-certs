# This file is managed centrally by modulesync
#   https://github.com/Katello/foreman-installer-modulesync

require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'

# blacksmith is broken with ruby 1.8.7
if Gem::Version.new(RUBY_VERSION) > Gem::Version.new('1.8.7')
  # blacksmith isn't always present, e.g. on Travis with --without development
  begin
    require 'puppet_blacksmith/rake_tasks'
    Blacksmith::RakeTask.new do |t|
      t.tag_pattern = "%s"
    end
  rescue LoadError
  end
end

PuppetLint.configuration.ignore_paths = ["spec/**/*.pp", "pkg/**/*.pp", "vendor/**/*.pp"]
PuppetLint.configuration.log_format = '%{path}:%{linenumber}:%{KIND}: %{message}'

# Used for type alias tests
PuppetSyntax.exclude_paths << 'spec/static_fixtures/test_module/**/*.pp' if Puppet.version.to_f < 4.0

require 'puppet-lint-param-docs/tasks'
PuppetLintParamDocs.define_selective do |config|
  config.pattern = ["manifests/foreman_proxy_content.pp", "manifests/init.pp"]
end

task :default => [:validate, :lint, :spec]
