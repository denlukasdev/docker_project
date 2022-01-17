		Database changes
core_config_data table:
    - set base url with port (http://test.loc::8088/)
    - for elasticsearch, in 'path' column find %elasticsearch%, set elasticsearch_container_name
            


		PhpStorm + PHP  + Docker + Xdebug  Settings
- docker-compose file at php container add:
		...
		environment:
      	PHP_IDE_CONFIG: serverName=Docker
    	extra_hosts:
     		 - "host.docker.internal:host-gateway"
      		...
      		
- xdebug.ini file:
	[xdebug]
	zend_extension=xdebug.so
	xdebug.mode=debug
	xdebug.start_with_request=yes
	xdebug.client_host=host.docker.internal	
	
- PhpStorm Settings->PHP->Servers:
	Name: Docker
	Host: _
	Port: 80
	Debugger: Xdebug
	Use path mappings: set 'chek', compare project directory and project docker directory
	/var/www/html/server
	
- PhpStorm Settings->PHP->CLI Interpreter->... :
	+ -> From Docker -> Docker Compose -> Server: Docker -> 
	-> Configuration files: path_to_docker-compose.yml -> Service: php_docker_name
- PhpStorm Settings->PHP->Debug:
	Debug port: 9003


