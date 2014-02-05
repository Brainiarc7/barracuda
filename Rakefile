require 'rubygems'
require 'rubygems/package_task'
require 'rake/testtask'

WINDOWS = (PLATFORM =~ /win32|cygwin/ ? true : false) rescue false
SUDO = WINDOWS ? '' : 'sudo'

task :default => :test
task :test => :build

desc "Make the extension makefile"
task :makefile do
  sh "cd ext && ruby extconf.rb"
end

Rake::TestTask.new

load 'barracuda.gemspec'
Gem::PackageTask.new(SPEC) do |pkg|
  pkg.gem_spec = SPEC
  pkg.need_zip = true
  pkg.need_tar = true
end

desc "Install the gem locally"
task :install => :package do 
  sh "#{SUDO} gem install pkg/#{SPEC.name}-#{SPEC.version}.gem --local"
  sh "rm -rf pkg/#{SPEC.name}-#{SPEC.version}" unless ENV['KEEP_FILES']
end

desc 'Build Barracuda'
task :build => :makefile do
  sh "cd ext && make"
end
