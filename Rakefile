require 'rubygems'
require 'tmpdir'
require 'bundler/setup'
require "jekyll"
require 'yaml'






# Indiquez le nom de votre dépôt
GITHUB_REPONAME = "stephDocs/jeedomOrvibo"

namespace :site do







   

  desc "préparation dossier de travail"
  task :prepare do
    rm_rf('run')
    cp_r "publish", "run"
    cp_r "doc/fr_FR/.", "run"
    cp_r "doc/images/.", "run/images"

  end


  desc "build "
  task :build => [:prepare] do
    config = Jekyll.configuration({ })
    site = Jekyll::Site.new(config)
    Jekyll::Commands::Build.build site, config
  end


   

  


  desc "Génération et publication des fichiers sur GitHub"
  task :publish => [:build]  do
   
     
  
 
    cp_r "doc/images/.", "_site/images"

    Dir.mktmpdir do |tmp|
      cp_r "_site/.", tmp
      
      pwd = Dir.pwd
      Dir.chdir tmp
      File.open(".nojekyll", "wb") { |f| f.puts("Site généré localement.") }
      
      system "git init"
      system "git add ."
      message = "Site mis à jour le #{Time.now.utc}"
      system "git commit -m #{message.inspect}"
      system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
      system "git push origin master:refs/heads/gh-pages --force"

      Dir.chdir pwd
    end
  end
end
