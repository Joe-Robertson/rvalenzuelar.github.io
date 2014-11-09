require "tmpdir"

desc "Publish master site"
task :publish do

  puts "\n## Building _site files"
  status = system("jekyll build")
  puts status ? "Success" : "Failed"

  puts "\n## Staging modified files"
  status = system("git add -A")
  puts status ? "Success" : "Failed"
  
  puts "\n## Committing new source build at #{Time.now.utc}"
  message = "Source build at #{Time.now.utc}"
  status = system("git commit -m \"#{message}\"")
  puts status ? "Success" : "Failed"

  puts "\n## Pushing commits to remote source"
  status = system("git push")
  puts status ? "Success" : "Failed"

  puts "\n## Switching to master"
  status = system("git checkout master")
  puts status ? "Success" : "Failed"

  puts "\n## Cleaning local master"  
  status = system("git rm -rf .")
  puts status ? "Success" : "Failed"

  puts "\n## Staging changes in master"  
  status = system("git add -A")
  puts status ? "Success" : "Failed"

  puts "\n## Commiting changes in master"
  message = "Cleaned local master"  
  status = system("git commit -m \"#{message}\"")
  puts status ? "Success" : "Failed"

  puts "\n## Switching to source"
  status = system("git checkout source")
  puts status ? "Success" : "Failed"

  Dir.mktmpdir do |tmp|
    puts "\n## Copying source _site contents to tmp folder"
    status = system("cp -r _site/* #{tmp}")
    puts status ? "Success" : "Failed"

    puts "\n## Switching to master"
    status = system("git checkout master")
    puts status ? "Success" : "Failed"

    puts "\n## Moving contents in tmp folder to master"
    status = system("mv #{tmp}/* .")
    puts status ? "Success" : "Failed"
  end

  puts "\n## Adding master branch changes"
  status = system("git add -A")
  puts status ? "Success" : "Failed"

  puts "\n## Committing master site at #{Time.now.utc}"
  message = "Build master site at #{Time.now.utc}"
  status = system("git commit -m \"#{message}\"")
  puts status ? "Success" : "Failed"

  puts "\n## Pushing commits to remote master"
  status = system("git push origin master")
  puts status ? "Success" : "Failed"

  puts "\n## Switching back to source"
  status = system("git checkout source")
  puts status ? "Success" : "Failed"
end