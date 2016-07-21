# ¿Cómo instalar y configurar phpMyAdmin en CentOS 6?
*Actualizado el 20 de Julio, 2016. Por OpenCloud.*

>[phpMyAdmin](https://www.phpmyadmin.net/ "phpMyAdmin") es una aplicación Web que provee una interfaz gráfica de usuario que permite ayudar en la administración de bases de datos en MySQL. Es compatible con varios servidores MySQL y constituye una alternativa sencilla y robusta para usar el cliente de línea de comandos MySQL.

***Nota***  
Esta guía está escrita para un usuario sin permisos *root*. Los comandos que requieran privilegios elevados están precedidos por ***sudo***. Recuerde que esta utilidad de sistemas basados en Unix, permite ejecutar comandos con privilegios *root* de manera segura.

## Antes de comenzar

1. Asegúrese de que ha configurado y asegurado su Servidor correctamente. El nombre del host o *hostname* también debe estar configurado.  
Para verificar su *hostname* puede ejecutar:
~~~
hostname  
hostname -f
~~~  
	El primer comando debería mostrar el *hostname* corto, y el segundo debería mostrar el nombre de dominio completamente calificado (o FQDN, por sus siglas en inglés).
2. Actualice su sistema:  
	```
	sudo yum update
	```  
3. Configure una infraestructura LAMP operativa. Puede consultar la [Guía de instalación de LAMP en CentOS](agregar_enlace "instalación_LAMP") si es necesario.  
***Nota***  
Si tiene instalado el paquete `php-suhosin`, hay algunos problemas conocidos al usar phpMyAdmin. Por favor visite la [página de problemas de compatibilidad entre phpMyAdmin y Suhosin](http://www.hardened-php.net/hphp/troubleshooting.html "phpMyAdmin Suhosin troubleshooting") para más información de ajustes y soluciones.
4. Habilite el repositorio EPEL:  
	```
	cd ~  
	wget http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm  
	sudo rpm -ivh epel-release*
	```
5. Configure Apache con SSL, de manera que sus contraseñas no sean enviadas sobre texto plano. Para hacerlo, debe seguir el tutorial de [Certificados SSL con Apache en CentOS](https://www.linode.com/docs/security/ssl/ssl-apache2-centos "SSL Apache CentOS Tutorial").
6. Instale el módulo PHP `mycrypt`:  
	```
	sudo yum install php-mcrypt
	```  
7. Reinicie Apache:  
	```
	sudo service httpd restart
	```

## Instalar phpMyAdmin

1. Instale phpMyAdmin ejecutando el siguiente comando:  
	```
	sudo yum install phpmyadmin
	```  
2. Para cada host virtual al que le gustaría dar acceso a la instalación de su phpMyAdmin, debe crear un enlace simbólico desde el documento root a la ubicación de la instalación de phpMyAdmin (`/usr/share/phpmyadmin`)  
	```
	cd /var/www/example.com/public_html  
	sudo ln -s /usr/share/phpmyadmin
	```  
	Esto creará un enlace simbólico llamado `phpmyadmin` en su documento root.

## Configurar phpMyAdmin

Por defecto, phpMyAdmin está configurado para solo permitir el acceso desde el localhost (`127.0.0.0.1`). Usted deberá agregar la dirección IP de su computador con el fin de poder acceder.

1. Tome nota de la dirección IP externa que está siendo usada por el computador de su trabajo u hogar. Puede visitar el siguiente sitio web para hacerlo: [What is my IP?](http://www.whatismyip.com "What is my IP website"). 
2. Edite la configuración del archivo ubicado en `/etc/httpd/conf.d/phpMyAdmin.conf`, remplazando las cuatro instancias de `127.0.0.1` con la dirección IP anotada en el paso anterior.

## Forzar SSL

Debido a que requiere ingresar las credenciales de MySQL cuando utilice phpMyAdmin, recomendamos que utilice SSL para asegurar el tráfico HTTP a su instalación de phpMyAdmin. Para más información sobre cómo usar SSL en sus sitios, por favor consulte el tutorial que aborda el tema de los [Certificados SSL](https://www.linode.com/docs/security/ssl// "certificados SSL").

1. Force a phpMyAdmin a que utilice SSL en el archivo de configuración de este último: `/etc/phpmyadmin/config.inc.php` añadiendo las siguientes líneas en la sección `Server(s) configuration`:  
Extracto del archivo **/etc/phpmyadmin/config.inc.php**  
	```
	$cfg['ForceSSL'] = 'true';
	```
2. Reinicie Apache  
	```
	sudo service httpd restart
	```
## Probar su instalación de phpMyAdmin

Para probar phpMyAdmin, abra su navegador preferido y diríjase a la siguiente dirección: https://example.com/phpmyadmin. Se le pedirá un nombre de usuario y contraseña. Puede utilizar el nombre de usuario "root" y la contraseña que especificó al instalar MySQL. De forma alternativa, puede ingresar usando cualquier usuario de MySQL y conservar sus permisos.

Si puede ingresar con éxito, phpMyAdmin ha sido instalado adecuadamente.

## Recursos adicionales

Puede consultar los siguientes recursos en busca de información adicional con respecto a este tema. Aunque este material es provisto esperando que sea útil, tome en cuenta que no podemos dar fe de la actualidad o precisión de los contenidos externos.  

* [Página principal de phpMyAdmin](https://www.phpmyadmin.net/ "phpMyAdmin")
* [Documentación oficial de phpMyAdmin](http://www.phpmyadmin.net/home_page/docs.php "Documentación phpMyAdmin")
