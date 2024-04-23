# Exemplo Docker SFTP

Este projeto foi realizado como trabalho de Servidores e Redes 2, no IFSP.  
Feito com o intuito de apresentar a comunicação e segurança oferecida quando se utiliza um Docker para hospedar imagens de Filebrowser e SFTP, bem como o isolamento do serviço SFTP em uma rede virtual criada pelo próprio hóspede do Docker.  
  
Desta forma, o volume de arquivos utilizado pelo SFTP ficará virtualmente seguro dentro de uma VLAN criada pelo Docker, onde o container é executado, e inacessível por outros meios além da rede isolada onde a imagem de Filebrowser é executada, garantindo segurança do volume de arquivos sem muita complexidade de configurações de acesso em firewall.  
  
### Pre-requisitos (Windows)
- Tenha instalado a versão mais recente do [Docker Desktop (para Windows)](https://www.docker.com/products/docker-desktop/) no site oficial.  
- Faça download do arquivo [docker-compose.yml](docker-compose.yml) que será utilizado para indicar quais imagens públicas obter (SFTP e Filebrowser), bem como as configurações para criação de rede interna, volume e redirecionamento de portas para acesso ao Filebrowser

### Passo a passo
Inicie o Docker Desktop e em seguida, por linha de comando, acesse a pasta onde o docker-compose.yml está instalado e execute os seguintes comandos para download das imagens e começar o processo de hospedar os containers das mesmas, bem como a criação de um volume.
```
docker-compose config
docker-compose up -d
```
Após o download das imagens e inicialização das mesmas, poderá verificar os status do container e volume dentro do Docker Windows.  

### Conclusão
O Docker está criando uma rede virtual para seus containers chamada internal_net, cujo sem um redirecionamento de portas do host, é impossível que a máquina tenha acesso às portas de serviço dos containers no ar, porém internamente na rede, a aplicação do Filebrowser se conecta ao serviço SFTP do mesmo container pela porta 22 corretamente, enquanto expõe-se pela porta 80. Como indicado no docker-compose, a porta 80 do container é redirecionada usando a 8080 do host, logo, a aplicação do Filebrowser é acessível por http://localhost:8080    

Realize o acesso usando usuario e senha "admin" padrão da imagem escolhida no docker-compose, e já poderá fazer uso do SFTP utilizando o volume virtual, seguro dentro da rede virtualizada pelo Docker, onde os arquivos serão acessíveis somente via portal do Filebrowser.
