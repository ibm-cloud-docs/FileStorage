---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}

# {{site.data.keyword.filestorage_short}} - Perguntas mais frequentes

## Como posso saber quais dos meus volumes do {{site.data.keyword.filestorage_short}} são criptografados?
Veja a sua lista de {{site.data.keyword.filestorage_short}} no portal do cliente. É possível ver um ícone de bloqueio à direita do nome do LUN/do volume para os volumes que estão criptografados.

## Se eu comprei um {{site.data.keyword.filestorage_short}} não criptografado em um data center que foi submetido a upgrade para criptografia, posso criptografar meu {{site.data.keyword.filestorage_short}}?
O {{site.data.keyword.filestorage_short}} que foi provisionado antes de um upgrade do data center não pode ser criptografado. O novo {{site.data.keyword.filestorage_short}} provisionado em data centers submetidos a upgrade é criptografado automaticamente. Não há configuração de criptografia para escolher, é automático. Os dados no armazenamento não criptografado podem ser criptografados criando um novo volume e, em seguida, copiando os dados para o novo volume criptografado com migração baseada em host. Para obter mais informações, consulte [Migrando o File Storage](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html).

## Como eu sei se estou provisionando o {{site.data.keyword.filestorage_short}} em um data center submetido a upgrade?
No formulário de pedido do {{site.data.keyword.filestorage_short}}, todos os data centers submetidos a upgrade são denotados com um asterisco (`*`). Durante o processo de pedido, você recebe uma indicação de que está provisionando armazenamento com criptografia. Quando o armazenamento é provisionado, é possível ver um ícone na lista de armazenamento que mostra esse volume como criptografado. 

Todos os volumes criptografados e os compartilhamentos de arquivo são provisionados somente em data centers submetidos a upgrade. É possível localizar uma lista completa de data centers submetidos a upgrade e recursos disponíveis [aqui](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Por que o {{site.data.keyword.filestorage_short}} com uma camada de 10 IOPS do Endurance é provisionado em alguns data centers e em outros não?
A camada de 10 IOPS/GB do tipo Endurance do {{site.data.keyword.filestorage_short}} está disponível somente em data centers selecionados e novos data centers serão incluídos em breve. É possível localizar uma lista completa de data centers submetidos a upgrade e recursos disponíveis [aqui](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Como posso localizar o ponto de montagem correto para o meu {{site.data.keyword.filestorage_short}}?
Todos os volumes criptografados do {{site.data.keyword.filestorage_short}} que são provisionados nos data centers aprimorados têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto, visualize as informações do ponto de montagem na página **Detalhes do volume** na IU. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Quantos volumes posso provisionar?
Por padrão, é possível provisionar um total combinado de 250 volumes de armazenamento de arquivo e bloco. Para aumentar o limite, entre em contato com seu representante de vendas.

## Quantas instâncias podem compartilhar o uso de um volume fornecido do {{site.data.keyword.filestorage_short}}?
O limite padrão para o número de autorizações por volume de arquivo é 64. Para aumentar esse limite, entre em contato com seu representante de vendas.

## Quantos compartilhamentos de arquivo são permitidos por tamanho do volume de arquivo? Quais são os compartilhamentos máximos de arquivo permitidos por tamanho do volume?

<table>
  <caption>A Tabela 1 mostra o número máximo de nós-i permitidos com base no tamanho do volume. Os tamanhos de volume estão à esquerda. O número de nós-i/compartilhamentos de arquivo está à direita.</caption>
  <thead>
    <tr>
      <th>Tamanho do volume</th>
      <th>Nós-i/Compartilhamentos de arquivo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB </td>
      <td>622.484</td>
    </tr>
    <tr>
      <td>40 GB </td>
      <td>1.245.084</td>
    </tr>          
    <tr>
      <td>80 GB</td>
      <td>2.490.263</td>
    </tr>          
    <tr>
      <td>100 GB</td>
      <td>3.112.863</td>
    </tr>          
    <tr>
      <td>250 GB</td>
      <td>7.782.300</td>
    </tr>          
    <tr>
      <td>500 GB</td>
      <td>15.564.695</td>
    </tr>
    <tr>
      <td>1 TB+</td>
      <td>31.876.593</td>
    </tr>
   </tbody>
</table>

## Medindo IOPS
O IOPS é medido com base em um perfil de carregamento de blocos de 16 KB com 50 por cento de leituras e 50 por cento de gravações aleatórias. As cargas de trabalho que diferirem desse perfil poderão enfrentar desempenho insatisfatório.

## O que acontece quando eu uso um tamanho de bloco menor para medir o desempenho?
O máximo de IOPS poderá ser obtido mesmo se você usar tamanhos de blocos menores. No entanto, o rendimento será menor nesse caso. Por exemplo, um volume com 6.000 IOPS tem o rendimento a seguir em vários tamanhos de bloco:

- 16 KB * 6.000 IOPS == ~93,75 MB/s
- 8 KB * 6.000 IOPS == ~46,88 MB/s
- 4 KB * 6.000 IOPS == ~23,44 MB/s


## O IOPS alocado é cumprido por instância ou por volume?
O IOPS é cumprido no nível de volume. Dito de forma diferente, dois hosts conectados a um volume com 6.000 IOPS compartilham esses 6.000 IOPS.

## O volume precisa ser pré-aquecido para alcançar o rendimento esperado?
Não há necessidade de pré-aquecimento. É possível observar o rendimento especificado imediatamente ao provisionar o volume.

## É possível ter mais rendimento caso uma conexão Ethernet mais rápida seja usada?
Os limites de rendimento são configurados de acordo com o nível de volume/LUN, então, o uso de uma conexão Ethernet mais rápida não aumenta esse limite configurado. No entanto, com uma conexão Ethernet mais lenta, sua largura da banda pode ser um gargalo potencial.

## Os firewalls / grupos de segurança afetam o desempenho?
É melhor executar o tráfego de armazenamento em uma VLAN, que efetua bypass do firewall. A execução do tráfego de armazenamento por meio de firewalls de software aumenta a latência e prejudica o desempenho do armazenamento.

## Qual latência de desempenho pode ser esperada do {{site.data.keyword.filestorage_short}}?   
A latência de destino dentro do armazenamento é <1 ms. O armazenamento é conectado a instâncias de cálculo em uma rede compartilhada, portanto, a latência exata de desempenho depende do tráfego de rede durante a operação.

## O que acontece com os dados quando os Volumes do {{site.data.keyword.filestorage_short}} são excluídos?
Quando o armazenamento é excluído, todos os ponteiros para os dados nesse volume são removidos, portanto, os dados se tornam inacessíveis. Se o armazenamento físico é reprovisionado para outra conta, um novo conjunto de ponteiros é designado. Não há como a nova conta acessar quaisquer dados que estavam no armazenamento físico. O novo conjunto de ponteiros mostra todos 0s. Os novos dados sobrescrevem todos os dados inacessíveis que existiam nesse armazenamento físico.

## O que acontece com as unidades que são desatribuídas do data center de nuvem?
Quando as unidades são desatribuídas, a IBM as destrói antes de elas serem descartadas. As unidades se tornam inutilizáveis. Quaisquer dados que foram gravados nessa unidade se tornam inacessíveis.
