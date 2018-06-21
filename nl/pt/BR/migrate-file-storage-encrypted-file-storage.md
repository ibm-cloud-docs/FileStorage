---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migrando o {{site.data.keyword.filestorage_short}} para o {{site.data.keyword.filestorage_short}} aprimorado

O {{site.data.keyword.filestorage_full}} aprimorado está agora disponível nos data centers selecionados. Para ver a lista de data centers submetidos a upgrade e recursos disponíveis, como taxas de IOPS ajustáveis e volumes expansíveis, clique [aqui](new-ibm-block-and-file-storage-location-and-features.html). Para obter mais informações sobre armazenamento criptografado gerenciado por provedor, leia o artigo [Criptografia em repouso do {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html).

O caminho de migração preferencial é conectar-se aos dois LUNs simultaneamente e transferir dados diretamente de um LUN para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia. 

Supõe-se que seu LUN não criptografado já esteja conectado ao seu host. Se não, siga as instruções que se ajustam melhor ao seu sistema operacional para realizar essa tarefa:

- [Montando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)
- [Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html)

**NOTA:** todos os volumes aprimorados do {{site.data.keyword.filestorage_short}} têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os volumes criptografados e não criptografados do {{site.data.keyword.filestorage_short}}, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** na IU. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.


## Criar um novo {{site.data.keyword.filestorage_short}}

**IMPORTANTE**: ao fazer um pedido com a API, especifique o pacote "Armazenamento como um Serviço" para assegurar que você esteja recebendo os recursos atualizados com seu novo armazenamento.

As instruções a seguir são para pedir um volume/compartilhamento de arquivo aprimorado por meio da IU. Seu novo volume deve ser do mesmo tamanho ou maior que o volume original para facilitar a migração.

### Pedir um novo volume de armazenamento do Endurance

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir o {{site.data.keyword.filestorage_short}}** no canto superior direito. 
3. Selecione **Endurance** na lista **Selecionar tipo de armazenamento**.
4. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o original.
5. Selecione sua opção de faturamento. É possível escolher entre faturamento mensal ou por hora.
6. Clique em **Endurance** e selecione a camada de IOPS.
6. Selecione o **Tamanho de armazenamento utilizável** na lista. Seu novo volume deve ser do mesmo tamanho ou maior que o volume original.
7. Escolha o **Tamanho do espaço de captura instantânea** (além de seu espaço utilizável) na lista suspensa.
8. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes do pedido. Clique em **Anterior** se você desejar mudar sua ordem.
9. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**
 
### Pedir um volume de armazenamento do Performance criptografado

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** >** Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir o {{site.data.keyword.filestorage_short}}** no canto superior direito. 
3. Selecione **Desempenho** na lista Selecionar tipo de armazenamento.
4. Clique em **Local** e selecione seu data center.
    -  Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o original.
5. Selecione suas opções de faturamento. É possível escolher entre faturamento por hora e mensal.
6. Selecione o botão de opções próximo ao **Tamanho de armazenamento** apropriado.
6. Insira o IOPS no campo **Especificar IOPS**.
7. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes do pedido. Clique em **Anterior** se você desejar mudar sua ordem.
8. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.

O armazenamento será provisionado em menos de um minuto e ficará visível na página {{site.data.keyword.filestorage_short}} do {{site.data.keyword.slportal}}.

 
## Conectar o novo {{site.data.keyword.filestorage_short}} ao host

Hosts "autorizados" são hosts que receberam direitos de acesso a um volume. Sem autorização do host, você não será capaz de acessar ou usar o armazenamento de seu sistema.

1. Clique no nome de seu novo volume.
2. Role para a seção **Hosts autorizados** da página.
3. Clique no link **Autorizar host** no lado direito da página. Selecione os hosts que podem acessar o volume.

Quando autorizado, conecte o volume ao seu host.

 
## Capturas instantâneas e replicação

Você tem capturas instantâneas e replicação estabelecida para seu volume original? Se sim, será necessário configurar a replicação, o espaço de captura instantânea e criar planejamentos de captura instantânea para o novo volume criptografado com as mesmas configurações que o volume original. 

**Nota**: se o seu data center de destino não foi submetido a upgrade para criptografia, não será possível estabelecer a replicação para o novo volume até que o data center seja submetido a upgrade.

 
## Migrar seus dados

O host deve estar conectado aos volumes do {{site.data.keyword.filestorage_short}} original e criptografado. Se não:

- Certifique-se de que você seguiu as etapas neste documento e referenciou os documentos corretamente.
- Abra um chamado de suporte para obter assistência adicional ao conectar os dois volumes a seu host.

### Considerações de dados

Neste momento, considere que tipo de dados você tem em seu volume original do {{site.data.keyword.filestorage_short}} e qual a melhor maneira de copiá-los para o seu volume criptografado. Se você tem backups, conteúdo estático e coisas que não são esperadas mudar durante a cópia, não há nenhuma consideração importante.

Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.filestorage_short}}, certifique-se de que os dados no volume original não sejam alterados durante a cópia para que nenhuma distorção ocorra. Se você tiver preocupações com relação à largura da banda, será necessário executar a migração durante os horários fora de pico. Se precisar de assistência com essas considerações, abra um chamado de suporte.

### Microsoft Windows

Para copiar dados do volume original do {{site.data.keyword.filestorage_short}} para seu volume criptografado, formatar o novo armazenamento e copie os arquivos usando o Windows Explorer.

### Linux

Você pode considerar usar `rsync` para copiar sobre os dados. Aqui está um comando de exemplo:

```
[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage
```

É recomendado usar o exemplo de comando com a sinalização `--dry-run` uma vez para garantir que os caminhos sejam alinhados corretamente. Se esse processo for interrompido, você talvez deseje excluir o último arquivo de destino que estava sendo copiado para assegurar que ele seja copiado do início no novo local.

Quando esse comando é concluído sem a sinalização `--dry-run`, seus dados devem ser copiados para o volume criptografado do {{site.data.keyword.filestorage_short}}. É necessário rolar para cima e executar o comando novamente para ter certeza de que nada foi perdido. Você também pode desejar revisar manualmente ambos os locais para procurar qualquer coisa que possa estar ausente.

Quando a migração for concluída, você será capaz de mover a produção para o volume criptografado e separar e excluir o volume original da sua configuração. 

**Nota**: a exclusão também removerá qualquer captura instantânea ou réplica no site de destino que foi associada ao volume original.
