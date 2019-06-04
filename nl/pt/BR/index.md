---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-22"

keywords: File Storage, Endurance, Performance, IOPS, replication, billing, file storage, NFS,

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
 {:shortdesc: .shortdesc}


# Sobre o {{site.data.keyword.filestorage_short}}
{: #about}

O {{site.data.keyword.cloud}}  {{site.data.keyword.filestorage_short}} é um {{site.data.keyword.filestorage_short}} baseado em NFS persistente, rápido e flexível, conectado à rede. Nesse ambiente de armazenamento conectado à rede (NAS), você tem controle total sobre sua função de compartilhamentos de arquivo e sobre o desempenho. Os compartilhamentos do {{site.data.keyword.filestorage_short}} podem ser conectados a até 64 dispositivos autorizados sobre conexões TCP/IP roteadas para resiliência.
{:shortdesc}

## Variáveis
{: #FileStorageFeatures}

O {{site.data.keyword.filestorage_short}} traz os melhores níveis de durabilidade e disponibilidade com um conjunto de recursos incomparável. O {{site.data.keyword.filestorage_short}} é construído usando padrões de mercado e melhores práticas, além de ser projetado para proteger a integridade dos dados. O {{site.data.keyword.filestorage_short}} mantém a disponibilidade por meio de eventos de manutenção e falhas não planejadas e fornece uma linha de base de desempenho consistente.

Tire vantagem dos recursos principais do {{site.data.keyword.filestorage_short}} a seguir:

- **Linha de base de desempenho consistente**
   - Fornecida por meio da alocação de operações de entrada/saída por segundo (IOPS) no nível de protocolo para volumes individuais.
- **{{site.data.keyword.filestorage_short}}**
   - Disponível para compartilhamentos de NFS baseados em arquivo.
- **Altamente durável e resiliente**
   - Protege a integridade dos dados e mantém a disponibilidade por meio de eventos de manutenção e falhas não planejadas sem a necessidade de criar e gerenciar matrizes Redundant Array of Independent Disks (RAID) no nível de sistema operacional
- **Criptografia de dados em repouso** [(disponível em data centers selecionados.)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - A criptografia gerenciada por provedor para dados em repouso é fornecida sem custo adicional
- **Todo armazenamento suportado por flash** [(disponível em data centers selecionados.)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - O armazenamento totalmente em flash para volumes pode ser provisionado em 2 IOPS/GB ou níveis mais altos.
- ** Capturas Instantâneas **  [ (Disponíveis em datacenters de seleção.)](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Captura capturas instantâneas de dados point-in-time sem interrupções.
- **Replicação**  [(Disponível em data centers selecionados.)](/docs/infrastructure/FileStorage?topic=FileStorage-news)
   - Disponível quando o armazenamento é provisionado em [data centers selecionados](/docs/infrastructure/FileStorage?topic=FileStorage-news).
   - Copia capturas instantâneas automaticamente para um data center parceiro do {{site.data.keyword.cloud}}.
- **Conectividade altamente disponível**
   - Usa conexões de rede redundantes para maximizar a disponibilidade.
   - Conexões TCP/IP  {{site.data.keyword.filestorage_short}}  roteadas por NFS.
- **Acesso simultâneo**
   - Permite que múltiplos hosts (até 64) acessem volumes de arquivo simultaneamente.
- **Bancos de dados em cluster**
   - Suporta casos de uso avançados, como bancos de dados em cluster.


## Fornecimento

Os volumes do {{site.data.keyword.filestorage_short}} podem ser fornecidos de 20 GB a 12 TB com duas opções: <br/>
- Provisiona camadas do **Endurance** que apresentam níveis de desempenho predefinidos e outros recursos, como capturas instantâneas e replicação.
- Construa um ambiente **Performance** de alta potência com input/output operations per second (IOPS) alocado.


### Provisionando com Camadas de Endurance

O {{site.data.keyword.filestorage_short}} Endurance está disponível em quatro camadas de desempenho do IOPS para suportar necessidades de aplicativo variadas. <br />

- **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos a qualquer momento. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.

- **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco da VM para um hypervisor.

- **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos a qualquer momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.

- **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível para o armazenamento provisionado até 4 TB em [data centers selecionados](/docs/infrastructure/FileStorage?topic=FileStorage-news) apenas.

Até 48.000 IOPS estão disponíveis com um volume do Endurance de 12 TB.

A escolha da camada correta do Endurance é essencial para sua carga de trabalho. É igualmente importante usar o tamanho de bloco, a velocidade de conexão Ethernet e o número de hosts corretos necessários para alcançar o máximo desempenho. Se alguma dessas partes não se alinhar, poderá haver um impacto significativo no rendimento resultante.

### Provisionando com Desempenho

Performance é uma classe do {{site.data.keyword.filestorage_short}} projetada para suportar aplicativos de alta E/S com requisitos de desempenho entendidos que não se ajustam bem em uma camada do Endurance. O desempenho previsível é alcançado por meio da alocação de IOPS de nível de protocolo para volumes individuais. Várias taxas de IOPS (100 - 48.000) podem ser provisionadas com tamanhos de armazenamento que variam de 20 GB a 12 TB.

O Performance para o {{site.data.keyword.filestorage_short}} é acessado e montado por meio de uma conexão do Network File System (NFS). O {{site.data.keyword.filestorage_short}} é usado geralmente quando o volume é acessado por múltiplos servidores simultaneamente. Os volumes consistentes do Performance podem ser pedidos de acordo com os Tamanhos e o IOPS na Tabela 1 e podem ser usados com os sistemas operacionais Linux.

| Tamanho (GB) | Mínimo de IOPS | Máximo de IOPS
|-----|-----|-----|
| 20 | 100 | 1.000 |
| 40 | 100 | 2.000  |
| 80 | 100 | 4.000 |
| 100 | 100 | 6.000 |
| 250 | 100 | 6.000 |
| 500 | 100  | 6.000 ou 10.000 |
| 1.000 | 100 | 6.000 ou 20.000 ![Nota de rodapé](/images/numberone.png) |
| 2.000 | 200 | 6.000 ou 40.000 ![Nota de rodapé](/images/numberone.png) |
| 3.000-7.000 | 300 | 6.000 ou 48.000 ![Nota de rodapé](/images/numberone.png) |
| 8.000-9.000 | 500 | 6.000 ou 48.000 ![Nota de rodapé](/images/numberone.png) |
| 10.000-12.000 | 1.000 | 6.000 ou 48.000 ![Nota de rodapé](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparação de tabela" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}


![Nota de rodapé](/images/numberone.png) *Os limites do IOPS maiores que 6.000 estão disponíveis em data centers selecionados.*

Os volumes do Performance foram projetados para operar consistentemente próximo ao nível de IOPS provisionado. A consistência facilita dimensionar e escalar ambientes de aplicativos com um nível específico de desempenho. Além disso, é possível otimizar um ambiente construindo um volume com a proporção preço/desempenho ideal.

## Faturamento

É possível selecionar o faturamento por hora ou mensal para um volume de Arquivo. O tipo de faturamento selecionado para um LUN aplica-se a seu espaço de captura instantânea e réplicas. Por exemplo, se você provisionar um LUN com o faturamento por hora, quaisquer capturas instantâneas ou taxas de réplica serão faturadas por hora. Se você provisionar um LUN com faturamento mensal, quaisquer capturas instantâneas ou taxas de réplica serão faturadas mensalmente.

 * Com o **faturamento por hora**, o número de horas em que o volume de arquivo existiu na conta é calculado no momento em que o LUN é excluído ou no final do ciclo de faturamento, o que ocorrer primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível para o armazenamento provisionado em [data centers selecionados](/docs/infrastructure/FileStorage?topic=FileStorage-news) apenas.

 * Com o **faturamento mensal**, o cálculo para o preço é rateado da data de criação ao término do ciclo de faturamento e faturado imediatamente. Se um volume for excluído antes do término do ciclo de faturamento, não haverá reembolso. O faturamento mensal é uma boa opção para o armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).


### Endurance
{: #pricing-comparison-endurance}

| Opções de precificação para camadas IOPS predefinidas | 0,25 IOPS | 2 IOPS/GB | 4 IOPS/GB | 10 IOPS/GB |
|-----|-----|-----|-----|-----|
| Preço mensal | US$ 0,06/GB | US$ 0,15/GB | $0,20/GB | $0,58/GB |
| Preço por hora | US$ 0,0001/GB | US$ 0,0002/GB | $0,0003/GB | $0,0009/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparação de tabela" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Desempenho
{: #pricing-comparison-performance}

| Opções de precificação para IOPS customizado | Cálculo de precificação |
|-----|-----|
| Preço mensal | $0,10/GB + $0,07/IOP |
| Preço por hora | $0,0001/GB + $0,0002/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparação de tabela" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
