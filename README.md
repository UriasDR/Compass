# Compass

Requisitos do Projeto: 

# 1. Requisitos AWS
• 1.1 Gerar uma chave pública para acesso ao ambiente;
• 1.2 Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD);
• 1.3 Gerar 1 elastic IP e anexar à instância EC2;
• 1.4 Liberar as portas de comunicação para acesso público: (22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP).

# 2. Requisitos no linux:

• 2.1 Configurar o NFS entregue;
• 2.2 Criar um diretorio dentro do filesystem do NFS com seu nome;
• 2.3 Subir um apache no servidor - o apache deve estar online e rodando;
• 2.4 Criar um script que valide se o serviço esta online e envie o resultado da validação para o seu diretorio no nfs;
• 2.5 O script deve conter - Data HORA + nome do serviço + Status + mensagem personalizada de ONLINE ou offline;
• 2.6 O script deve gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço OFFLINE;
• 2.7 Preparar a execução automatizada do script a cada 5 minutos.
• 2.8 Fazer o versionamento da atividade;
• 2.9 Fazer a documentação explicando o processo de instalação do Linux.

# 3. Como foi executado?

1.1 Dentro do Linux, foi gerada uma chave SSH com o comando ssh-keygen -t rsa -b 2048, após usar o comando cat no arquivo da chave pública, é possível copiá-lo e fazer uma importação de chave dentro da AWS, assim linkando as 2 chaves.
1.2 Para criar a instância, basta seguir o passo a passo da criação na AWS, e se atentar para mudar as tags e o tipo de Linux que está sendo criado na instância.
1.3 Ao acessar a parte de Elastic IP, geramos um novo endereço e associamos à nossa instância ec2, ele é fundamental para o acesso à instancia e à página web do Apache.
1.4 Para liberar as portas, foi criado um novo Security Group dentro da AWS, e através das rotas da tabela de rotas, podemos abrir os acessos para as portas e posteriormente associa-las ao que for necessário dentro da parte de VPC.

2.1 Utilizando o EFS da AWS, primeiro precisamos configura-lo dentro da mesma, através do console, é bem simples, basta seguir o manual, disponível em: https://aws.amazon.com/pt/getting-started/tutorials/create-network-file-system/?pg=ln&sec=hs.
2.2 Após instalado e montado o EFS, é possível através de vários métodos criar um diretório com o nome, dentre eles, criar manualmente toda vez que reiniciar a instância, criar na hora da execução do script, ou configurar para ser salvo no /etc/fstab, optei pela última opção.
2.3 Para subir o apache, basta executar os comandos presentes no arquivo de comandos do Linux, é fundamental que a porta 80 esteja aberta para poder acessar o http.
2.4,2.5,2.6 O script foi criado usando o vim, o arquivo /home/ec2-user/Scripts/scpAP.sh é o executável. É possível saber se o script estava online ou offline acessando o diretório "Urias" presentem em /home/ec2-user/efs-mount-point/Urias , dentro desse diretório podem ser encontradas um ou dois diretórios, nomeadas de Online e Offline, caso o script nunca rode enquanto o serviço do Apache estiver offline, o diretório "Offline" não irá estar presente. Dentro dos diretórios, constam arquivos nomeadas por "Status_Service_$data_$hora.txt" esses arquivos contém a data, a hora, e o nome do serviço(no caso o Apache) que estava Online/Offline.
2.7 A execução cronometrada do arquivo fica de responsabilidade do crontab, através do comando crontab -e como consta no arquivo de comandos do Linux é executada a cada 5 minutos, à partir do momento que a instância EC2 fica ativa.
2.8 O versionamento foi feito através do GitHub.
2.9 A documentação está presente no arquivo "Instalando o Linux".
