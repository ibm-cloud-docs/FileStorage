---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Introdução ao {{site.data.keyword.filestorage_short}}

O {{site.data.keyword.filestorage_full}} é um {{site.data.keyword.filestorage_short}} baseado no NFS, persistente, rápido e conectado por rede flexível. Nesse ambiente Network Attached Storage (NAS), você tem controle total sobre sua função de compartilhamentos de arquivo e desempenho. Os compartilhamentos do {{site.data.keyword.filestorage_short}} podem ser conectados a até 64 dispositivos autorizados sobre conexões TCP/IP roteadas para resiliência.

O {{site.data.keyword.filestorage_short}} traz os melhores níveis da classe de durabilidade e disponibilidade com um conjunto de recursos não correspondidos e é construído usando os padrões de mercado e as melhores práticas e projetado para proteger a integridade dos dados e manter a disponibilidade por meio de eventos de manutenção e falhas não planejadas enquanto fornece uma linha de base de desempenho consistente.

Tire vantagem dos recursos principais do {{site.data.keyword.filestorage_short}} a seguir:

- **Linha de base de desempenho consistente**
   - Fornecido pela alocação de input/output operations per second (IOPS) de nível de protocolo para volumes individuais
- **{{site.data.keyword.filestorage_short}}**
   - Disponível para compartilhamentos NFS baseados em arquivo
- **Altamente durável e resiliente**
   - Protege a integridade dos dados e mantém a disponibilidade por meio de eventos de manutenção e falhas não planejadas sem a necessidade de criar e gerenciar matrizes Redundant Array of Independent Disks (RAID) de nível de sistema operacional
- **Criptografia de dados em repouso** [(disponível em data centers selecionados.)](new-ibm-block-and-file-storage-location-and-features.html)
   - Criptografia gerenciada por provedor para dados em repouso sem nenhum custo adicional
- **Todo armazenamento suportado por flash** [(disponível em data centers selecionados.)](new-ibm-block-and-file-storage-location-and-features.html)
   - Todo armazenamento flash para volumes provisionados com Endurance ou Performance em 2 IOPS/GB ou superior
- **Capturas instantâneas (quando provisionado com Endurance ou Performance em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html))**.
   - Captura as capturas instantâneas de dados de um momento sem interrupção
- **Replicação** (quando provisionado com Endurance ou Performance em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html).
   - Copia capturas instantâneas automaticamente para um data center do {{site.data.keyword.BluSoftlayer_full}} de parceiro.
- **Conectividade altamente disponível**
   - Usa conexões de rede redundantes para maximizar a disponibilidade - conexão TCP/IP roteada pelo {{site.data.keyword.filestorage_short}} com base em NFS
- **Acesso simultâneo**
   - Permite que múltiplos hosts (até 64) acessem volumes de arquivo simultaneamente
- **Bancos de dados em cluster**
   - Suporta casos de uso avançados, como bancos de dados em cluster

## Faturamento por hora/mensal

É possível selecionar o faturamento por hora ou mensal para um Volume de arquivo. O tipo de faturamento selecionado para um LUN será aplicado ao seu espaço de captura instantânea e réplicas. Por exemplo, se você provisionar um LUN com o faturamento por hora, quaisquer capturas instantâneas ou taxas de réplica serão faturadas por hora. Se você provisionar um LUN com faturamento mensal, quaisquer capturas instantâneas ou taxas de réplica serão faturadas mensalmente. 

Com o **faturamento por hora**, o cálculo do número de horas que o volume de arquivo existiu na conta é executado no momento em que o volume é excluído ou no término do ciclo de faturamento, aquele que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

Com o **faturamento mensal**, o cálculo para o preço é rateado da data de criação ao término do ciclo de faturamento e faturado imediatamente. Não haverá reembolso se um volume de arquivo for excluído antes do término do ciclo de faturamento.  O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).

 
### Performance:
<table>
 <tbody>
  <tr>
   <th>Preço mensal</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Endurance:
<table>
 <tbody>
  <tr>
   <th>Camada de IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Preço mensal</th>
   <td>$0,10/GB</td>
   <td>$0,20/GB</td>
   <td>$0,35/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0005/GB</td>
   <td>$0,0009/GB</td>
  </tr>
  </tbody>
</table>

 

## Fornecimento

Os volumes do {{site.data.keyword.filestorage_short}} podem ser provisionados de 20 GB a 12 TB com duas opções para fornecimento: <br/>
- Provisione com **camadas do Endurance** que apresentam níveis de desempenho e recursos predefinidos, como capturas instantâneas e replicação.
- Construa um ambiente **Performance** de alta potência com input/output operations per second (IOPS) alocado.

 
### Camadas do Endurance

Ao pedir com o Endurance, escolha entre múltiplas camadas de desempenho para suportar necessidades variadas do aplicativo.

O Endurance está disponível em três camadas de desempenho de IOPS para suportar necessidades variadas do aplicativo.

- **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho são caracterizadas geralmente por ter uma grande porcentagem de dados inativos em um determinado momento. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.
- **2 IOPS por GB** foi projetado para uso de propósito mais geral. Aplicativos de exemplo incluem a hospedagem de pequenos bancos de dados que suportam aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
- **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho são caracterizadas geralmente por ter uma alta porcentagem de dados ativos em um determinado momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.
- **10 IOPS por GB** foi projetado para as cargas de trabalho de maior demanda, como aquelas criadas por Bancos de dados NoSQL e processamento de dados para Analytics.  Essa camada está disponível para armazenamento provisionado de até 4 TB de tamanho em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html).

Até 48.000 IOPS estão disponíveis com um volume do Endurance de 12 TB.


Embora escolher a camada correta do Endurance {{site.data.keyword.filestorage_short}} para sua carga de trabalho seja fundamental, é igualmente importante usar o tamanho de bloco, a velocidade de conexão Ethernet e o número de hosts necessários para alcançar o desempenho máximo. Se alguma dessas partes não se alinhar com a outra, isso poderá ter um impacto significativo no rendimento resultante
 
### Performance

O Performance é uma classe de {{site.data.keyword.filestorage_full}} que é projetada para suportar aplicativos de E/S alta com requisitos de desempenho bem entendidos que não se ajustam bem dentro de uma camada do Endurance. O desempenho previsível é alcançado por meio da alocação de IOPS de nível de protocolo para volumes individuais. O IOPS que varia de 100 a 6.000 pode ser provisionado com tamanhos de armazenamento que variam de 20 GB a 12 TB. 

O Performance para o {{site.data.keyword.filestorage_short}} é acessado e montado por meio de uma conexão do Network File System (NFS). O {{site.data.keyword.filestorage_short}} é geralmente usado quando o volume será acessado por múltiplas máquinas simultaneamente. Os volumes consistentes do Performance podem ser pedidos de acordo com os Tamanhos e o IOPS na Tabela 1 e podem ser usados com os sistemas operacionais Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Tamanho (GB)</th>
            <th>Mínimo de IOPS</th>
            <th>Máximo de IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 ou 10.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 ou 20.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>2.000 - 3.000</td>
            <td>200</td>
            <td>6.000 ou 40.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>4.000 - 7.000</td>
            <td>300</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>8.000 - 9.000</td>
            <td>500</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>10.000 - 12.000</td>
            <td>1.000</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![nota de rodapé](/images/numberone.png)</sup> O limite de IOPS acima de 6.000 está disponível em data centers selecionados.


Os volumes do Performance são projetados para executar consistentemente próximo ao nível de IOPS provisionado. A consistência torna mais fácil dimensionar e escalar ambientes de aplicativos com um determinado nível de desempenho. Além disso, dado o intervalo de tamanhos de volume e as contagens de IOPS, torna-se possível otimizar um ambiente construindo um volume com a razão de preço ideal para desempenho.

O IOPS, para Endurance e Performance, é medido com base em um tamanho de bloco de 16 KB com uma combinação de leitura/gravação 50/50. Para alcançar o máximo de IOPS em um volume, recursos de rede adequados precisam estar em vigor. Outras considerações incluem o uso da rede privada fora do armazenamento e do lado do host e dos ajustes específicos do aplicativo (pilha IP, profundidades da fila, e assim por diante). 

## Dicas para provisionar o IOPS para {{site.data.keyword.filestorage_short}}

O IOPS para Endurance e Performance é baseado em um tamanho de bloco de 16 KB com uma carga de trabalho aleatória de 50% de leitura/gravação 50/50. Um bloco de ~16 KB é o equivalente de uma gravação para o volume.

O tamanho de bloco usado por seu aplicativo afetará diretamente o desempenho de armazenamento. Se o tamanho de bloco usado por seu aplicativo for menor que 16 KB, o limite de IOP será realizado antes do limite de rendimento. Por outro lado, se o tamanho de bloco usado por seu aplicativo for maior que 16 KB, o limite de rendimento será realizado antes do limite de IOP.

Mudar o tamanho de bloco afetará o desempenho como a seguir:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Tamanho de bloco (KB)</th>
            <th>IOPS</th>
            <th>Rendimento (MB/s)</th>
          </tr>
          <tr>
            <td>4 (típico para Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (típico para Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (típico para SQLServer)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

A escolha do {{site.data.keyword.blockstorageshort}} correto para sua carga de trabalho é importante e igualmente importante é como evitar gargalos. A velocidade da sua conexão Ethernet deve ser mais rápida do que o rendimento máximo esperado de seu volume. Como uma regra geral, você não deve esperar saturar a sua conexão Ethernet além de 70% da largura da banda disponível. Por exemplo, se você tiver 6.000 IOPS e estiver usando um tamanho de bloco de 16 KB, o volume será capaz de aproximadamente 94 MB por segundo. Se você tiver uma conexão Ethernet de 1 Gbps para o LUN, ela se tornará um gargalo quando seus servidores tentarem utilizar o rendimento máximo disponível porque 70% do limite teórico de uma conexão Ethernet de 1 Gbps (125 MB por segundo) permitiria somente para 88 MB por segundo.


Outro fator a considerar é o número de hosts que estão utilizando o volume. Se houver um único host que está acessando o volume, poderá ser difícil realizar o IOPS máximo disponível, especialmente em contagens extremas de IOPS (10.000s). Se sua carga de trabalho requer alto rendimento, é melhor para configurar pelo menos dois ou três servidores para acessar seu volume para evitar um gargalo de servidor único.


Para alcançar o IOPS máximo, recursos de rede adequados precisam estar em vigor. Outras considerações incluem o uso da rede privada fora do armazenamento e do lado do host e dos ajustes específicos do aplicativo (pilha IP, profundidades da fila, e assim por diante).

O NFS v3 e NFS v4.1 são suportados no ambiente do {{site.data.keyword.BluSoftlayer_full}}. No entanto, é nossa recomendação que o NFS v3 seja usado. O NFS v4.1 é um protocolo stateful (não stateless como o NFSv3) e, portanto, problemas de protocolo podem ocorrer durante eventos de rede. O NFS v4.1 deve colocar as operações em modo quiesce e, em seguida, executar a recuperação de bloqueio. Em um servidor de arquivos NFS relativamente ocupado, a latência aumentada pode causar interrupções. A falta de caminhos múltiplos/entroncamento do NFS v4.1 também pode estender a recuperação de operações do NFS.
