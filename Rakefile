require "bundler/gem_tasks"
require "rake/testtask"

Rake::TestTask.new(:test) do |t|
  t.libs << "test"
  t.libs << "lib"
  t.test_files = FileList['test/**/*_test.rb']
end

task :default => :test

def pad(array)
  max = []
  array.each do |row|
    i = 0
    row.each do |col|
      max[i] = [max[i] || 0, col.length].max
      i += 1
    end
  end

  array.each do |row|
    i = 0
    row.each do |col|
      col << " " * (max[i] - col.length)
      i += 1
    end
  end

end

desc "generate mime type database"
task :rebuild_db do
  puts "Generating mime type DB"
  require 'mime/types'
  index = {}

  MIME::Types.each do |type|
    type.extensions.each {|ext| (index[ext] ||= []) << type}
  end

  index.each do |k,list|
    list.sort!{|a,b| a.priority_compare(b)}
  end

  buffer = []

  index.each do |ext, list|
    first = list.first
    buffer << [ext.dup, first.content_type.dup, first.encoding.dup]
  end

  buffer.sort!{|a,b| a[0] <=> b[0]}

  pad(buffer)

  File.open("lib/db/mime.db", File::CREAT|File::TRUNC|File::RDWR) do |f|
    buffer.each do |row|
      f.write "#{row[0]} #{row[1]} #{row[2]}\n"
    end
  end

  puts "#{buffer.count} rows written to lib/db/mime.db"

end