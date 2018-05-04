---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.filestorage_short}} para backup com o cPanel

O objetivo deste artigo é fornecer instruções para configurar seus backups para que sejam armazenados
no {{site.data.keyword.filestorage_full}} por meio do cPanel. A suposição é que o SSH
raiz ou sudo e o acesso completo ao WebHost Manager (WHM) estejam disponíveis. Esse exemplo se baseia em um
host do **CentOS 7**.

**Nota**: é possível localizar a documentação do cPanel para Configurando o diretório
de backup
[aqui](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   **Nota**: por padrão, o sistema cPanel salva arquivos de backup localmente, no
diretório `/backup`. Para os propósitos deste documento, vamos supor que
`/backup` já existe e que contém os backups, de modo que usaremos `/backup2`
como o novo ponto de montagem.
   
3. Configure seu {{site.data.keyword.filestorage_short}} conforme descrito em
[Acessando o {{site.data.keyword.filestorage_short}} no
Red Hat Enterprise Linux](accessing-file-storage-linux.html) e em [Montando o
NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html). Certifique-se de montá-lo no
`/backup2` e de configurá-lo em `/etc/fstab` para ativar a montagem na
inicialização. <br />
   **Nota**: por padrão, o NFS fará downgrade de quaisquer arquivos criados com as
permissões raiz para o usuário nobody. Para permitir que os clientes raiz retenham as permissões raiz no
compartilhamento do NFS, o `no_root_squash` deve ser incluído em `/etc/exports`. <br />
   **Nota**: também há instruções disponíveis para
[Montando o NFS/{{site.data.keyword.filestorage_short}} no
CoreOS](mounting-storage-coreos.html). <br />

4. **Opcional**: copie backups existentes para o novo armazenamento. Use
`rsync`, por exemplo:
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Nota**: esse comando transmite seus dados compactados ao mesmo tempo em que os preserva o
máximo possível (exceto links físicos) e fornece informações sobre quais arquivos estão sendo transferidos, além
de um breve resumo no final.
    
5.  Efetue login no WebHost Manager e navegue para a configuração de backup por meio de
**Início** > **Backup** > **Configuração de backup**.

6.  Edite a configuração para salvar os backups no novo ponto de montagem. 
    - Mude o diretório de backup padrão inserindo o caminho absoluto para o novo local no lugar do
diretório /backup/. 
    - Selecione **Ativar para montar uma unidade de backup**. Essa configuração faz
com que o processo Configuração de backup verifique o arquivo `/etc/fstab` quanto a uma
montagem de backup (`/backup2`). <br /> **Nota**: se uma montagem existe
com o mesmo nome que o diretório temporário, o processo Configuração de backup monta a unidade e faz backup
das informações para a montagem. Depois que o processo de backup é concluído, ele desmonta a unidade. 

7. Aplique as mudanças clicando em **Salvar configuração** na parte inferior da
interface **Configuração de backup**.

8. **Opcional**: conforme determinado pelas suas necessidades de
negócios e pelo seu caso de uso específicos, remova o armazenamento antigo do servidor e cancele-o da conta.
