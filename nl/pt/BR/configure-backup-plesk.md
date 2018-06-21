---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.filestorage_short}} para backup com o Plesk

Neste artigo, o objetivo é fornecer instruções para configurar o {{site.data.keyword.blockstoragefull}} para seus backups no Plesk. A suposição é que o SSH
raiz ou sudo e o acesso Plesk no nível de administrador integral estejam disponíveis. Esse exemplo se baseia
em um host do CentOS7.

**Nota**: é possível localizar a documentação do Plesk para backup e restauração
[aqui](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   **Nota**: o Plesk tem duas opções para armazenar backups. Uma é o armazenamento Plesk interno - um armazenamento de backup em seu servidor Plesk. A outra é um armazenamento FTP externo - um armazenamento de backup em algum servidor externo na web ou na sua rede local. Geralmente em caixas do Plesk, os backups internos são armazenados em
`/var/lib/psa/dumps` e usam `/tmp` como um diretório temporário. Em nosso exemplo, mantemos o local do diretório temporário, mas movemos o diretório de dump para o destino STaaS (`/backup/psa/dumps`). Nenhuma credencial do usuário FTP é necessária.
   
3. Configure seu {{site.data.keyword.filestorage_short}} conforme descrito em
[Acessando o {{site.data.keyword.filestorage_short}} no
Red Hat Enterprise Linux](accessing-file-storage-linux.html) e em [Montando o
NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html). Monte o volume em /backup e configure-o na tabela de sistemas de arquivos /etc/fstab para ativar a montagem na inicialização. <br />
   **Nota**: por padrão, NFS faz downgrade de quaisquer arquivos que foram criados com as permissões raiz para o usuário nobody. Para permitir que os clientes raiz retenham as permissões raiz no compartilhamento do NFS, `no_root_squash` precisa ser incluído em `/etc/exports`. <br />
   **Nota**: também há instruções disponíveis para
[Montando o NFS/{{site.data.keyword.filestorage_short}} no
CoreOS](mounting-storage-coreos.html). <br />

4. **Opcional**: copie backups existentes para o novo armazenamento. Use
`rsync`, por exemplo:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}
    
    **Nota**: esse comando transmite seus dados compactados enquanto preserva o máximo possível, exceto links físicos. Ele também fornece informações sobre quais arquivos estão sendo transferidos e um breve resumo no final.
    
5. Edite `/etc/psa/psa.conf` para apontar o valor de `DUMP_D`
para o novo destino. 
    - Deve aparecer como: `DUMP_D /backup/psa/dumps`. 

6. **Opcional**: conforme determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele-o da conta.

