---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migrando o {{site.data.keyword.filestorage_short}} para o {{site.data.keyword.filestorage_short}} aprimorado
{: #migratestorage}

O {{site.data.keyword.filestorage_full}} aprimorado está agora disponível nos data centers selecionados. Para ver a lista de data centers submetidos a upgrade e recursos disponíveis, como taxas de IOPS ajustáveis e volumes expansíveis, clique [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-news). Para obter mais informações sobre criptografia gerenciada pelo provedor, consulte [Criptografia em repouso do {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-encryption).

O caminho de migração preferencial é conectar-se a ambos os volumes simultaneamente e traNFSerir dados diretamente de um LUN para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia.

A suposição é que você já tem o LUN não criptografado conectado ao seu host. Caso contrário, siga as instruções que melhor se ajustem ao seu sistema operacional para realizar essa tarefa.

- [Montando o {{site.data.keyword.filestorage_short}} no Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montando o {{site.data.keyword.filestorage_short}} no CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)

Todos os volumes aprimorados do {{site.data.keyword.filestorage_short}} provisionados nesses data centers têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os volumes de armazenamento, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** no console. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Criando um {{site.data.keyword.filestorage_short}}

Ao fazer um pedido com a API, especifique o pacote "Armazenamento como um serviço" para assegurar-se de que esteja obtendo os recursos atualizados com seu novo armazenamento.
{:important}

É possível pedir um LUN aprimorado por meio do catálogo do {{site.data.keyword.BluSoftlayer_full}} e do {{site.data.keyword.slportal}}. Seu novo volume deve ter o mesmo tamanho ou ser maior que o compartilhamento de arquivo original para facilitar a migração.

- [Pedindo o {{site.data.keyword.filestorage_short}} com camadas IOPS predefinidas (Endurance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#endurance)
- [Pedindo o {{site.data.keyword.filestorage_short}} com IOPS customizado (Performance)](/docs/infrastructure/FileStorage?topic=FileStorage-orderingConsole#performance)

Seu novo armazenamento estará disponível para montagem em alguns minutos. É possível visualizá-lo na lista de recursos e na lista do {{site.data.keyword.blockstorageshort}}.


## Autorizando o host para o novo {{site.data.keyword.filestorage_short}}

Hosts "autorizados" são aqueles que receberam acesso a um volume. Sem a autorização do host, não é possível acessar ou usar o armazenamento de seu sistema.

1. Clique no nome de seu novo volume.
2. Role para a seção  ** Hosts Autorizados ** .
3. Clique no link **Autorizar host** à direita. Selecione os hosts que podem acessar o volume.

Quando o host estiver autorizado, conecte o volume ao host.


## Configurando Capturas Instantâneas e Replicação

Se capturas instantâneas e replicação tiverem sido estabelecidas para seu volume original, será necessário configurá-las para o novo volume. Configure a replicação, o espaço de captura instantânea e crie planejamentos de captura instantânea com as mesmas configurações do volume original.

Se o data center de destino não tiver criptografia, não será possível estabelecer a replicação para o novo volume até que o data center seja submetido a upgrade.
{:important}


## Migrando seus dados

1. Conecte-se a ambos os volumes do {{site.data.keyword.filestorage_short}}, originais e novos.
  - Se precisar de assistência com a conexão dos dois compartilhamentos de arquivo a seu host, abra um chamado de suporte.

2. Considere qual tipo de dados você tem no volume original do {{site.data.keyword.filestorage_short}} e como melhor copiá-lo para o novo compartilhamento de arquivo
  - Se você tem backups, conteúdo estático e coisas que não se espera que sejam mudadas
durante a cópia, não precisa se preocupar.
  - Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.filestorage_short}}, certifique-se de que os dados não sejam alterados durante a cópia para evitar distorção de dados.
  - Se você tiver alguma preocupação com a largura de banda, faça a migração durante os horários fora de pico.
  - Se precisar de assistência com essas considerações, abra um chamado de suporte.

3. Copie seus dados entre.
   - **Microsoft Windows**
     - Para copiar dados do LUN original do {{site.data.keyword.filestorage_short}} para o novo LUN, formate o novo armazenamento e copie os arquivos usando o Windows Explorer.
   - **Linux**
     - É possível usar `rsync` para copiar os dados.
       ```
       [root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
       ```

   É uma boa ideia usar o comando anterior com a sinalização `--dry-run` uma vez para certificar-se de que os caminhos sejam alinhados corretamente. Se esse processo for interrompido, será possível excluir o último arquivo de destino que estava sendo copiado para certificar-se de que ele seja copiado para o novo local do início.

   Quando esse comando for concluído sem a sinalização `--dry-run`, seus dados serão copiados para o novo volume do {{site.data.keyword.filestorage_short}}. Execute o comando novamente para certificar-se de que nada foi perdido. Também é possível revisar manualmente ambos os locais para procurar qualquer coisa que possa estar ausente.

   Quando a migração estiver concluída, será possível mover a produção para o novo LUN. Em seguida, será possível remover e excluir o volume original de sua configuração. A exclusão também remove qualquer captura instantânea ou réplica no site de destino que tenha sido associada ao volume original.
