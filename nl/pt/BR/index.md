---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Introdução ao {{site.data.keyword.filestorage_short}}

O {{site.data.keyword.filestorage_full}} é um {{site.data.keyword.filestorage_short}} baseado em NFS persistente, rápido e flexível, conectado à rede. Nesse ambiente de armazenamento conectado à rede (NAS), você tem controle total sobre sua função de compartilhamentos de arquivo e sobre o desempenho. Os compartilhamentos do {{site.data.keyword.filestorage_short}} podem ser conectados a até 64 dispositivos autorizados sobre conexões TCP/IP roteadas para resiliência.

O {{site.data.keyword.filestorage_short}} traz os melhores níveis de durabilidade e disponibilidade com um conjunto de recursos incomparável. O {{site.data.keyword.filestorage_short}} é construído usando padrões de mercado e melhores práticas, além de ser projetado para proteger a integridade dos dados. O {{site.data.keyword.filestorage_short}} mantém a disponibilidade por meio de eventos de manutenção e falhas não planejadas e fornece uma linha de base de desempenho consistente.

Tire vantagem dos recursos principais do {{site.data.keyword.filestorage_short}} a seguir:

- **Linha de base de desempenho consistente**
   - Fornecida por meio da alocação de operações de entrada/saída por segundo (IOPS) no nível de protocolo para volumes individuais.
- **{{site.data.keyword.filestorage_short}}**
   - Disponível para compartilhamentos de NFS baseados em arquivo.
- **Altamente durável e resiliente**
   - Protege a integridade dos dados e mantém a disponibilidade por meio de eventos de manutenção e falhas não planejadas sem a necessidade de criar e gerenciar matrizes Redundant Array of Independent Disks (RAID) no nível de sistema operacional
- **Criptografia de dados em repouso** [(disponível em data centers selecionados.)](new-ibm-block-and-file-storage-location-and-features.html)
   - A criptografia gerenciada por provedor para dados em repouso é fornecida sem custo adicional
- **Todo armazenamento suportado por flash** [(disponível em data centers selecionados.)](new-ibm-block-and-file-storage-location-and-features.html)
   - O armazenamento totalmente em flash para volumes pode ser provisionado em 2 IOPS/GB ou níveis mais altos.
- ** Capturas Instantâneas **  [ (Disponíveis em data centers selecionados.)](new-ibm-block-and-file-storage-location-and-features.html).
   - Captura capturas instantâneas de dados point-in-time sem interrupções.
- **Replicação** [(Disponível nos data centers selecionados.)](new-ibm-block-and-file-storage-location-and-features.html)
   - Disponível quando o armazenamento é provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html).
   - Copia capturas instantâneas automaticamente para um data center parceiro do {{site.data.keyword.BluSoftlayer_full}}.
- **Conectividade altamente disponível**
   - Usa conexões de rede redundantes para maximizar a disponibilidade.
   - Conexões TCP/IP  {{site.data.keyword.filestorage_short}}  roteadas por NFS.
- **Acesso simultâneo**
   - Permite que múltiplos hosts (até 64) acessem volumes de arquivo simultaneamente.
- **Bancos de dados em cluster**
   - Suporta casos de uso avançados, como bancos de dados em cluster.

## Faturamento

É possível selecionar o faturamento por hora ou mensal para um volume de Arquivo. O tipo de faturamento selecionado para um LUN aplica-se a seu espaço de captura instantânea e réplicas. Por exemplo, se você provisionar um LUN com o faturamento por hora, quaisquer capturas instantâneas ou taxas de réplica serão faturadas por hora. Se você provisionar um LUN com faturamento mensal, quaisquer capturas instantâneas ou taxas de réplica serão faturadas mensalmente.

Com o **faturamento por hora**, o número de horas em que o volume de arquivo existiu na conta é calculado no momento em que o LUN é excluído ou no final do ciclo de faturamento, o que ocorrer primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível para o armazenamento provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) apenas.

Com o **faturamento mensal**, o cálculo para o preço é rateado da data de criação ao término do ciclo de faturamento e faturado imediatamente. Se um volume for excluído antes do término do ciclo de faturamento, não haverá reembolso. O faturamento mensal é uma boa opção para o armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).


**Performance**
<table>
  <caption>A Tabela 1 está mostrando os preços para o armazenamento do Performance com faturamento mensal e por hora.</caption>
  <tr>
   <th>Preço mensal</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>A Tabela 2 está mostrando os preços para o armazenamento do Endurance para cada camada com opções de faturamento mensais e por hora.</caption>
  <tr>
   <th>Camada de IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Preço mensal</th>
   <td>US$ 0,06/GB</td>
   <td>US$ 0,15/GB</td>
   <td>$0,20/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>US$ 0,0001/GB</td>
   <td>US$ 0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0009/GB</td>
  </tr>
</table>



## Fornecimento

Os volumes do {{site.data.keyword.filestorage_short}} podem ser provisionados de 20 GB a 12 TB com duas opções: <br/>
- Provisiona camadas do **Endurance** que apresentam níveis de desempenho predefinidos e outros recursos, como capturas instantâneas e replicação.
- Construa um ambiente **Performance** de alta potência com input/output operations per second (IOPS) alocado.


### Provisionando com Camadas de Endurance

O {{site.data.keyword.filestorage_short}} Endurance está disponível em quatro camadas de desempenho do IOPS para suportar necessidades de aplicativo variadas. <br />

- **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos a qualquer momento. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.

- **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco da VM para um hypervisor.

- **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos a qualquer momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.

- **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível para o armazenamento provisionado até 4 TB em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) apenas.

Até 48.000 IOPS estão disponíveis com um volume do Endurance de 12 TB.

A escolha da camada correta do Endurance é essencial para sua carga de trabalho. É igualmente importante usar o tamanho de bloco, a velocidade de conexão Ethernet e o número de hosts corretos necessários para alcançar o máximo desempenho. Se alguma dessas partes não se alinhar, poderá haver um impacto significativo no rendimento resultante.

### Provisionando com Desempenho

Performance é uma classe do {{site.data.keyword.filestorage_short}} projetada para suportar aplicativos de alta E/S com requisitos de desempenho entendidos que não se ajustam bem em uma camada do Endurance. O desempenho previsível é alcançado por meio da alocação de IOPS de nível de protocolo para volumes individuais. Várias taxas de IOPS (100 - 48.000) podem ser provisionadas com tamanhos de armazenamento que variam de 20 GB a 12 TB.

O Performance para o {{site.data.keyword.filestorage_short}} é acessado e montado por meio de uma conexão do Network File System (NFS). O {{site.data.keyword.filestorage_short}} é usado geralmente quando o volume é acessado por múltiplos servidores simultaneamente. Os volumes consistentes do Performance podem ser pedidos de acordo com os Tamanhos e o IOPS na Tabela 1 e podem ser usados com os sistemas operacionais Linux.

<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>A Tabela 3 está mostrando combinações de tamanho e de IOPS para armazenamento do Performance.<br/><sup><img src="/images/numberone.png" alt="Nota de rodapé" /></sup> Os limites de IOPS maiores que 6.000 estão disponíveis nos data centers selecionados.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
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
            <td>6.000 ou 20.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
          <tr>
            <td>2.000 - 3.000</td>
            <td>200</td>
            <td>6.000 ou 40.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
          <tr>
            <td>4.000 - 7.000</td>
            <td>300</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="Nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>8.000 - 9.000</td>
            <td>500</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="Nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>10.000 - 12.000</td>
            <td>1.000</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="Nota de rodapé" /></sup></td>
          </tr>
</table>


Os volumes do Performance foram projetados para operar consistentemente próximo ao nível de IOPS provisionado. A consistência facilita dimensionar e escalar ambientes de aplicativos com um nível específico de desempenho. Além disso, é possível otimizar um ambiente construindo um volume com a proporção preço/desempenho ideal.

### Considerações de fornecimento

** Tamanho do Bloco **

O IOPS para o Endurance e o Performance tem como base um tamanho de bloco de 16 KB com uma carga de trabalho aleatória de 50 por cento, 50/50 de leitura/gravação. Um bloco de 16 KB equivale a uma gravação no volume.
{:important}

O tamanho do bloco usado por seu aplicativo afetará diretamente o desempenho do armazenamento. Se o tamanho do bloco usado por seu aplicativo for menor que 16 KB, o limite do IOPS será realizado antes do limite do rendimento. Por outro lado, se o tamanho do bloco usado por seu aplicativo for maior que 16 KB, o limite de rendimento será realizado antes do limite do IOPS.

<table>
  <caption>A Tabela 4 mostra exemplos de como o tamanho do bloco e o IOPS afetam o rendimento.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Tamanho de bloco (KB)</th>
            <th>IOPS</th>
            <th>Rendimento (MB/s)</th>
          </tr>
        </thead>
        <tbody>
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
            <td>32 (típico para o SQL Server)</td>
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

** Hosts autorizados **

Outro fator a ser considerado é o número de hosts que estão usando seu volume. Se houver um único host acessando o volume, poderá ser difícil realizar o IOPS máximo disponível, especialmente em contagens extremas de IOPS (10.000s). Se a sua carga de trabalho requerer alto rendimento, será melhor configurar pelo menos alguns servidores para acessar seu volume para evitar um gargalo de servidor único.

** Conexão de rede **

A velocidade da sua conexão Ethernet deve ser mais rápida do que o rendimento máximo esperado de seu volume. Em geral, não espere saturar sua conexão Ethernet além de 70% da largura de banda disponível. Por exemplo, se você tiver 6.000 IOPS e estiver usando um tamanho de bloco de 16 KB, o volume poderá manipular o rendimento de aproximadamente 94 MBps. Se você tiver uma conexão Ethernet de 1 Gbps com seu LUN, ela se tornará um gargalo quando seus servidores tentarem usar o rendimento máximo disponível. Isso porque 70 por cento do limite teórico de uma conexão Ethernet de 1 Gbps (125 MB por segundo) permitiria 88 MB por segundo apenas.

Para alcançar o IOPS máximo, recursos de rede adequados precisam estar em vigor. Outras considerações incluem o uso de rede privada fora do armazenamento e do lado do host, além de ajustes específicos do aplicativo (pilha de IP ou [profundidades de fila](set-host-queue-depth-settings-performance-and-endurance-storage.html) e outras configurações).

O tráfego de armazenamento é incluído no uso total de rede de Virtual Servers Públicos. Para obter mais informações sobre os limites que podem ser impostos pelo serviço, consulte a [Documentação do Virtual Server](https://{DomainName}/docs/vsi/vsi_public.html#public-virtual-servers).

** Versão do NFS **

O NFS v3 e NFS v4.1 são suportados no ambiente do {{site.data.keyword.BluSoftlayer_full}}. No entanto, o NFS v3 é preferencial porque o NFS v4.1 é um protocolo stateful (não stateless como o NFSv3) e problemas de protocolo podem ocorrer durante eventos de rede. O NFS v4.1 deve colocar em modo quiesce todas as operações e, em seguida, concluir a recuperação de bloqueio. Em um servidor de arquivos NFS relativamente ocupado, a latência aumentada pode causar interrupções. A falta de caminhos múltiplos e entroncamento do NFS v4.1 também pode estender a recuperação das operações do NFS.

## Enviando sua Ordem

Quando você estiver pronto para enviar seu pedido, poderá fazer isso por meio
do [Console](provisioning-block_storage.html) ou da [SLCLI](ordering-through-cli.html). Para ver Provisionando o File Storage com o VMware, clique [aqui](architecture-guide-file-storage-vmware.html)

## Conectando seu novo armazenamento

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional de seu host, siga o link apropriado.
- [Acessando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com o cPanel](configure-backup-cpanel.html)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com o Plesk](configure-backup-plesk.html)
- [Montando o volume do {{site.data.keyword.filestorage_short}} em hosts ESXi](architecture-guide-file-storage-vmware.html)
