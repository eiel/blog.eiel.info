task :deploy do
  sh "hugo"
  sh "rsync -avz public/ blog.eiel.info:www/blog/"
end
