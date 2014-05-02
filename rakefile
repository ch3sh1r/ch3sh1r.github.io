require 'rubygems'
require 'rake'
require 'fileutils'

desc "Begin a new post"
task :post do
  puts "What should we call this post for now?"
  name = STDIN.gets.chomp
  filename = File.join('_posts/', "#{Time.now.strftime('%Y-%m-%d')}-#{name}.md")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{name.gsub(/-/,' ')}\""
    post.puts "abstract: \"\""
    post.puts "tags: []"
    post.puts "---"
    post.puts ""
  end
end

desc "Create a new page"
task :page do
  name = ENV["name"] || "new-page.md"
  filename = File.join(".", "#{name}")
  filename = File.join(filename, "index.html") if File.extname(filename) == ""
  title = File.basename(filename, File.extname(filename)).gsub(/[\W\_]/, " ").gsub(/\b\w/){$&.upcase}
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  
  mkdir_p File.dirname(filename)
  puts "Creating new page: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: page"
    post.puts "title: \"#{title}\""
    post.puts "---"
    post.puts "{% include setup %}"
  end
end

desc "Startup Jekyll"
task :start do
  sh "rm -rf _site"
  sh "jekyll --server"
end

task :default => :start
