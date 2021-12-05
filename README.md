# Roteiro de instalação LAMP

1. Adicione no arquivo de repositorio o backports para instalar o phpmyadmin posteriormente: deb http://www.deb.debian.org/debian/ buster-backports.main
2. Rode os comandos apt update && apt upgrade -y para atualizar o sistema com a linha recem adicionada
3. Instalar o apache2: apt install apache2
4. Instalar o mysql server 8 mas para isso precisa ser instalado alguns pacotes com antecedencia: apt install wget lsb-release gnupg -y
5. Após instalar faça o download do arquivo do repositorio do mysql: wget -c https://repo.mysql.com/mysql-apt-config_0.8.15-1_all.deb
6. Para instalar execute o seguinte comando: dpkg -i mysql-apt-config_0.8.15-1_all.deb
7. Na tela que aparecer de um ok e será instalado
8. Execute um apt update para atualizar o repositorio e apt upgrade -y
9. Clique em ok novamente em todas as telas que sejam exibidas
10. Após todos esses processos, voce poderá instalar o mysql server com o comando: apt install mysql-server -y
11. Ele irá solicitar uma senha para o root, digite a de sua preferencia e selecione a opção para usar encriptação de senha forte e pressione enter
12. Aguarde até a instalação ser concluida
13. Feita a instalação, será necessário realizar uma configuração na senha do root
14. Para isso digite: mysql -u root -p, digite a senha do root e pressione enter
15. Após isso você estará no terminal do mysql, lembre-se que ele é case sensitive, então digite: ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '1234'; (esse ultimo campo é a senha de sua preferência)
16. Pressione enter e a senha será alterada, para que ela comece a valer precisa atualizar o cadastro do mysql, para isso digite: FLUSH PRIVILEGES;
17. Em seguida bastar sair com o comando exit
18. Agora vamos instalar o php: apt install php7.0 php7.3-mysql -y
19. Feito isso vamos criar uma página de teste: nano /var/www/html/teste.php
20. Nele insira o seguinte comando: <?php phpinfo (); ?> salve e saia
21. Agora vamos instalar o phpmyadmin e alguns modulos: apt install -t buster-backports phpmyadmin php-mbstring php-zip php-gd php-json php-curl
22. Durante a instalação quando aparecer uma janela selecione com a barra de espaço o apache2 como servidor, pressione tab e dê enter
23. Quando ele pedir para configurar o banco de dados do phpmyadmin como dbconfig-common voce clique em sim e coloque a senha do root
24. Responda ok na proxima janela e na subsequente coloque ignorar.
25. Após o phpmyadmin vamos instalar o wordpress
26. Para instalar execute as seguintes instruções:
27. Primeiro remova o arquivo padrão de index.html : cd /var/www/html/
28. Após entrar no diretorio digite rm index.html e cd ..
29. Agora vamos fazer o download do wordpress: wget http://wordpress.org/latest.tar.gz
30. Após concluir o download execute o comando: tar -xzvf latest.tar.gz
31. Quando terminar de extrair copie executando a sincronização com o espaço html: rsync -avP wordpress/ /var/www/html
32. Vamos entrar no diretorio html que agora possui o app do wordpress: cd html
33. Em seguida vamos conceder permissão de escrita para o apache nesses arquivos
34. Digite chown -R www-data:www-data /var/www/html
35. Após isso o usuário e o grupo ao qual esses arquivos pertencem será o APACHE
37. Agora vamos trazer a máquina para a rede interna, digite: nano /etc/network/interfaces
38. Comente a linha da interface dhcp e adicione:
```
iface enp0s3 inet static
netmask 255.255.255.0
network 172.16.0.0
broadcast 172.16.0.255
address 172.16.0.4
```
32. Salve e saia
33. Agora execute os comandos ifdown enp0s3 e ifup enp0s3 e pronto a maquina estará com ip fixo e funcionando na rede local, basta acessar a máquina windows e inserir o ip da máquina debian que ele irá exibir que o apache está instalado corretamente, e agora que possui o wordpress ela irá levar para a configuração do aplicativo
34. Para verificar se o php esta instalado corretamente basta pesquisar na maquina local: 172.16.0.4/teste.php que ele irá executar o script que criamos exibindo as informações do php
35. E para verificar o phpmyadmin basta acessar: 172.16.0.4/phpmyadmin

36. Dessa forma o LAMP estará instalado.


## Apache instalado:
![image](https://user-images.githubusercontent.com/54211710/144758715-aa5e4348-29b6-469e-b6bb-c26dd8bc0a20.png)

## PHP Instalado:
![image](https://user-images.githubusercontent.com/54211710/144758773-4c2828ed-a32e-4269-963b-85edb21126e4.png)

## PHP Myadmin Instalado:
![image](https://user-images.githubusercontent.com/54211710/144758802-1d1a0316-3c8e-4656-a2ca-7dc99a4be701.png)

## Wordpress Instalado:
![image](https://user-images.githubusercontent.com/54211710/144759474-aae2e70d-0df9-451d-a3a1-fe0d599a6897.png)
