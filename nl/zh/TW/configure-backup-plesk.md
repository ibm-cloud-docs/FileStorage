---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurando o {{site.data.keyword.filestorage_short}} para backup com o Plesk

É possível usar essas instruções para configurar o {{site.data.keyword.filestorage_full}} para seus backups no Plesk. A suposição é que o SSH
raiz ou sudo e o acesso Plesk no nível de administrador integral estejam disponíveis. Esse exemplo se baseia
em um host do CentOS7.

Para obter mais informações, consulte a [Documentação do Plesk para backup e restauração ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Conecte-se ao host por meio de SSH.
2. Assegure-se de que um destino de ponto de montagem exista. <br />

   O Plesk tem duas opções para armazenar backups. Uma é o armazenamento de Plesk interno, que é armazenamento em seu servidor Plesk. A outra é armazenamento de FTP externo, que é o armazenamento em algum servidor externo na web ou em sua rede local. Geralmente em caixas do Plesk, os backups internos são armazenados em
`/var/lib/psa/dumps` e usam `/tmp` como um diretório temporário. Nesse exemplo, o diretório temporário é mantido local, mas o diretório `dumps` é movido para o destino do {{site.data.keyword.filestorage_short}} (`/backup/psa/dumps`). Nenhuma credencial do usuário FTP é necessária.
   {:note}
3. Configure seu {{site.data.keyword.filestorage_short}} conforme descrito em [Acessando o {{site.data.keyword.filestorage_short}} no Red Hat Enterprise Linux](accessing-file-storage-linux.html) e [Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html) ou [Montando o NFS/{{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html). Monte o volume em `/backup` e configure-o na tabela de sistema de arquivos (`/etc/fstab`) para ativar a montagem no início. <br />

   Por padrão, o NFS faz downgrade de quaisquer arquivos que foram criados com as permissões raiz para o usuário nobody. Para permitir que os clientes raiz retenham as permissões raiz no compartilhamento NFS, `no_root_squash` precisa ser incluído em `/etc/exports`.
   {:tip}
4. **Opcional**. Copie os backups existentes para o novo armazenamento. Use
`rsync`, por exemplo:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {:pre}

   Esse comando compacta e transmite seus dados e os preserva o máximo possível, exceto links físicos. Ele também fornece informações sobre quais arquivos estão sendo transferidos e um breve resumo no final.
   {:tip}
5. Edite `/etc/psa/psa.conf` para apontar o valor de `DUMP_D`
para o novo destino.
    - Ele aparece como:  ` DUMP_D /backup/psa/dumps `.
6. **Opcional**. Conforme determinado por seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele-o da conta.
