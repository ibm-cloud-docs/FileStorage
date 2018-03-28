---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
 
# Migrando o {{site.data.keyword.filestorage_short}} para o {{site.data.keyword.filestorage_short}} criptografado

O {{site.data.keyword.filestorage_full}} criptografado para Endurance ou Performance foi ativado em data centers selecionados. Abaixo você localizará informações sobre como migrar seu {{site.data.keyword.filestorage_short}} de não criptografado para criptografado. Para obter mais informações sobre o armazenamento criptografado gerenciado pelo provedor, leia o artigo [Criptografia em repouso do {{site.data.keyword.filestorage_short}}](block-file-storage-encryption-rest.html). Para ver uma lista de data centers submetidos a upgrade e recursos disponíveis, clique [aqui](new-ibm-block-and-file-storage-location-and-features).

O caminho de migração preferencial é para conectar ambos os volumes simultaneamente e transferir dados diretamente de volume de arquivo para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia.

Os cenários mais comuns foram descritos para sua conveniência. Há uma suposição de que seu volume de arquivo não criptografado já está conectado ao seu host. Se não, siga as instruções abaixo que melhor se ajustem ao sistema operacional que está em execução para realizar essa tarefa. 

**NOTA:** todos os volumes criptografados do {{site.data.keyword.filestorage_short}} têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os volumes criptografados e não criptografados do {{site.data.keyword.filestorage_short}}, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** na UI, assim como acessar o ponto de montagem correto por meio de uma chamada API: SoftLayer_Network_Storage::getNetworkMountAddress().

[Acessando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)

## Criar um volume de arquivo criptografado

Use as etapas a seguir para criar um volume do mesmo tamanho ou maior do que está criptografado para facilitar o processo de migração.

### Pedir um volume de armazenamento do Endurance criptografado

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** na [página inicial do {{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no catálogo do {{site.data.keyword.BluSoftlayer_full}}.

2. Clique no link **Pedir {{site.data.keyword.filestorage_short}}** na página {{site.data.keyword.filestorage_short}}.

3. Selecione **Endurance**.

4. Selecione o data center no qual seu volume original está localizado. Observe que a criptografia está disponível somente em data centers com um asterisco.

5. Insira a **Camada do IOPS** desejada.

6. Selecione a quantia desejada de espaço de armazenamento em GBs. Para TB, 1 TB é igual a 1.000 GB e 12 TB é igual a 12.000 GB.

7. Digite a quantia desejada de espaço de armazenamento em GB para capturas instantâneas.

8. Selecione o **S.O. VMware** na lista suspensa.

9. Envie a ordem.
 
### Pedir um volume de armazenamento do Performance criptografado

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** na [página inicial do {{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}** no catálogo do {{site.data.keyword.BluSoftlayer_full}}.

2. Clique em **Pedir {{site.data.keyword.filestorage_short}}**.

3. Selecione **Performance**.

4. Selecione o data center no qual seu volume original está localizado. Observe que a criptografia está disponível somente em data centers com um asterisco (`*`).

5. Selecione a quantia desejada de espaço de armazenamento em GBs do mesmo tamanho do volume original ou maior.

6. Insira a quantia desejada de IOPS em intervalos de 100.

7. Selecione o S.O. VMware na lista suspensa.

8. Envie a ordem.

O armazenamento será provisionado em menos de um minuto e estará visível na página {{site.data.keyword.filestorage_short}} do portal do cliente.

 
## Conectar novo volume ao host

Os hosts “Autorizados” são hosts que receberam direitos de acesso para um volume. Sem autorização do host, você não será capaz de acessar ou usar o armazenamento de seu sistema.

1. Clique em seu **Nome do volume** criptografado.

2. Role para a seção **Hosts autorizados** da página.

3. Clique no link **Autorizar host** no lado direito da página. Selecione os hosts que podem acessar o volume.

Quando autorizado, conecte o volume ao seu host.

 
## Capturas instantâneas e replicação

Você tem capturas instantâneas e replicação estabelecida para seu volume original? Se sim, você precisará configurar a replicação, o espaço de captura instantânea e criar planejamentos de captura instantânea para o novo volume criptografado com as mesmas configurações que o volume original. 

Observe que se o seu data center de destino não tiver sido submetido a upgrade para criptografia, você não será capaz de estabelecer a replicação para o novo volume até que o data center seja submetido a upgrade.

 
## Migrar seus dados

O host deve estar conectado aos volumes do {{site.data.keyword.filestorage_short}} original e criptografado. Se não:

• Certifique-se de que você tenha seguido as etapas acima e referenciado documentos corretamente.

• Abra um chamado de suporte para obter assistência adicional ao conectar os dois volumes a seu host.

### Considerações de dados

Neste momento, considere que tipo de dados você tem em seu volume original do {{site.data.keyword.filestorage_short}} e qual a melhor maneira de copiá-los para o seu volume criptografado. Se você tem backups, conteúdo estático e coisas que não são esperadas mudar durante a cópia, não há nenhuma consideração importante.

Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.filestorage_short}}, certifique-se de que os dados no volume original não sejam alterados durante a cópia para que nenhum dano ocorra. Se você tiver quaisquer interesses de largura da banda, será necessário executar a migração durante horários fora de pico. Se precisar de assistência com estas considerações, não hesite em abrir um chamado de suporte.

### Microsoft Windows

Para copiar dados do volume original do {{site.data.keyword.filestorage_short}} para seu volume criptografado, formatar o novo armazenamento e copie os arquivos usando o Windows Explorer.

### Linux

Você pode considerar o uso de rsync para copiar os dados. Abaixo está um exemplo de comando

`[root@server ~]# rsync -Pavzu /path/to/original/file/storage/* /path/to/encrypted/file/storage` 

É recomendado usar o comando acima com a sinalização `--dry-run` uma vez para garantir que os caminhos sejam alinhados corretamente. Se esse processo for interrompido, você talvez deseje excluir o último arquivo de destino que estava sendo copiado para assegurar que ele seja copiado do início no novo local.

Quando esse comando é concluído sem a sinalização `--dry-run`, seus dados devem ser copiados para o volume criptografado do {{site.data.keyword.filestorage_short}}. É necessário rolar para cima e executar o comando novamente para ter certeza de que nada foi perdido. Você também pode desejar revisar manualmente ambos os locais para procurar qualquer coisa que possa estar ausente.

Quando a migração for concluída, você será capaz de mover a produção para o volume criptografado e separar e excluir o volume original da sua configuração. Observe que a exclusão também removerá qualquer captura instantânea ou réplica no site de destino que foi associada ao volume original.
