require "html/proofer"
require 'rspec/core/rake_task'

desc "Run specs"
RSpec::Core::RakeTask.new do |t|
  t.pattern = 'spec/**/*_spec.rb'
  t.rspec_opts = ["--order", "rand", "--color"]
end

task :test do
  sh "bundle exec jekyll build --trace"
  Rake::Task["spec"].invoke
  HTML::Proofer.new("./_site", :check_html => true,
                    :href_swap => { %r{http://choosealicense.com} => "" }).run
end

task :approved_licenses do
  require './spec/spec_helper'
  approved = approved_licenses
  approved.select! { |l| spdx_ids.include?(l) }
  puts "#{approved.count} approved licenses:"
  puts approved.join(", ")
  puts "\n"

  potential = approved - licenses.map { |l| l["id"] }
  puts "#{potential.count} potential additions:"
  puts potential.join(", ")
end
