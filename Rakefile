# encoding: UTF-8
$:.unshift("lib")

require 'xcode_build'
require 'xcode_build/output_translator'
require 'xcode_build/tasks/build_task'
require 'pp'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)

task :default => :spec

class OutputFormatter
  def beginning_translation_of_line(line)
    puts line
  end
  
  def build_started(params)
  end
  
  def build_succeeded
  end
  
  def build_failed
  end
end

class InspectorFormatter
  def build_started(params)
    pp({build_started: params})
  end
  
  def build_action(params)
    pp({build_action: params})
  end
  
  def build_error_detected(params)
    pp({build_error_detected: params})
  end
  
  def build_succeeded
    pp({build_succeeded: {}})
  end
  
  def build_failed
    pp({build_failed: {}})
  end
  
  def build_action_failed(params)
    pp({build_action_failed: params})
  end
end

namespace :examples do
  XcodeBuild::Tasks::BuildTask.new do |t|
    t.invoke_from_within = "resources/ExampleProject"
    t.formatter = XcodeBuild::Formatters::ProgressFormatter.new
  end
end

task :clean_example do
  Dir.chdir("resources/ExampleProject") do
    XcodeBuild.run("clean", File.open("/dev/null", "w"))
  end
end

task :build_example => :clean_example do
  Dir.chdir("resources/ExampleProject") do
    formatter = InspectorFormatter.new
    XcodeBuild.run("", XcodeBuild::OutputTranslator.new(formatter))
  end
end

task :build_example_and_fail => :clean_example do
  Dir.chdir("resources/ExampleProject") do
    formatter = InspectorFormatter.new
    XcodeBuild.run("-configuration AlwaysFails", XcodeBuild::OutputTranslator.new(formatter))
  end
end
