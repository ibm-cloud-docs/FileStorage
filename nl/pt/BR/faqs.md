---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-26"

keywords: File Storage, encryption, security, provisioning, limitations, NFS

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# Perguntas mais frequentes
{: #faqs}

## Como posso saber quais dos meus volumes do {{site.data.keyword.filestorage_short}} são criptografados?
{: faq}

Veja a sua lista de {{site.data.keyword.filestorage_short}} no portal do cliente. É possível ver um ícone de bloqueio à direita do nome de volume para os volumes que estão criptografados.

## Se eu comprei um {{site.data.keyword.filestorage_short}} não criptografado em um data center que foi submetido a upgrade para criptografia, posso criptografar meu {{site.data.keyword.filestorage_short}}?
{: faq}

O {{site.data.keyword.filestorage_short}} que foi provisionado antes de um upgrade do data center não pode ser criptografado. O novo {{site.data.keyword.filestorage_short}} que foi fornecido em data centers com upgrade é criptografado automaticamente. Ele é automático, não uma configuração de fornecimento que pode ser selecionada ou deixada de fora. Os dados no armazenamento não criptografado podem ser criptografados criando um novo volume e, em seguida, copiando os dados para o novo volume criptografado com migração baseada em host. Para obter mais informações, consulte [Migrando o File Storage](/docs/infrastructure/FileStorage?topic=FileStorage-migratestorage).

## Como eu sei se estou provisionando o {{site.data.keyword.filestorage_short}} em um data center submetido a upgrade?
{: faq}

No formulário de pedido do {{site.data.keyword.filestorage_short}}, todos os data centers submetidos a upgrade são denotados com um asterisco (`*`). Durante o processo de pedido, você recebe uma indicação de que está provisionando armazenamento com criptografia. Quando o armazenamento é provisionado, é possível ver um ícone na lista de armazenamento que mostra esse volume como criptografado.

Todos os volumes criptografados e os compartilhamentos de arquivo são provisionados somente em data centers submetidos a upgrade. É possível localizar uma lista completa de data centers submetidos a upgrade e recursos disponíveis [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-news).

## Por que o {{site.data.keyword.filestorage_short}} com uma camada de 10 IOPS do Endurance é provisionado em alguns data centers e em outros não?
{: faq}

A camada de 10 IOPS/GB do tipo Endurance do {{site.data.keyword.filestorage_short}} está disponível somente em data centers selecionados e novos data centers serão incluídos em breve. É possível localizar uma lista completa de data centers submetidos a upgrade e recursos disponíveis [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-news).

## Como posso localizar o ponto de montagem correto para o meu {{site.data.keyword.filestorage_short}}?
{: faq}

Todos os volumes criptografados do {{site.data.keyword.filestorage_short}} que são provisionados nos data centers aprimorados têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto, visualize as informações do ponto de montagem na página **Detalhes do volume** na IU. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

## Quantos volumes posso provisionar?
{: faq}

Por padrão, é possível provisionar um total combinado de 250 volumes de armazenamento de arquivo e bloco. Para aumentar o limite, entre em contato com seu representante de vendas. Para obter mais informações, veja [Gerenciando limites de armazenamento](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).

## Quantas instâncias podem compartilhar o uso de um volume fornecido do {{site.data.keyword.filestorage_short}}?
{: faq}

O limite padrão para o número de autorizações por volume de arquivo é 64. Para aumentar esse limite, entre em contato com seu representante de vendas.

## Quantos volumes do {{site.data.keyword.filestorage_short}} podem ser anexados a um único host?
{: faq}

Isso depende do que o sistema operacional do host é capaz de manipular, não é algo limitado pelo {{site.data.keyword.BluSoftlayer_full}}. Consulte a documentação do S.O. para conhecer os limites com relação ao número de compartilhamentos de arquivo que podem ser montados.

## Quantos compartilhamentos de arquivo são permitidos por tamanho do volume de arquivo? Quais são os compartilhamentos máximos de arquivo permitidos por tamanho do volume?
{: faq}

<table>
  <caption>A Tabela 1 mostra o número máximo de nós-i permitidos com base no tamanho do volume. Os tamanhos dos volumes estão na coluna à esquerda. O número de inodes e compartilhamentos de arquivo estão à direita.</caption>
  <thead>
    <tr>
      <th>Tamanho do volume</th>
      <th>Inodes e compartilhamentos de arquivo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>20 GB - 39 GB</td>
      <td>622.484</td>
    </tr>
    <tr>
      <td>40 GB - 79 GB</td>
      <td>1.245.084</td>
    </tr>          
    <tr>
      <td>80 GB - 99 GB</td>
      <td>2.490.263</td>
    </tr>          
    <tr>
      <td>100 GB - 249 GB</td>
      <td>3.112.863</td>
    </tr>          
    <tr>
      <td>250 GB - 499 GB</td>
      <td>7.782.300</td>
    </tr>          
    <tr>
      <td>500 GB - 999 GB</td>
      <td>15.564.695</td>
    </tr>
    <tr>
      <td>1 TB</td>
      <td>31.876.593</td>
    </tr>
    <tr>
      <td>2 TB</td>
      <td>63.753.186</td>
    </tr>
    <tr>
      <td>3 TB</td>
      <td>95.629.970</td>
    </tr>
    <tr>
      <td>4 TB - 12 TB</td>
      <td>127.506.359</td>
    </tr>
   </tbody>
</table>

## Medindo IOPS
{: faq}

O IOPS é medido com base em um perfil de carregamento de blocos de 16 KB com 50 por cento de leitura e 50 por cento de gravações aleatórias. As cargas de trabalho que diferirem desse perfil poderão enfrentar desempenho insatisfatório.

## O que acontece quando eu uso um tamanho de bloco menor para medir o desempenho?
{: faq}

O máximo de IOPS poderá ser obtido mesmo se você usar tamanhos de blocos menores. No entanto, o rendimento será menor nesse caso. Por exemplo, um volume com 6.000 IOPS tem o rendimento a seguir em vários tamanhos de bloco:

- 16 KB * 6.000 IOPS == ~93,75 MB/s
- 8 KB * 6.000 IOPS == ~46,88 MB/s
- 4 KB * 6.000 IOPS == ~23,44 MB/s


## O IOPS alocado é cumprido por instância ou por volume?
{: faq}

O IOPS é cumprido no nível de volume. Dito de forma diferente, dois hosts conectados a um volume com 6.000 IOPS compartilham esses 6.000 IOPS.

## O volume precisa ser pré-aquecido para alcançar o rendimento esperado?
{: faq}

Não há necessidade de pré-aquecimento. É possível observar o rendimento especificado imediatamente ao provisionar o volume.

## É possível ter mais rendimento caso uma conexão Ethernet mais rápida seja usada?
{: faq}

Os limites de rendimento são definidos em um nível por volume. Esse limite não pode ser aumentado usando uma conexão Ethernet mais rápida. No entanto, com uma conexão Ethernet mais lenta, sua largura da banda pode ser um gargalo potencial.

## Os firewalls e os grupos de segurança afetam o desempenho?
{: faq}

É melhor executar o tráfego de armazenamento em uma VLAN, que efetua bypass do firewall. A execução do tráfego de armazenamento por meio de firewalls de software aumenta a latência e prejudica o desempenho do armazenamento.

## Qual latência de desempenho pode ser esperada do {{site.data.keyword.filestorage_short}}?   
{: faq}

A latência de destino dentro do armazenamento é menor que um ms. O armazenamento é conectado a instâncias de cálculo em uma rede compartilhada, portanto, a latência exata de desempenho depende do tráfego de rede durante a operação.

## O que acontece com os dados quando os Volumes do {{site.data.keyword.filestorage_short}} são excluídos?
{: faq}

O {{site.data.keyword.filestorage_full}} apresenta compartilhamentos de arquivo para clientes no armazenamento físico que é limpo antes de qualquer reutilização. Os clientes com necessidades especiais de conformidade, como NIST 800-88 Diretrizes de Sanitização de Mídias, precisam executar o procedimento de sanitização de dados antes de excluir seu armazenamento.

## Quais versões do NFS são suportadas?
{: faq}

O NFS v3 e NFS v4.1 são suportados no ambiente do {{site.data.keyword.BluSoftlayer_full}}. 

O NFS v3 é a versão preferencial, pois é um protocolo sem preservação de estado e mais resiliente quando ocorrem eventos de rede. 

O NFS v3 suporta nativamente `no_root_squash`, o qual permite que os clientes raiz retenham as permissões raiz no compartilhamento do NFS. É possível ativar esse recurso no NFS v4.1 editando as informações de domínio e executando o `rpcidmapd` ou um serviço semelhante. Para obter mais informações, veja [Implementando no_root_squash para o NFS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux#norootsquash).

Quando se trata das Soluções vSphere, o NFS v3 suporta mais recursos do que a v4.1. Esses recursos incluem o Storage DRS e o Site Recovery Manager.


## O que acontece com as unidades que são desatribuídas do data center de nuvem?
{: faq}

Quando as unidades são desatribuídas, a IBM as destrói antes de elas serem descartadas. As unidades se tornam inutilizáveis. Quaisquer dados que foram gravados nessa unidade se tornam inacessíveis.
