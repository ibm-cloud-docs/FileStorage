---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.filestorage_short}} - Perguntas mais frequentes

## Como o IOPS é medido?

O IOPS é medido com base em um perfil de carregamento de blocos de 16 KB com 50% de leituras e 50% de gravações aleatórias. As cargas de trabalho que diferem desse perfil podem experimentar desempenho inferior.

## O que acontece se eu uso um tamanho de bloco menor ao medir o desempenho?

O máximo de IOPS ainda pode ser obtido ao usar tamanhos de blocos menores, porém o rendimento será inferior. Por exemplo, um volume com 6.000 IOPS teria o rendimento a seguir em vários tamanhos de blocos:

- 16 KB * 6.000 IOPS == ~93,75 MB/s
- 8 KB * 6.000 IOPS == ~46,88 MB/s
- 4 KB * 6.000 IOPS == ~23,44 MB/s


## O volume precisa ser pré-aquecido para alcançar o rendimento esperado?

Não há necessidade de pré-aquecimento. Você observará o rendimento especificado imediatamente depois de provisionar o volume.

## Como posso identificar quais de meus LUNs/volumes do {{site.data.keyword.filestorage_short}} são criptografados?

Ao visualizar sua lista de {{site.data.keyword.filestorage_short}} no portal do cliente, você verá um ícone de bloqueio à direita do LUN/nome do volume para aqueles que são criptografados.

## Se eu tiver {{site.data.keyword.filestorage_short}} não criptografado provisionado em um data center que foi submetido a upgrade para criptografia, posso criptografar meus {{site.data.keyword.filestorage_short}}s?

O {{site.data.keyword.filestorage_short}} que é provisionado antes de um upgrade do data center não pode ser criptografado. O novo {{site.data.keyword.filestorage_short}} provisionado em data centers submetidos a upgrade é criptografado automaticamente; não há nenhuma configuração de criptografia para escolher, isso é automático. Os dados no armazenamento não criptografado em um data center submetido a upgrade podem ser criptografados criando um novo volume de arquivo, em seguida, copiando os dados para o novo volume criptografado ou volume com migração baseada em host. Veja [este artigo](/docs/infrastructure/FileStorage/migrate-file-storage-encrypted-file-storage.html) para obter instruções sobre como executar a migração.

## Como eu sei se estou provisionando o {{site.data.keyword.filestorage_short}} em um data center submetido a upgrade?

Ao provisionar o {{site.data.keyword.filestorage_short}}, todos os data centers submetidos a upgrade serão denotados com um asterisco (`*`) no formulário de ordem e uma indicação de que você provisionará o armazenamento com criptografia. Quando o armazenamento for provisionado, você verá um ícone na lista de armazenamentos que mostra esse volume ou o volume como criptografado. Todos os volumes criptografados e os volumes são provisionados somente em data centers submetidos a upgrade. É possível localizar uma lista completa de data centers submetidos a upgrade e recursos disponíveis [aqui](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Por que eu posso provisionar o {{site.data.keyword.filestorage_short}} com uma camada 10 IOPS do Endurance em alguns data centers e não em outros?

A camada 10 IOPS/GB do tipo Endurance do {{site.data.keyword.filestorage_short}} está disponível somente em data centers selecionados, com novos data centers sendo incluídos em breve. É possível localizar uma lista completa de data centers submetidos a upgrade e recursos disponíveis [aqui](/docs//infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

## Como posso localizar o ponto de montagem correto para o meu {{site.data.keyword.filestorage_short}}?

Todos os volumes criptografados do {{site.data.keyword.filestorage_short}} provisionados nestes data centers têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os volumes criptografados e não criptografados do {{site.data.keyword.filestorage_short}}, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** na UI, assim como acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Quantos compartilhamentos de arquivo são permitidos por tamanho do volume de arquivo? Quais são os compartilhamentos máximos de arquivo permitidos por tamanho do volume?
A seguir estão os nós-i ou compartilhamentos de arquivo máximos permitidos com base no tamanho do volume:

<table>
        <tbody>
          <tr>
            <th>Tamanho do volume</th>
            <th>Nós-i/arquivos</th>
          </tr>
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

## Quantos volumes posso provisionar?

Por padrão, é possível provisionar um total combinado de 250 volumes de armazenamento de arquivo e bloco. Entre em contato com seu representante de vendas para aumentar seus volumes.

## Quantas instâncias podem compartilhar o uso de um volume fornecido do {{site.data.keyword.filestorage_short}}?

O limite padrão para o número de autorizações por volume de arquivo é 64. Entre em contato com seu representante de vendas para aumentar o limite.

## Ao provisionar o {{site.data.keyword.filestorage_short}} Performance ou Endurance, o IOPS alocado é cumprido por instância ou por volume?

O IOPS é cumprido no nível de volume. Dito de forma diferente, dois hosts conectados a um volume com 6.000 IOPS compartilham esses 6.000 IOPS.

## Vou ser capaz de alcançar mais rendimento se usei uma conexão Ethernet mais rápida?

Os limites de rendimento são configurados em um nível por volume/LUN, portanto usar uma conexão Ethernet mais rápida não aumentará o limite configurado. No entanto, com uma conexão Ethernet mais lenta, sua largura da banda pode ser um gargalo potencial.

## Os firewalls/grupos de segurança afetarão o desempenho?

Como uma melhor prática, recomendamos executar o tráfego de armazenamento em uma vlan que efetua bypass do firewall. Executar o tráfego de armazenamento por meio de firewalls de software aumentará a latência e afetará adversamente o desempenho de armazenamento.

## O que acontece com os meus dados quando os Volumes do {{site.data.keyword.filestorage_short}} são excluídos?

Quando o armazenamento é excluído, quaisquer ponteiros para os dados nesse volume são removidos, portanto os dados se tornam completamente inacessíveis. Se o armazenamento físico é reprovisionado para outra conta, um novo conjunto de ponteiros é designado. Não há nenhuma maneira para a nova conta acessar quaisquer dados que podem ter estado no armazenamento físico, o novo conjunto de ponteiros mostra todos como 0 (zero). Quando novos dados são gravados no volume/LUN, quaisquer dados inacessíveis que ainda existem são sobrescritos. 

## Qual latência de desempenho posso esperar do meu {{site.data.keyword.filestorage_short}}?   

A latência de destino dentro do armazenamento é <1 ms. Nosso armazenamento está conectado a instâncias de cálculo em uma rede compartilhada, então a latência exata de desempenho dependerá do tráfego de rede dentro de um intervalo de tempo especificado.

