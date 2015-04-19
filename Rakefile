task :default => [:deploy]

task :build => [:build_raw, :minify, :gzip]

task :deploy => :build do
  sh "s3cmd sync --no-preserve --cf-invalidate --delete-removed --reduced-redundancy --verbose --add-header='Cache-Control: max-age=86400, public' --add-header='Content-Encoding: gzip' _deploy/ s3://chrisdown.name"
end

task :gzip => :build_raw do
  sh 'find _deploy/ -type f -exec gzip -n -9 {} \; -exec mv {}.gz {} \;'
end

task :minify => :build_raw do
  sh 'find _deploy/ -type f -name "*.html" -exec sh -c "htmlmin -c -s > /tmp/q < \"\$0\"" {} \; -exec mv /tmp/q {} \;'
end

task :build_raw do
  sh "jekyll build"
end
