require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'
require 'puppet-syntax/tasks/puppet-syntax'
require 'metadata-json-lint/rake_task'

# require 'puppet-strings/rake_tasks'
# require 'rubocop/rake_task'
# RuboCop::RakeTask.new

PuppetLint.configuration.log_format = '%{path}:%{linenumber}:%{check}:%{KIND}:%{message}'
PuppetLint.configuration.fail_on_warnings = true
PuppetLint.configuration.send('relative')
PuppetLint.configuration.send('disable_140chars')
PuppetLint.configuration.send('disable_class_inherits_from_params_class')
PuppetLint.configuration.send('disable_documentation')
PuppetLint.configuration.send('disable_single_quote_string_with_variables')
PuppetSyntax.fail_on_deprecation_notices = false

exclude_paths = %w(
  pkg/**/*
  vendor/**/*
  spec/**/*
)
PuppetLint.configuration.ignore_paths = exclude_paths
PuppetSyntax.exclude_paths = exclude_paths

desc 'Run acceptance tests'
RSpec::Core::RakeTask.new(:acceptance) do |t|
  t.pattern = 'spec/acceptance'
end

desc 'Run metadata_lint, lint, syntax, and spec tests.'
task test: [
  :metadata_lint,
  :lint,
  :syntax,
  :spec,
]

desc 'Generate the Stonith modules'
task :generate_stonith do
  sh './agent_generator/generate_manifests.sh'
end

ENV['BEAKER_debug'] = 'yes'
ENV['BEAKER_set'] = 'vagrant-ubuntu-14.04-64' unless ENV['BEAKER_set']
ENV['BEAKER_destroy'] = 'onpass' unless ENV['BEAKER_destroy']
ENV['BEAKER_provision'] = 'yes' unless ENV['BEAKER_provision']
