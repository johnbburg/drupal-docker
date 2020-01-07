Copy webroot to services/drupal/docroot

Move vm.env to .env

Edit settings.php to use the following for a database. 
user: web
pass: web
db: web
host: mysql

Run 

$ f1 build
$ f1 up

Install drush:

$ cd services/drupal
$ composer install

To import a DB

$ zcat ~/path/to/dump.sql.gz | f1 drush sqlc


