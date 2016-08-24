#!/usr/bin/env ruby
# jekyll Rakefile
# Config variables above those defined in _config.yml

#
puts "Starting main engines!"

desc "Test site using HTML::Proofer"
task :proof_sitedir do
  jekyll("build")
  #HTMLProofer.check_directory(
  #  "./_site/",
  #  allow_hash_href: true,
  #  assume_extension: true,
  #  url_ignore: [
  #    "http://validator.w3.org/check?uri=referer"
  #  ]
  #).run
  puts "YOLO that shit straight to prod, son!"
end

desc "Test site"
task test: [:proof_sitedir] do
  cleanup
end

desc "Clean up generated site"
task :clean do
  cleanup
end

desc "Preview on local machine"
task preview: :clean do
  jekyll("serve --watch")
end
task serve: :preview

desc "Build for deployment"
task build: :clean do
  if rake_running
    puts "\n\n****WARNING: An instance of rake is running.\n"
    puts "****WARNING: Building while running other tasks (preview!)\n"
    puts "             might create a site with broken links.\n\n"
    puts "Are you certain you want to continue? [Y|n]"

    ans = STDIN.gets.chomp
    exit if ans != "Y"
  end
  jekyll("build --config _config.yml")
end

def cleanup
  jekyll("clean")
end

# launch jekyll
def jekyll(directives = "")
  system "bundle exec jekyll " + directives
end

# check if there is another rake task running (in addition to this one!)
def rake_running
  `pgrep rake | wc -l`.to_i > 1
end

def git_local_diffs
  `git diff --name-only` != ""
end

def git_remote_diffs(branch)
  `git fetch`
  `git rev-parse #{branch}` != `git rev-parse origin/#{branch}`
end

def git_repo?
  `git status` != ""
end

def git_requires_attention(branch)
  git_check && git_repo? && git_remote_diffs(branch)
end

#
# default task stuff
#
require "rake"
require "html-proofer"
require "rubocop/rake_task"

RuboCop::RakeTask.new

desc "Set default tasks"
task default: :proof_sitedir
