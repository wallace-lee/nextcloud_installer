1.run in command prompt: "[folder_contain_docker_compose.yml]:\> docker compose up -d"
2. copy default.conf into .\nginx_data, and restart nginx-proxy container.
   $ docker stop nginx-proxy
   $ docker start nginx-proxy

3. open browser, and logon to nextcloud using this url:
    http://localhost:8081
4. create admin account in nextcloud portal
5. enable https via the step below:

  docker exec -it -u www-data nextcloud php occ config:system:set trusted_domains 0 --value=localhost
  docker exec -it -u www-data nextcloud php occ config:system:set trusted_domains 1 --value=172.*.*.*
  docker exec -it -u www-data nextcloud php occ config:system:set trusted_domains 2 --value=nextcloud.*
  docker exec -it -u www-data nextcloud php occ config:system:set trusted_domains 3 --value=127.0.0.1
  docker exec -it -u www-data nextcloud php occ config:system:set trusted_domains 4 --value=192.*.*.*
  docker exec -it -u www-data nextcloud php occ config:system:set overwriteprotocol --value=https
  docker exec -it -u www-data nextcloud php occ config:system:set overwrite.cli.url --value=https://localhost

6. now, relaunch nextcloud in browser using this url:
  https://localhost
  
7. to stop, run in command prompt:
  [folder_contain_docker_compose.yml]:\> docker compose stop"
8. to rerun, do:
[folder_contain_docker_compose.yml]:\> docker compose start"

9. if static ip is used for the host, make sure the ip is in the 192.168.x.x subnet

10. create scheduled task:
SCHTASKS /Create /S %computername% /RU SYSTEM /TN nextcloud /TR "docker exec -u www-data nextcloud php /var/www/html/cron.php" /SC MINUTE /RL highest