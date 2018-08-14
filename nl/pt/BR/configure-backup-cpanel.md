---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.filestorage_short}} para backup com o cPanel

É possível usar essas instruções para configurar seus backups para serem armazenados no {{site.data.keyword.filestorage_full}} por cPanel. A suposição é que o SSH
raiz ou sudo e o acesso completo ao WebHost Manager (WHM) estejam disponíveis. Esse exemplo se baseia em um
host do **CentOS 7**.

>**Nota** - É possível localizar a documentação do cPanel para configurar um diretório de backup [aqui](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   >**Nota** - Por padrão, o sistema do cPanel salva arquivos de backup localmente, no diretório `/backup`. Nesse documento, a suposição é que a pasta `/backup` existe e contém backups, e `/backup2` pode ser usado como o novo ponto de montagem.
   
3. Configure seu {{site.data.keyword.filestorage_short}} conforme descrito em [Acessando o {{site.data.keyword.filestorage_short}} no Red Hat Enterprise Linux](accessing-file-storage-linux.html) e [Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html)/[Montando o NFS/{{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html). Monte o volume em `/backup2` e configure-o na tabela do sistema de arquivos (`/etc/fstab`) para ativar a montagem no início. <br />
   >**Nota** - por padrão, o NFS faz downgrade de quaisquer arquivos que foram criados com as permissões raiz para o usuário nobody. Para permitir que os clientes raiz retenham as permissões raiz no compartilhamento do NFS, `no_root_squash` precisa ser incluído em `/etc/exports`. 

4. **Opcional**. Copie os backups existentes para o novo armazenamento. É possível usar  ` rsync ` , por exemplo.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    >**Nota** - Esse comando compacta e transmite seus dados, além de preservar o máximo possível, exceto links físicos. Ele também fornece informações sobre quais arquivos estão sendo transferidos, além de um breve resumo no final.
    
5. Efetue login no WebHost Manager e acesse a configuração de backup por meio de **Página inicial** > **Backup** > **Configuração de backup**.

6. Edite a configuração para salvar os backups no novo ponto de montagem. 
    - Mude o diretório de backup padrão inserindo o caminho absoluto para o novo local no lugar do diretório `/backup/`. 
    - Selecione **Ativar para montar uma unidade de backup**. Essa configuração faz com que o processo de configuração verifique o arquivo `/etc/fstab` quanto a uma montagem de backup (`/backup2`). <br /> 
      >**Nota** - Se existir uma montagem com o mesmo nome que o diretório temporário, o processo de configuração de backup montará a unidade e fará backup das informações lá. Depois que o processo de backup é concluído, ele desmonta a unidade. 

7. Aplique as mudanças clicando em **Salvar configuração**.

8. **Opcional**. Conforme determinado por seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele-o da conta.
