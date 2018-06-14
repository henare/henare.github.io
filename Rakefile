require 'html-proofer'
require 'rubocop/rake_task'

task default: %i[test rubocop]

task :test do
  sh 'bundle exec jekyll build'
  options = {
    assume_extension: true,
    disable_external: true,
    empty_alt_ignore: true,
    check_html: true,
    check_img_http: true,
    check_opengraph: true
  }
  HTMLProofer.check_directory('./_site', options).run
end

RuboCop::RakeTask.new
