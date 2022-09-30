		Start project
1) Run init_config to setup docker configuration
2) Copy project to server directory
3) Change env file
4) Run install_dump to setup database dump
5) Run init_start 

        Database changes
core_config_data table:
    - set base url with port (http://test.loc::8088/)
    - for elasticsearch, in 'path' column find %elasticsearch%, set elasticsearch_container_name

	For Xdebug 3+
- PhpStorm Settings->PHP->Servers:
	Name: Docker
	Host: _
	Port: 80
	Debugger: Xdebug
	Use path mappings: set 'check', compare project directory and project docker directory
	/var/www/html/server
	
- PhpStorm Settings->PHP->CLI Interpreter->... :
	+ -> From Docker -> Docker Compose -> Server: Docker -> 
	-> Configuration files: path_to_docker-compose.yml -> Service: php_docker_name
  - PhpStorm Settings->PHP->Debug:
      Debug port: 9003

	For also add Xdebug <3
- PhpStorm Settings->PHP->Debug->DBGp proxy:
	Ide key:PHPSTORM
	Host:Docker
	Port:9000

