---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, cPanel, backups

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurando o {{site.data.keyword.filestorage_short}} para backup com o cPanel
{: #cPanelBackups}

É possível usar essas instruções para configurar seus backups para serem armazenados no {{site.data.keyword.filestorage_full}} por cPanel. A suposição é que o SSH
raiz ou sudo e o acesso completo ao WebHost Manager (WHM) estejam disponíveis. Esse exemplo se baseia em um
host do **CentOS 7**.

Para obter mais informações, consulte [cPanel - Configurando o diretório de backup](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external}.
{:tip}

1. Conecte-se ao host por meio de SSH.
2. Assegure-se de que um destino de ponto de montagem exista. <br />

   Por padrão, o sistema cPanel salva os arquivos de backup localmente, no diretório `/backup`. Nesse documento, a suposição é que a pasta `/backup` existe e contém backups, e `/backup2` pode ser usado como o novo ponto de montagem.
   {:note}

3. Configure seu {{site.data.keyword.filestorage_short}} conforme descrito em [Acessando o {{site.data.keyword.filestorage_short}} no Red Hat Enterprise Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux) e [Montando o {{site.data.keyword.filestorage_short}} no CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)/[Montando o NFS/{{site.data.keyword.filestorage_short}} no CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS). Monte o volume em `/backup2` e configure-o na tabela do sistema de arquivos (`/etc/fstab`) para ativar a montagem no início. <br />

   Por padrão, o NFS faz downgrade de quaisquer arquivos que foram criados com as permissões raiz para o usuário nobody. Para permitir que os clientes raiz retenham as permissões raiz no compartilhamento NFS, `no_root_squash` precisa ser incluído em `/etc/exports`.
   {:tip}

4. **Opcional**. Copie os backups existentes para o novo armazenamento. É possível usar  ` rsync ` , por exemplo.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Esse comando compacta e transmite seus dados e os preserva o máximo possível, exceto links físicos. Ele também fornece informações sobre quais arquivos estão sendo transferidos via NFS, além de um breve resumo no final.
    {:tip}

5. Efetue login no WebHost Manager e acesse a configuração de backup por meio de **Página inicial** > **Backup** > **Configuração de backup**.

6. Edite a configuração para salvar os backups no novo ponto de montagem.
    - Mude o diretório de backup padrão inserindo o caminho absoluto para o novo local no lugar do diretório `/backup/`.
    - Selecione **Ativar para montar uma unidade de backup**. Essa configuração faz com que o processo de configuração verifique o arquivo `/etc/fstab` quanto a uma montagem de backup (`/backup2`). <br />

      Se existir uma montagem com o mesmo nome que o diretório temporário, o processo de configuração de backup montará a unidade e fará backup das informações ali. Depois que o processo de backup é concluído, ele desmonta a unidade.
      {:note}
7. Aplique as mudanças clicando em **Salvar configuração**.
8. **Opcional**. Conforme determinado por seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele-o da conta.
