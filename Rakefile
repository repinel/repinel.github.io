task default: %w[test]

task :test do
  unless system('bundle exec jekyll build')
    abort 'ERROR: Failed to build _site with Jekyll.'
  end

  require 'html/proofer'
  HTML::Proofer.new('./_site', {
    typhoeus: {
      headers: { 'User-Agent' => 'Mozilla/5.0 (compatible; My New User-Agent)' }
    },
    external_only: true,
    check_favicon: true,
    check_html: true
  }).run
end
