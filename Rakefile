task :deploy do
  sh "hugo"
  sh "cp public/index.xml public/atom.xml"
  sh "rsync -avz public/ blog.eiel.info:www/blog/"
end
