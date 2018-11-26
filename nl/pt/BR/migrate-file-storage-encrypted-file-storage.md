---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Migrando o {{site.data.keyword.filestorage_short}} para o {{site.data.keyword.filestorage_short}} aprimorado

O {{site.data.keyword.filestorage_full}} aprimorado está agora disponível nos data centers selecionados. Para ver a lista de data centers submetidos a upgrade e recursos disponíveis, como taxas de IOPS ajustáveis e volumes expansíveis, clique [aqui](new-ibm-block-and-file-storage-location-and-features.html). Para obter mais informações sobre o armazenamento criptografado gerenciado pelo provedor, consulte [{{site.data.keyword.filestorage_short}}Criptografia em repouso](block-file-storage-encryption-rest.html).

O caminho de migração preferencial é conectar-se a ambos os volumes simultaneamente e transferir dados diretamente de um LUN para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia.

Supõe-se que seu LUN não criptografado já esteja conectado ao seu host. Caso contrário, siga as instruções que melhor se ajustem ao seu sistema operacional para realizar essa tarefa.

- [Montando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)
- [Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html)

Todos os volumes do {{site.data.keyword.filestorage_short}} aprimorados têm um ponto de montagem diferente dos volumes não criptografados. Para assegurar-se de que esteja usando o ponto de montagem correto para os volumes criptografados e não criptografados do {{site.data.keyword.filestorage_short}}, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** no {{site.data.keyword.slportal}}. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}


## Criando um novo  {{site.data.keyword.filestorage_short}}

Ao fazer um pedido com a API, especifique o pacote "Armazenamento como um serviço" para assegurar-se de que esteja obtendo os recursos atualizados com seu novo armazenamento.
{:important}

As instruções a seguir são para pedir um volume/compartilhamento de arquivo aprimorado por meio do catálogo do {{site.data.keyword.slportal}}/{{site.data.keyword.BluSoftlayer_full}}. Seu novo volume deve ser do mesmo tamanho ou maior que o volume original para facilitar a migração.

### Solicitando um novo volume de Armazenamento de Endurance

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir {{site.data.keyword.filestorage_short}}**.
3. Selecione **Endurance** na lista **Selecionar tipo de armazenamento**.
4. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o original.
5. Selecione sua opção de faturamento. É possível escolher entre faturamento mensal ou por hora.
6. Clique em **Endurance** e selecione a camada de IOPS.
6. Selecione o **Tamanho de armazenamento utilizável** na lista. Seu novo volume deve ser do mesmo tamanho ou maior que o volume original.
7. Escolha o **Tamanho do espaço de captura instantânea** (além de seu espaço utilizável) na lista.
8. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes do pedido. Clique em **Anterior** se você desejar mudar sua ordem.
9. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**

### Solicitando um Volume de Armazenamento de Desempenho Criptografado

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** >** Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir {{site.data.keyword.filestorage_short}}**.
3. Selecione **Performance** na lista **Selecionar tipo de armazenamento**.
4. Clique em **Local** e selecione seu data center.
    -  Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o original.
5. Selecione suas opções de faturamento. É possível escolher entre faturamento por hora e mensal.
6. Selecione o botão de opções próximo ao **Tamanho de armazenamento** apropriado.
6. Insira o IOPS no campo **Especificar IOPS**.
7. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes do pedido. Clique em **Anterior** se você desejar mudar sua ordem.
8. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.

O armazenamento é provisionado em menos de um minuto e é visível na página {{site.data.keyword.filestorage_short}} do {{site.data.keyword.slportal}}.


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
  - Se você tiver backups, conteúdo estático e coisas que não devem mudar durante a cópia, não haverá grandes preocupações.
  - Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.filestorage_short}}, certifique-se de que os dados não sejam alterados durante a cópia para evitar distorção de dados. Se você tiver alguma preocupação com a largura de banda, faça a migração durante os horários fora de pico. Se precisar de assistência com essas considerações, abra um chamado de suporte.

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
