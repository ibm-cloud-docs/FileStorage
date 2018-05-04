---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.filestorage_short}} para backup com o Plesk

O objetivo deste artigo é fornecer instruções para configurar o
{{site.data.keyword.blockstoragefull}} para seus backups no Plesk. A suposição é que o SSH
raiz ou sudo e o acesso Plesk no nível de administrador integral estejam disponíveis. Esse exemplo se baseia
em um host do CentOS7.

**Nota**: é possível localizar a documentação do Plesk para backup e restauração
[aqui](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   **Nota**: o Plesk tem duas opções para armazenamento de backups. Uma é o armazenamento do
Plesk interno, que é um armazenamento de backup localizado em seu servidor Plesk, e a outra é um armazenamento
FTP externo, que é um armazenamento de backup localizado em um servidor externo na web ou na sua
rede local. Geralmente em caixas do Plesk, os backups internos são armazenados em
`/var/lib/psa/dumps` e usam `/tmp` como um diretório temporário. Em nosso
exemplo, mantemos o diretório temporário local, mas movemos o diretório de dump para o destino STaaS
(`/backup/psa/dumps`). Nenhuma credencial do usuário FTP é necessária.
   
3. Configure seu {{site.data.keyword.filestorage_short}} conforme descrito em
[Acessando o {{site.data.keyword.filestorage_short}} no
Red Hat Enterprise Linux](accessing-file-storage-linux.html) e em [Montando o
NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html). Certifique-se de montá-lo no
`/backup` e de configurá-lo em `/etc/fstab` para ativar a montagem na
inicialização. <br />
   **Nota**: por padrão, o NFS fará downgrade de quaisquer arquivos criados com as
permissões raiz para o usuário nobody. Para permitir que os clientes raiz retenham as permissões raiz no
compartilhamento do NFS, o `no_root_squash` deve ser incluído em
`/etc/exports`. <br />
   **Nota**: também há instruções disponíveis para
[Montando o NFS/{{site.data.keyword.filestorage_short}} no
CoreOS](mounting-storage-coreos.html). <br />

4. **Opcional**: copie backups existentes para o novo armazenamento. Use
`rsync`, por exemplo:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Nota**: esse comando transmite seus dados compactados ao mesmo tempo em que os preserva o máximo
possível (exceto links físicos) e fornece informações sobre quais arquivos estão sendo transferidos, além de um
breve resumo no final.
    
5. Edite `/etc/psa/psa.conf` para apontar o valor de `DUMP_D`
para o novo destino. 
    -  Deve aparecer como: `DUMP_D /backup/psa/dumps`. 

6. **Opcional**: conforme determinado pelas suas necessidades de
negócios e pelo seu caso de uso específicos, remova o armazenamento antigo do servidor e cancele-o da conta.

