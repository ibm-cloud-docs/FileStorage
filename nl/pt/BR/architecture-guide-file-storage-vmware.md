---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# Guia de arquitetura para o {{site.data.keyword.filestorage_short}} com VMware

As etapas a seguir podem ajudar você a pedir e configurar o {{site.data.keyword.filestorage_full}} em um ambiente do vSphere 5.5 e vSphere 6.0 no {{site.data.keyword.BluSoftlayer_full}}. Se você precisa de mais de oito conexões a seu host VMWare, usar o NFS {{site.data.keyword.filestorage_short}} é a melhor prática.

## Visão geral do {{site.data.keyword.filestorage_short}}

Nosso {{site.data.keyword.filestorage_short}} é projetado para suportar aplicativos de E/S alta que requerem níveis previsíveis de desempenho. O desempenho previsível é alcançado por meio da alocação de input/output operations per second (IOPS) de nível de protocolo para volumes individuais.

A oferta {{site.data.keyword.filestorage_short}} é acessada e montada por meio de uma conexão NFS. Em uma implementação do VMware, um único volume pode ser montado em até 64 hosts ESXi como armazenamento compartilhado ou é possível montar múltiplos volumes para criar um cluster de armazenamento para usar o vSphere Storage Distributed Resource Scheduling (DRS).

As opções de precificação e configuração para o {{site.data.keyword.filestorage_short}} Endurance e Performance são cobradas com base em uma combinação do espaço reservado e para o IOPS oferecido.

### 1. Considerações sobre {{site.data.keyword.filestorage_short}}

Ao pedir o {{site.data.keyword.filestorage_short}}, considere as informações a seguir:

- Quando você decidir sobre o tamanho, considere o tamanho da carga de trabalho e o rendimento necessário. O tamanho é importante com o serviço Endurance, que escala o desempenho linearmente em relação à capacidade (IOPS/GB). Por outro lado, o serviço Performance permite que o administrador escolha a capacidade e o desempenho independentemente. Os requisitos de rendimento são importantes com o Performance. <br/> **Nota**: o cálculo de rendimento é IOPS x 16 KB. O IOPS é medido com base em um tamanho de bloco de 16 KB com uma combinação de leitura/gravação 50/50. <br/> **Nota**: aumentar o tamanho de bloco aumentará o rendimento, mas diminuirá o IOPS. Por exemplo, dobrar o tamanho de bloco para blocos de 32 KB manterá o rendimento máximo, mas metade do IOPS.
- O NFS usa muitas operações de controle de arquivos adicionais como `lookup`, `getattr` e `readdir` para citar alguns. Essas operações, além das operações de leitura/gravação, podem contar como IOPS e variam por tipo de operação e versão do NFS.
- Tecnicamente, múltiplos volumes podem ser divididos juntos para alcançar IOPS superior e mais rendimento. No entanto, o VMware recomenda um único armazenamento de dados Virtual Machine File System (VMFS) por volume para evitar a degradação do desempenho.
- Os volumes do {{site.data.keyword.filestorage_short}} são expostos a dispositivos, sub-redes ou endereços IP autorizados.
- Os serviços de Captura instantânea e Replicação estão nativamente disponíveis somente em volumes do Endurance {{site.data.keyword.filestorage_short}}. O Performance {{site.data.keyword.filestorage_short}} não tem esses recursos.
- Para evitar a desconexão de armazenamento durante o failover do caminho, a {{site.data.keyword.IBM}} recomenda instalar as ferramentas VMWare, que configurarão um valor de tempo limite apropriado. Não há necessidade de mudar o valor, a configuração padrão é suficiente para assegurar que seu host VMWare não perderá a conectividade.
- O NFS v3 e NFS v4.1 são suportados no ambiente do {{site.data.keyword.BluSoftlayer_full}}. No entanto, a {{site.data.keyword.IBM}} recomenda que você use o NFS v3. Como o NFS v4.1 é um protocolo stateful (não stateless como o NFSv3), os problemas de protocolo podem ocorrer durante eventos de rede. O NFS v4.1 deve colocar em modo quiesce todas as operações e, em seguida, executar a recuperação de bloqueio. Enquanto essas operações estão acontecendo, interrupções podem ocorrer.

#### Matriz de suporte do recurso VMware do protocolo NFS.
<table>
  <caption>A Tabela 1 mostra os recursos do vSphere que se aplicam às duas versões diferentes do NFS</caption>
 <thead>
  <tr>
   <th>Recursos do vSphere</th>
   <th>NFS versão 3</th>
   <th>NFS versão 4.1</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vMotion e Storage vMotion</td>
   <td>Sim</td>
   <td>Sim</td>
  </tr>
  <tr>
   <td>Alta Disponibilidade (HA)</td>
   <td>Sim</td>
   <td>Sim</td>
  </tr>
  <tr>
   <td>Fault Tolerance (FT)</td>
   <td>Sim</td>
   <td>Sim</td>
  </tr>
  <tr>
   <td>Distributed Resource Scheduler (DRS)</td>
   <td>Sim</td>
   <td>Sim</td>
  </tr>
  <tr>
   <td>Host Profiles</td>
   <td>Sim</td>
   <td>Sim</td>
  </tr>
  <tr>
   <td>Storage DRS</td>
   <td>Sim</td>
   <td>Nenhuma</td>
  </tr>
  <tr>
   <td>Storage I/O Control</td>
   <td>Sim</td>
   <td>Nenhuma</td>
  </tr>
  <tr>
   <td>Site Recovery Manager</td>
   <td>Sim</td>
   <td>Nenhuma</td>
  </tr>
  <tr>
   <td>Virtual Volumes</td>
   <td>Sim</td>
   <td>Nenhuma</td>
  </tr>
 </tbody>
</table>
*Origem: [VMWare - Protocolos NFS e ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*





### 2. Capturas instantâneas do Endurance {{site.data.keyword.filestorage_short}}

O Endurance {{site.data.keyword.filestorage_short}} permite que os administradores configurem planejamentos de captura instantânea que criam e excluem cópias de captura instantânea automaticamente para cada volume de armazenamento. Eles também podem criar planejamentos de captura instantânea extra (por hora, diário, semanal) para capturas instantâneas automáticas e criar capturas instantâneas ad hoc manualmente para cenários de business continuity and disaster recovery (BCDR). Os alertas automáticos são entregues por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} ao proprietário do volume para as capturas instantâneas retidas e o espaço usado.

Esteja ciente de que o “espaço de captura instantânea” é necessário para usar capturas instantâneas. O espaço pode ser adquirido no pedido de volume inicial ou após o fornecimento inicial por meio da página **Detalhes do volume**, clicando em **Ações** e selecionando **Incluir espaço de captura instantânea**.

É importante observar que os ambientes VMware não estão cientes de capturas instantâneas. O recurso de captura instantânea do Endurance {{site.data.keyword.filestorage_short}} não deve ser confundido com capturas instantâneas do VMware. Qualquer recuperação que usa o recurso de captura instantânea do Endurance {{site.data.keyword.filestorage_short}} deve ser manipulada por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. 

A restauração do volume do Endurance {{site.data.keyword.filestorage_short}} requer o desligamento de todas as VMs que residem no Endurance {{site.data.keyword.filestorage_short}}. O volume precisa ser temporariamente desmontado dos hosts ESXi para evitar qualquer distorção de dados durante o processo.

Consulte o nosso artigo de [capturas instantâneas](snapshots.html) para obter mais detalhes sobre como configurar capturas instantâneas.


### 3. Replicação de armazenamento de arquivos

A replicação usa um de seus planejamentos de captura instantânea para copiar automaticamente capturas instantâneas para um volume de destino em um data center remoto. As cópias podem ser recuperadas no site remoto se um evento catastrófico ou distorção de dados ocorre.


Com réplicas, é possível

- Recuperar de falhas do site e outros desastres rapidamente efetuando failover para o volume de destino
- Efetuar failover para um momento específico na cópia de DR

Antes de replicar, deve-se criar um planejamento de captura instantânea. Ao efetuar failover, você está “invertendo o comutador” do volume de armazenamento em seu data center primário para o volume de destino em seu data center remoto. Por exemplo, seu data center primário é Londres e seu data center secundário é Amsterdã. Se um evento de falha ocorresse, você efetuaria failover para Amsterdã, conectando-se ao volume agora primário de uma instância de Cluster do vSphere em Amsterdã.

Depois que seu volume em Londres é reparado, uma captura instantânea é tomada do volume de Amsterdã. Em seguida, é possível efetuar failback para Londres e no volume novamente primário de uma instância de cálculo em Londres. 

Antes que o volume efetue failback novamente para o data center primário, ele precisa parar de ser usado no site remoto. Uma captura instantânea de quaisquer informações novas ou mudadas é tomada e replicada para o data center primário antes que elas possam ser montadas novamente em hosts ESXi do site de produção.

Consulte a página de informações [Replicação](replication.html) para obter mais detalhes sobre como configurar a replicação.

**Nota**: os dados inválidos, quer estejam corrompidos, hackeados ou infectados, serão replicados de acordo com o planejamento de captura instantânea e a retenção de captura instantânea. O uso das janelas de replicação menores pode fornecer um objetivo do ponto de recuperação melhor. No entanto, isso também pode fornecer menos tempo para reagir à replicação de dados inválidos.


## Pedir o {{site.data.keyword.filestorage_short}}

É possível pedir e configurar o {{site.data.keyword.filestorage_short}} para um ambiente VMware ESXi 5. Use as informações a seguir junto com a arquitetura de referência do Advanced Single-Site VMware para configurar uma dessas opções de armazenamento em seu ambiente VMware.


O {{site.data.keyword.filestorage_short}} pode ser pedido por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} acessando a página {{site.data.keyword.filestorage_short}} por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.


### 1. Pedindo o {{site.data.keyword.filestorage_short}}

Use as etapas a seguir para pedir o {{site.data.keyword.filestorage_short}}:
1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** na página inicial do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Clique em **Pedir o {{site.data.keyword.filestorage_short}}** na página **{{site.data.keyword.filestorage_short}}**.
3. Selecione **Endurance**/**Performance** na lista **Selecionar tipo de armazenamento**.
4. Selecione o local. Os data centers com recursos melhorados são denotados com um asterisco. Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host ESXi pedido anteriormente.
5. Selecione o método de Faturamento. As opções de faturamento Mensal e Por hora estão disponíveis.
6. Selecione a quantia de espaço de armazenamento em GBs. Para TB, 1 TB é igual a 1.000 GB e 12 TB é igual a 12.000 GB.
7. Insira a quantia de IOPS em intervalos de 100 ou selecione uma camada de IOPS.
8. Especifique o tamanho de Espaço de captura instantânea.
9. Clique em **Continuar**.
10. Insira um código promocional, se tiver um, e clique em **Recalcular**.
11. Revise sua ordem.
12. Marque a caixa de seleção **Eu li o contrato de prestação de serviços principal e concordo com os termos nesse documento**.
13. Clique em **Colocar ordem** para enviar a ordem ou **Cancelar** para fechar o formulário sem enviar uma ordem.

O armazenamento será provisionado em menos de um minuto e ficará visível na página **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



### 2. Autorizar hosts a usarem o {{site.data.keyword.filestorage_short}}

Quando um volume é provisionado, o {{site.data.keyword.BluBareMetServers_full}} ou {{site.data.keyword.BluVirtServers_full}} que usará o volume deve estar autorizado a acessar o armazenamento. Use as etapas a seguir para autorizar o volume:

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione **Acessar host** no menu **Ações de volume do Performance** ou **Endurance**.
3. Selecione o botão de opções **Subnets**
4. Escolha dentre as sub-redes disponíveis na lista que estão designadas às portas vmkernel nos hosts ESXi e clique em **Enviar**.<br/> **Nota**: as sub-redes exibidas serão sub-redes inscritas no mesmo data center que o volume de armazenamento.


Após as sub-redes serem autorizadas, anote o nome do host do servidor de armazenamento Endurance ou Performance que você deseja utilizar ao montar o volume. Essas informações podem ser localizadas na página de detalhes do {{site.data.keyword.filestorage_short}} clicando em um volume específico.


##  Configurar o host de máquina virtual VMware

### 1. Pré-requisitos

Antes de iniciar o processo de configuração do VMware, certifique-se de que os pré-requisitos a seguir sejam atendidos:

- Os {{site.data.keyword.BluBareMetServers}} com VMware ESXi são provisionados com a configuração de armazenamento adequada e as credenciais de login do ESXi.
- Os {{site.data.keyword.virtualmachinesshort}} ou físicos do Windows do {{site.data.keyword.BluSoftlayer_full}} no mesmo data center que o {{site.data.keyword.BluBareMetServers}}. Incluindo o endereço IP público da VM do Windows do {{site.data.keyword.BluSoftlayer_full}} e credenciais de login.
- Um computador com acesso à Internet e com o software de navegador da web e um cliente Remote Desktop Protocol (RDP) instalados.


### 2. Etapas de configuração de host VMware.

Para configurar o host virtual, conclua as etapas a seguir:

1. De um computador conectado à Internet, ative um cliente RDP e estabeleça uma sessão RDP para o {{site.data.keyword.BluVirtServers_full}} provisionado no mesmo data center no qual o vSphere vCenter está instalado.
2. Nos {{site.data.keyword.BluVirtServers_short}}, ative um navegador da web e conecte-se ao VMware vCenter por meio do vSphere Web Client.
3. Na tela **PÁGINA INICIAL**, selecione **Hosts e clusters**. Expanda o painel à esquerda e selecione o **Servidor VMware ESXi** que deve ser usado para essa implementação.
4. Certifique-se de que a porta de firewall para o cliente NFS esteja aberta em todos os hosts para configurar o cliente NFS no host vSphere. Ela é aberta automaticamente nas liberações mais recentes do vSphere. Para verificar se a porta está aberta, acesse a guia **Gerenciar do host ESXi** em VMware® vCenter™, selecione **Configurações** e, em seguida, selecione **Perfil de segurança**. Na seção **Firewall**, clique em **Editar** e role para baixo até **Cliente NFS**.
5. Certifique-se de que **Permitir conexão de qualquer endereço IP ou uma lista de endereços IP** seja fornecido. <br/>
   ![Permitir conexão](/images/1_4.png)
6. Configure quadros gigantes acessando a guia **Gerenciar do host ESXi**, selecione **Gerenciar** e, em seguida, **Rede**.
7. Selecione **Adaptadores VMkernel**, destaque o **vSwitch** e clique em **Editar**. (Ícone de lápis)
8. Selecione **Configuração de NIC** e assegure-se de que a MTU do NIC esteja configurada para 9000.
   - Se necessário, configure a MTU para 9000 e clique em **OK**.
9. Se desejado, as configurações de quadro gigante podem ser validadas como a seguir:
   - No Windows: ping -f -l 8972 a.b.c.d
   - No Unix: ping -s 8972 a.b.c.d
   Em que a.b.c.d é a interface vizinha de {{site.data.keyword.BluVirtServers_short}} com o comando:
   A saída parece semelhante a:
   ```
   ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
   8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
   ```

Mais informações sobre VMware e Quadros Gigantes podem ser localizadas [aqui](https://kb.vmware.com/s/article/1003712){:new_window}.


### 3. Incluindo um adaptador de uplink para um comutador virtual

1. Configure um novo adaptador de uplink acessando a guia **Gerenciar do host ESXi**, selecione **Gerenciar** e, em seguida, **Rede**.
2. Selecione a guia **Adaptadores físicos**
3. Clique em **Incluir rede de host**. (Ícone de globo com um sinal de mais)
4. Selecione o tipo de conexão como **Adaptador de rede física** e clique em **Avançar**.
5. Selecione o **vSwitch** existente e clique em **Avançar**.
6. Selecione **Adaptadores não usados** e clique em **Incluir adaptadores**. (Sinal de mais)
7. Clique no outro adaptador "Conectado" e clique em **OK**. <br/>
   ![Incluir adaptadores físicos no comutador](/images/2_3.png)
8. Clique em **Avançar** e **Concluir**.
9. Navegue de volta para a guia **Comutadores virtuais** e selecione o ícone superior **Editar configuração** sob o título **Comutadores virtuais**. (Ícone de lápis)
10. À esquerda, selecione a entrada **Equipe e failover** do vSwitch.
11. Verifique se a opção **Balanceamento de carga** está configurada para **Rotear com base na porta virtual de origem** e clique em **OK**.


### 4. Configurar o roteamento estático de ESXi (opcional)

A configuração de rede para este guia de arquitetura usa um número mínimo de grupos de portas. Se você tiver configurado um grupo de portas VMkernal para armazenamento NFS, etapas adicionais deverão ser executadas. Por padrão, o ESXi usará a porta vmkernel que está na mesma sub-rede que um volume NFS para montar o volume NFS no armazenamento do Endurance/Performance. Como estamos usando o roteamento de camada 3 para montar o volume NFS, devemos forçar o ESXi a usar a porta vmkernel que configuramos para montar o volume NFS. Para fazer isso, devemos criar uma rota estática para a matriz de armazenamento do Endurance/Performance.

1. Para configurar uma rota estática, execute o SSH para cada host ESXi utilizando o Performance ou Endurance e execute os comandos a seguir. Observe que deve-se tomar o comando ping do endereço IP resultante (primeiro comando) e usá-lo com o comando esxcli network mostrado abaixo:
   - `ping <hostname of the endurance/performance array>`

      **Observe** que o nome do host DNS do armazenamento NFS é um Forwarding Zone (FZ) designado a múltiplos endereços IP. Esses endereços IP são estáticos e pertencem a esse nome do host DNS específico.  Qualquer um desses endereços IP pode ser usado para acessar um volume específico.
   - `esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32`

2. Observe que as rotas estáticas não são persistentes entre as reinicializações no ESXi 5.0 e anterior. Para assegurar que quaisquer rotas estáticas incluídas sejam persistentes, esses comandos precisam ser incluídos no arquivo local.sh em cada host, localizado no diretório /etc/rc.local.d/. Para fazer isso, abra o arquivo local.sh usando o editor visual e inclua o comando acima para ser executado acima da saída de linha 0.
   - Anote o endereço IP, pois ele poderá ser usado para montar o volume na próxima etapa.
   - Esse processo precisa ser feito para cada volume NFS que você planeja montar em seu host ESXi.
   - Aqui está o link para um artigo de KB do VMware: [Configurando rotas estáticas para as portas do VMkernel no host ESXi](https://kb.vmware.com/s/article/2001426){:new_window}.


##  Montar os volumes do {{site.data.keyword.filestorage_short}} nos hosts ESXi

1. Clique no ícone **Acessar o vCenter** na parte superior da página da web e, em seguida, em **Hosts e clusters**.
2. Clique em **Armazenamentos de dados** sob a guia **Objeto relacionado**. Clique no ícone **Criar um novo armazenamento de dados**.
3. Na tela **Novo armazenamento de dados**, selecione o local do armazenamento de dados (seu host ESXi) e clique em **Avançar**.
4. Na tela **Tipo**, selecione o botão de opções **NFS** e clique em **Avançar**.
5. Na tela **Nome e configuração**, insira o nome como você deseja chamar o armazenamento de dados. Além disso, insira o nome do host do servidor NFS. Usar o FQDN para o servidor NFS produz a melhor distribuição de tráfego para o servidor subjacente. O endereço IP também é válido, mas usado menos frequentemente e somente em instâncias específicas. Insira o nome da pasta na forma de /foldername.
6. Na tela **Acessibilidade do host**, selecione todos os hosts em que você deseja montar o armazenamento de dados NFS e clique em **Avançar**.
7. Revise as entradas na próxima tela e clique em **Concluir**.
8. Repita para quaisquer volumes adicionais do {{site.data.keyword.filestorage_short}}.

**Nota**: é recomendação da {{site.data.keyword.BluSoftlayer_full}} que nomes FQDN sejam usados para se conectar ao armazenamento de dados. O uso de endereçamento IP direto pode estar efetuando bypass do mecanismo de balanceamento de carga fornecido usando FQDN. Para usar o endereço IP em vez do FQDN, execute o comando a seguir para obter o endereço IP.

  - `ping <hostname of the endurance/performance array>`
  - Em um host ESXi:
    ```
    ~ # vmkping nfsdal0902a-fz.service.softlayer.com
    PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
    64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
    10.2.125.80 é o endereço IP associado ao FQDN
    ```

## Ativar o Storage I/O Control do ESXi (opcional)

O Storage I/O Control (SIOC) é um recurso disponível para clientes que utilizam uma licença Enterprise Plus. Quando o SIOC for ativado no ambiente, ele mudará o comprimento da fila de dispositivo para a única VM. A mudança para o comprimento da fila de dispositivo reduz a fila de matriz de armazenamento de todas as VMs para um compartilhamento e regulador iguais da fila de armazenamento. O SIOC é encaixado somente se os recursos são restritos e a latência de E/S de armazenamento está acima de um limite definido.


Para que o SIOC determine quando um dispositivo de armazenamento está congestionado ou restrito, ele requer um limite definido. A latência de limite de congestionamento é diferente para tipos de armazenamento diferentes; as seleções padrão são para 90% de pico de rendimento. A porcentagem de valor de pico de rendimento indica o limite de latência estimado quando o armazenamento de dados está usando essa porcentagem de seu pico estimado de rendimento.


Deve ser observado que configurar incorretamente o SIOC para um armazenamento de dados ou para um VMDK pode afetar significativamente o desempenho.



### 1. Storage I/O Control para um armazenamento de dados

Use as etapas a seguir para ativar o SIOC com os valores recomendados para armazenamento do Endurance e Performance:

1. Navegue para o armazenamento de dados no navegador do vSphere Web Client.
2. Clique na guia **Gerenciar**.
3. Clique em **Configurações** e clique em **Geral**.
4. Clique em **Editar** para **Recursos de armazenamento de dados**.
5. Marque a caixa de seleção **Ativar Storage I/O Control**.<br/>
   ![NSFDataStore](/images/3_0.png)
6. Clique em **OK**.

**Nota**: esta configuração é específica para o armazenamento de dados e não para o host.


### 2. Storage I/O Control para {{site.data.keyword.BluVirtServers_short}}

Também é possível limitar discos virtuais individuais para VMs individuais ou conceder a eles compartilhamentos diferentes com SIOC. A limitação de discos e a concessão de compartilhamentos diferentes permite corresponder e alinhar o ambiente para a carga de trabalho com o número de IOPS do volume do {{site.data.keyword.filestorage_short}} adquirido. O limite é configurado por IOPS e é possível configurar um peso diferente ou "Compartilhamentos". Os discos virtuais com compartilhamentos configurados como Alto (2.000 compartilhamentos) recebem o dobro de E/S
que um disco configurado como Normal (1.000 compartilhamentos) e quatro vezes mais que um disco configurado
como Baixo (500 compartilhamentos). Normal é o valor padrão para todas as VMs, então você só precisa ajustar os valores acima ou abaixo do Normal para as VMs que realmente requerem isso.


Use as etapas a seguir para mudar os compartilhamentos de vDisk e o limite:

1. Escolha um dos {{site.data.keyword.BluVirtServers_short}} no inventário **VMs e modelos**. O ícone está na parte superior esquerda da página da web.
2. Selecione os {{site.data.keyword.BluVirtServers_short}} para controle de E/S.
3. Clique na guia **Gerenciar** e clique em **Configurações**. Clique em **Editar**.
4. Expanda a seta suspensa de Disco rígido. Modifique os compartilhamentos ou o Limite - IOPS conforme apropriado para seu ambiente. Escolha um disco rígido virtual na lista e modifique a seleção Compartilhamentos para escolher a quantia relativa de compartilhamentos para alocar para os {{site.data.keyword.BluVirtServers_short}} (Baixo, Normal ou Alto). Também é possível clicar em **Customizado** e insira um valor compartilhado definido pelo usuário.
5. Clique na coluna Limite - IOPS e insira o limite superior de recursos de armazenamento para alocar para a máquina virtual.
6. Clique em **OK**


   **Nota**: o processo acima é usado para configurar os limites de consumo de recurso de vDisks individuais em um dos {{site.data.keyword.BluVirtServers_short}} mesmo quando o SIOC não está ativado. Essas configurações são específicas para o guest individual e não para o host, embora elas sejam usadas por SIOC.





## Configurações do lado do host ESXi

Existem algumas configurações adicionais necessárias para configurar hosts ESXi 5.x para armazenamento NFS.

|Parâmetro | Configure para... |
|----------|------------|
|Net.TcpipHeapSize |	32 |
|Net.TcpipHeapMax |	Para vSphere 5.0/5.1, configure 128 <br/> Para vSphere 5.5 ou superior, configure 512 |
|NFS.MaxVolumes |	256 |
|NFS41.MaxVolumes |	256 (somente vSphere 6.0 ou mais recente) |
|NFS.HeartbeatMaxFailures |	10 |
|NFS.HeartbeatFrequency |	12 |
|NFS.HeartbeatTimeout |	5 |
|NFS.MaxQueueDepth|	64 |


- Usando a CLI no host ESXi 5.x host para parâmetros de configuração avançada:
   Os exemplos a seguir usam a CLI para configurar esses parâmetros de configuração avançada e, em seguida, verificá-los. A ferramenta esxcfg-advcfg usada nos exemplos pode ser localizada no diretório /usr/sbin nos hosts ESXi 5.x.

   - Configurando os parâmetros de configuração avançada por meio da CLI do ESXi 5.x:
   ```
   #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
   #esxcfg-advcfg -s 128 /Net/TcpipHeapMax (para vSphere 5.0/5.1)
   #esxcfg-advcfg -s 512 /Net/TcpipHeapMax (para vSphere 5.5 e acima)
   #esxcfg-advcfg -s 256 /NFS/MaxVolumes
   #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 e acima)
   #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
   #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout
   #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
   #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
   #esxcfg-advcfg -s 8 /Disk/QFullThreshold
   ```

- Verificando os parâmetros de configuração avançada da CLI do ESXi 5.x:
   ```
   #esxcfg-advcfg -g /Net/TcpipHeapSize
   #esxcfg-advcfg -g /Net/TcpipHeapMax
   #esxcfg-advcfg -g /NFS/MaxVolumes
   #esxcfg-advcfg -g /NFS41/MaxVolumes (ESXi 6.0 e acima)
   #esxcfg-advcfg -g /NFS/HeartbeatMaxFailures
   #esxcfg-advcfg -g /NFS/HeartbeatFrequency
   #esxcfg-advcfg -g /NFS/HeartbeatTimeout
   #esxcfg-advcfg -g /NFS/MaxQueueDepth
   #esxcfg-advcfg -g /Disk/QFullSampleSize
   #esxcfg-advcfg -g /Disk/QFullThreshold
   ```


## Ativando quadros gigantes no SoftLayer - Windows e Linux

O {{site.data.keyword.BluSoftlayer_full}} declarou que, para realizar plenamente as velocidades no armazenamento, os quadros gigantes precisam ser ativados em 9.000 MTU.


Um quadro gigante é um quadro Ethernet com uma carga útil maior que a unidade máxima de transmissão (MTU) padrão de 1.500 bytes. Os quadros gigantes são usados em redes locais que suportam pelo menos 1 Gbps e podem ser tão grandes quanto 9.000 bytes.


Os quadros gigantes precisam ser configurados da mesma maneira no caminho de rede inteiro por meio do dispositivo de origem <-> comutador <-> roteador <-> comutador <-> dispositivo de destino. Se a cadeia inteira não for configurada da mesma forma, ela assumirá por padrão a configuração mais baixa ao longo da cadeia. O {{site.data.keyword.BluSoftlayer_full}} tem seus dispositivos de rede configurados para 9.000 atualmente. Todos os dispositivos do cliente precisarão ser configurados da mesma forma.

### Windows

1. Abra o **Centro de rede e compartilhamento**.
2. Clique em **Mudar as configurações do adaptador**.
3. Clique com o botão direito no NIC para o qual você deseja ativar quadros gigantes e selecione **Propriedades**.
4. Na guia **Rede**, clique em **Configurar** para o adaptador de rede.
5. Selecione a guia **Avançado**.
6. Selecione **Quadro gigante** e mude o valor de desativado para o valor desejado, como MTU de 9 KB ou 9.014 Bytes, dependendo do NIC.
7. Clique em **OK** para todos os diálogos.

Observe que quando você fizer a mudança, o NIC perderá a conectividade de rede por alguns segundos. Também é necessário reinicializar para assegurar que a mudança entre em vigor.



### LINUX

1. Edite o arquivo de configuração de rede para a interface eth0 – por exemplo, /etc/sysconfig/network-script/ifcfg-eth0 (CentOS / RHEL / Fedora Linux):
`# vi /etc/sysconfig/network-script/ifcfg-eth0`

2. Anexe a diretiva de configuração a seguir, que especifica o tamanho do quadro em bytes:
MTU 9000

3. Feche e salve o arquivo. Reinicie a Interface eth0:
`# /etc/init.d/networking restart (isso causará uma breve perda de conectividade de rede)`


Uma nota sobre o usuário do Debian/Ubuntu Linux:
O usuário do Debian/Ubuntu Linux deve incluir MTU=9000 no arquivo de configuração /etc/network/interfaces.

Saiba mais sobre a arquitetura de referência do Advanced Single-Site VMware [aqui](https://console.bluemix.net/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
