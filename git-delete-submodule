#!/usr/bin/env ruby

require "fileutils"

USAGE =<<EOS
Usage:
  $ git delete-submodule <path-to-submodule>

This command will delete the files of a submodule and remove it from git completely. It is equivalent to
performing the following three steps:
  * Deleting the lines about the submodule from .gitmodules
  * Deleting the lines about the submodule from .git/config
  * git rm --cached path-to-submodule (from the root of the repo)
  * rm -rf path-to-submodule

Note that you will probably want to commit this change afterwards.

EOS

abort USAGE unless (["-h", "--help"] & ARGV).empty?
abort USAGE if ARGV.empty?

submodule_path = ARGV[0]
abort "ARGV[0] does not exist." unless File.exists? submodule_path
absolute_path = File.absolute_path(submodule_path)

def abort_on_failure(cmd)
  result = `#{cmd}`
  abort unless $?.to_i.zero?
  result
end

git_root = abort_on_failure("git rev-parse --show-toplevel").strip
FileUtils.cd git_root
submodule_path = absolute_path.gsub("#{git_root}/", "").gsub(/\/$/, "")

raw_submodule_status_lines = abort_on_failure("git submodule status").split "\n"

valid_submodule = raw_submodule_status_lines.any? do |line|
  matches = line.match /^.[0-9a-f]{40} (\S+)(\s|$)/
  matches && submodule_path == matches[1]
end

abort "#{submodule_path} does not appear to be a git submodule." unless valid_submodule

puts "Are you sure you want to delete the git submodule located at #{submodule_path}?"
print "This will delete the files. Type 'yes' to continue: "
unless STDIN.gets.chomp == "yes"
  puts "Aborting."
  exit
end

puts "Removing the appropriate section from .gitmodules..."
`git config -f .gitmodules --remove-section submodule.#{submodule_path} 2> /dev/null`
unless $?.to_i.zero?
  abort "There was a problem removing the submodule.#{submodule_path} section from .gitmodules. Aborting."
end

puts "Removing the appropriate section from .git/config..."
`git config -f .git/config --remove-section submodule.#{submodule_path} 2> /dev/null`
# Don't need to check this one -- it might not exist if you haven't run git submodule init yet.

metadata_directory = `cd #{submodule_path} && git rev-parse --git-dir`.strip
unless metadata_directory.start_with? "/"
  metadata_directory = File.join(submodule_path, metadata_directory)
end

puts "Deleting the submodule from the git cache..."
`git rm --cached #{submodule_path}`
unless $?.to_i.zero?
  abort "There was an error running 'git rm --cached #{submodule_path}'. Aborting."
end

# Need to explicitly remove the metadata dir for 1.7.8+ compatibility
puts "Removing the metadata directory..."
unless File.directory? metadata_directory
  abort "The submodule's metadata directory was reported to be #{metadata_directory}, but it does not exist."
end
FileUtils.rm_rf metadata_directory

FileUtils.rm_rf submodule_path

puts "Successfully deleted submodule #{submodule_path}. You should probably commit this change now."
