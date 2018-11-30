---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:pre: .pre}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Provisionando  {{site.data.keyword.filestorage_short}}  com VMware

As etapas a seguir podem ajudar você a pedir e configurar o {{site.data.keyword.filestorage_full}} em um ambiente do vSphere 5.5 e vSphere 6.0 no {{site.data.keyword.BluSoftlayer_full}}.

O {{site.data.keyword.filestorage_short}} foi projetado para suportar aplicativos de alta E/S que requerem níveis previsíveis de desempenho. O desempenho previsível é alcançado por meio da alocação de input/output operations per second (IOPS) de nível de protocolo para volumes individuais.

Se você precisar de mais de oito conexões para o host VMware, então, escolher o NFS {{site.data.keyword.filestorage_short}} será a melhor prática.
{:tip}

A oferta {{site.data.keyword.filestorage_short}} é acessada e montada por meio de uma conexão NFS. Em uma implementação do VMware, um único volume pode ser montado para até 64 hosts ESXi como armazenamento compartilhado. Também é possível montar múltiplos volumes para criar um cluster de armazenamento para usar o vSphere Storage Distributed Resource Scheduler (DRS).

As opções de precificação e configuração para o {{site.data.keyword.filestorage_short}} Endurance e Performance são cobradas com base em uma combinação do espaço reservado e do IOPS oferecido.

## Considerações de Solicitação

Ao pedir o {{site.data.keyword.filestorage_short}}, considere as informações a seguir:

- Quando você decidir sobre o tamanho, considere o tamanho da carga de trabalho e o rendimento necessário. O tamanho é importante com o serviço Endurance, que escala o desempenho linearmente em relação à capacidade (IOPS/GB). Por outro lado, o serviço Performance permite que o administrador escolha a capacidade e o desempenho independentemente. Os requisitos de rendimento são importantes com o Performance.

  O cálculo de rendimento é IOPS x 16 KB. O IOPS é medido com base em um tamanho de bloco de 16 KB com uma combinação de leitura/gravação 50/50.<br/>Aumentar o tamanho do bloco aumenta o rendimento, mas diminui o IOPS. Por exemplo, dobrar o tamanho do bloco para blocos de 32 KB mantém o rendimento máximo, mas diminui pela metade o IOPS.
  {:note}
- O NFS usa muitas operações de controle de arquivo extras, como `lookup`, `getattr` e `readdir`. Essas operações, além das operações de leitura/gravação, podem contar como IOPS e variam por tipo de operação e versão do NFS.
- Os volumes do {{site.data.keyword.filestorage_short}} são expostos a dispositivos, sub-redes ou endereços IP autorizados.
- Para evitar a desconexão do armazenamento durante o failover do caminho, a {{site.data.keyword.IBM}} recomenda a instalação das ferramentas do VMware, que configuram um valor de tempo limite apropriado. Não há necessidade de mudar o valor, a configuração padrão é suficiente para assegurar que o host do VMware não perca a conectividade.
- Tanto o NFSv3 quanto o NFSv4.1 são suportados no ambiente do {{site.data.keyword.BluSoftlayer_full}}. No entanto, a {{site.data.keyword.IBM}} sugere que você use o NFSv3. Como o NFSv4.1 é um protocolo stateful (não um protocolo sem preservação de estado, como o NFSv3), podem ocorrer problemas de protocolo durante eventos de rede. O NFSv4.1 deve colocar em modo quiesce todas as operações e, em seguida, concluir a solicitação de bloqueio. Enquanto essas operações estão acontecendo, interrupções podem ocorrer.

Para obter mais informações, veja o White Paper do VMware em [Best Practices for running VMware vSphere on Network Attached Storage] (https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/vmware-nfs-bestpractices-white-paper-en.pdf){:new_window}
{:tip}

** Matriz de suporte do recurso do VMware Protocol VMware **
<table>
  <caption>A Tabela 1 mostra os recursos do vSphere conforme se aplicam às duas versões diferentes do NFS.</caption>
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
*Fonte - [VMware - Protocolos NFS e ESXi](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.storage.doc/GUID-8A929FE4-1207-4CC5-A086-7016D73C328F.html){:new_window}*



### Usando capturas instantâneas

O {{site.data.keyword.filestorage_short}} permite que os administradores configurem programações de capturas instantâneas que criam e excluem cópias de capturas instantânea automaticamente para cada volume de armazenamento. Eles também podem criar planejamentos de captura instantânea extra (por hora, diário, semanal) para capturas instantâneas automáticas e criar capturas instantâneas ad hoc manualmente para cenários de business continuity and disaster recovery (BCDR). Os alertas automáticos são entregues por meio do [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} ao proprietário do volume para as capturas instantâneas retidas e o espaço usado.

O espaço de captura instantânea é necessário para usar capturas instantâneas. O espaço pode ser comprado no pedido de volume inicial ou após o fornecimento inicial por meio da página **Detalhes do volume** clicando em **Ações** e selecionando **Incluir espaço de captura instantânea**.

É importante observar que os ambientes VMware não estão cientes de capturas instantâneas. O recurso de captura instantânea do Endurance {{site.data.keyword.filestorage_short}} não deve ser confundido com capturas instantâneas do VMware. Qualquer recuperação que use o recurso de captura instantânea do {{site.data.keyword.filestorage_short}} deve ser manipulada por meio do [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

A restauração do volume do {{site.data.keyword.filestorage_short}} requer o desligamento de todas as MVs do {{site.data.keyword.filestorage_short}}. O volume precisa ser temporariamente desmontado dos hosts ESXi para evitar qualquer distorção de dados durante o processo.

Para obter mais detalhes sobre como configurar Capturas instantâneas, consulte o artigo sobre [capturas instantâneas](snapshots.html).


### Usando replicação

A replicação usa um de seus planejamentos de captura instantânea para copiar automaticamente capturas instantâneas para um volume de destino em um data center remoto. As cópias podem ser recuperadas no site remoto se um evento catastrófico ou distorção de dados ocorre.

Com réplicas, é possível

- Recuperar de falhas do site e outros desastres rapidamente efetuando failover para o volume de destino
- Efetuar failover para um momento específico na cópia de DR

A replicação mantém seus dados em sincronização em dois locais diferentes. Se desejar clonar seu volume e usá-lo independentemente do volume original, consulte [Criando um volume de arquivo duplicado](how-to-create-duplicate-volume.html).
{:tip}

Antes de replicar, deve-se criar um planejamento de captura instantânea.

Ao efetuar failover, você está “invertendo o comutador” do volume de armazenamento em seu data center primário para o volume de destino em seu data center remoto. Por exemplo, seu data center primário está em Londres e seu data center secundário está em Amsterdã. Se ocorrer um evento de falha, você executará failover em Amsterdã. Isso significa conectar-se ao volume agora primário de uma instância de Cluster do vSphere em Amsterdã. Depois que seu volume em Londres é reparado, uma captura instantânea é tomada do volume de Amsterdã. Em seguida, é possível efetuar failback para Londres e no volume novamente primário de uma instância de cálculo em Londres.

Antes que o volume efetue failback novamente para o data center primário, ele precisa parar de ser usado no site remoto. Uma captura instantânea de quaisquer informações novas ou mudadas é tomada e replicada para o data center primário antes que elas possam ser montadas novamente em hosts ESXi do site de produção.

Para obter mais informações sobre como configurar réplicas, consulte [Replicação](replication.html).

Dados inválidos, quer estejam corrompidos, hackeados ou infectados, devem ser replicados de acordo com o planejamento de captura instantânea e a retenção de captura instantânea. O uso das janelas de replicação menores pode fornecer um objetivo do ponto de recuperação melhor. No entanto, isso também pode fornecer menos tempo para reagir à replicação de dados inválidos.
{:note}


## Solicitando o {{site.data.keyword.filestorage_short}}

É possível pedir e configurar o {{site.data.keyword.filestorage_short}} para um ambiente VMware ESXi 5. Use as informações a seguir junto com a [Arquitetura de referência do Advanced Single-Site VMware](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window} para configurar uma dessas opções de armazenamento em seu ambiente VMware.

O {{site.data.keyword.filestorage_short}} pode ser pedido por meio do [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} acessando a página {{site.data.keyword.filestorage_short}} por meio de **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.


### 1. Pedindo o

Use as etapas a seguir para pedir o {{site.data.keyword.filestorage_short}}:
1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** na página inicial do [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
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

O armazenamento é provisionado em menos de um minuto e torna-se visível na página **{{site.data.keyword.filestorage_short}}** do [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.



### 2. Autorizando Hosts a Usar o  {{site.data.keyword.filestorage_short}}

Quando um volume é provisionado, o {{site.data.keyword.BluBareMetServers_full}} ou o {{site.data.keyword.BluVirtServers_full}} que usará o volume deve estar autorizado a acessar o armazenamento. Use as etapas a seguir para autorizar o volume:

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione **Acessar host** no menu **Ações de volume do Performance** ou **Endurance**.
3. Clique em  **Subnets**.
4. Escolha na lista de sub-redes disponíveis que estão designadas às portas do VMkernel nos hosts ESXi e clique em **Enviar**.<br/>

   As sub-redes que são exibidas são sub-redes inscritas no mesmo data center que o volume de armazenamento.
   {:note}


Depois que as sub-redes forem autorizadas, anote o nome do host do servidor de armazenamento do Endurance ou do Performance que você deseja usar quando montar o volume. Essas informações podem ser localizadas na página de detalhes do {{site.data.keyword.filestorage_short}} clicando em um volume específico.


##  Configurando o Host da Máquina Virtual VMware

### Cumprindo os Pré-requisitos

Antes de iniciar o processo de configuração do VMware, certifique-se de que os pré-requisitos a seguir sejam atendidos:

- Os {{site.data.keyword.BluBareMetServers}} com VMware ESXi são provisionados com a configuração de armazenamento adequada e as credenciais de login do ESXi.
- Os {{site.data.keyword.virtualmachinesshort}} ou físicos do Windows do {{site.data.keyword.BluSoftlayer_full}} no mesmo data center que o {{site.data.keyword.BluBareMetServers}}. Incluindo o endereço IP público da VM do Windows do {{site.data.keyword.BluSoftlayer_full}} e credenciais de login.
- Um computador com acesso à Internet e com o software de navegador da web e um cliente Remote Desktop Protocol (RDP) instalados.


### 1. Configurando o host do VMware.

1. Em um computador conectado à Internet, inicie um cliente RDP e estabeleça uma sessão RDP com o {{site.data.keyword.BluVirtServers_full}} provisionado no mesmo data center em que o vSphere vCenter está instalado.
2. No {{site.data.keyword.BluVirtServers_short}}, inicie um navegador da web e conecte-se ao VMware vCenter por meio do vSphere Web Client.
3. Na tela **INÍCIO**, selecione **Hosts e clusters**. Expanda a área de janela à esquerda e selecione o **Servidor VMware ESXi** que deve ser usado para esta implementação.
4. Certifique-se de que a porta de firewall do cliente NFS esteja aberta em todos os hosts para que seja possível configurar o cliente NFS no host vSphere. Ela é aberta automaticamente nas liberações mais recentes do vSphere. Para verificar se a porta está aberta, acesse a guia **Gerenciamento do host ESXi** no VMware® vCenter™, selecione **Configurações** e, em seguida, selecione **Perfil de segurança**. Na seção **Firewall**, clique em **Editar** e role para baixo para **Cliente NFS**.
5. Certifique-se de que **Permitir conexão de qualquer endereço IP ou uma lista de endereços IP** seja fornecido. <br/>
   ![Permitir conexão](/images/1_4.png)
6. Configure quadros gigantes acessando a guia **Gerenciar do host ESXi**, selecione **Gerenciar** e, em seguida, **Rede**.
7. Selecione **Adaptadores VMkernel**, destaque o **vSwitch** e, em seguida, clique em **Editar** (Ícone de lápis).
8. Selecione **Configuração de NIC** e assegure-se de que a MTU NIC esteja configurada como 9.000.
9. **Opcional**. Valide as configurações do quadro gigante.
   - No Windows: `ping -f -l 8972 a.b.c.d`
   - No UNIX: `ping -s 8972 a.b.c.d`
     Em que a.b.c.d é a interface vizinha de {{site.data.keyword.BluVirtServers_short}}.
     Exemplo
     ```
     ping a.b.c.d (a.b.c.d) 8972(9000) bytes of data.
     8980 bytes from a.b.c.d: icmp_seq=1 ttl=128 time=3.36 ms
     ```

Para obter mais informações sobre o VMware e os Quadros gigantes, clique [aqui](https://kb.vmware.com/s/article/1003712){:new_window}.
{:tip}


### 2. Incluindo um Adaptador uplink em um comutador virtual

1. Configure um novo adaptador de uplink acessando a guia **Gerenciar do host ESXi**, selecione **Gerenciar** e, em seguida, **Rede**.
2. Selecione a guia **Adaptadores físicos**
3. Clique em **Incluir rede de host** (Ícone Globo com um sinal de mais).
4. Selecione o tipo de conexão como **Adaptador de rede física** e clique em **Avançar**.
5. Selecione o **vSwitch** existente e clique em **Avançar**.
6. Selecione **Adaptadores não usados** e clique em **Incluir adaptadores** (Sinal de mais).
7. Clique no outro adaptador "Conectado" e clique em **OK**. <br/>
   ![Add physical adapters to switch](/images/2_3.png)
8. Clique em **Avançar** e **Concluir**.
9. Volte para a guia **Comutadores virtuais** e selecione a **Configuração de edição** (Ícone de lápis) sob o título **Comutadores virtuais**.
10. À esquerda, selecione a entrada **Equipe e failover** do vSwitch.
11. Verifique se a opção **Balanceamento de carga** está configurada para **Rotear com base na porta virtual de origem** e clique em **OK**.


### 3. Configurando o roteamento estático do ESXi (opcional)

A configuração de rede para este guia de arquitetura usa um número mínimo de grupos de portas. Se você tiver um grupo de portas do VMkernel para armazenamento NFS, etapas extras deverão ser executadas. Por padrão, o ESXi usa a porta do VMkernel que está na mesma sub-rede que um volume NFS para montar o volume NFS no armazenamento do Endurance/Performance. Como o roteamento da camada 3 é usado para montar o volume NFS, o ESXi deve ser forçado a usar a porta do VMkernel que foi configurada para montar o volume NFS. Para fazer isso, uma rota estática deve ser criada para a matriz de armazenamento do Endurance/Performance.

1. Para configurar uma rota estática, use SSH para cada host ESXi que usa armazenamento do Performance ou do Endurance e execute os comandos a seguir. Anote o endereço IP que for o resultado do comando `ping` (primeiro comando) e use-o com o comando de rede `esxcli`.
   ```
   ping <host name of the endurance/performance array>
   ```
   {: pre}

   >**Nota** - o nome do host DNS de armazenamento do NFS é uma Zona de encaminhamento (FZ) à qual é designada múltiplos endereços IP. Esses endereços IP são estáticos e pertencem a esse nome do host DNS específico. Qualquer um desses endereços IP pode ser usado para acessar um volume específico.

   ```
   esxcli network ip route ipv4 add –gateway GATEWAYIP –network <result of ping command>/32
   ```
   {: pre}

2. As rotas estáticas não são persistentes entre as reinicializações no ESXi 5.0 e anterior. Para assegurar-se de que quaisquer rotas estáticas incluídas permaneçam persistentes, esse comando precisará ser incluído no arquivo `local.sh` em cada host, que está localizado no diretório `/etc/rc.local.d/`. Abra o arquivo `local.sh` usando o editor visual e inclua o segundo comando na Etapa 4.1. na frente da linha `exit 0`.

Anote o endereço IP, pois ele poderá ser usado para montar o volume na próxima etapa.<br/>Esse processo precisa ser feito para cada volume NFS que você planeja montar em seu host ESXi.<br/>Para obter mais informações, veja o artigo do VMware KB, [Configurando rotas estáticas para portas do VMkernel em um host ESXi](https://kb.vmware.com/s/article/2001426){:new_window}.
{:tip}


##  Criando e montando o volume do {{site.data.keyword.filestorage_short}} nos hosts do ESXi

1. Clique no ícone **Acessar o vCenter** e, em seguida, em **Hosts e clusters**.
2. Na guia **Objeto relacionado**, clique em **Armazenamentos de dados**.
3. Clique no ícone **Criar um novo armazenamento de dados**.
4. Na tela **Novo armazenamento de dados**, selecione o local do armazenamento de dados do WMware (seu host ESXi) e clique em **Avançar**.
5. Na tela **Tipo**, selecione **NFS** e clique em **Avançar**.
6. Em seguida, selecione a versão do NFS. Tanto o NFSv3 quanto o NFSv4.1 são suportados, mas o NFSv3 é preferencial. Certifique-se de usar apenas uma versão do NFS para acessar um determinado armazenamento de dados. A montagem de um ou mais hosts no mesmo armazenamento de dados usando diferentes versões pode resultar em distorção de dados.
7. Na tela **Nome e configuração**, insira o nome que você deseja para chamar o armazenamento de dados do WMware. Além disso, insira o nome do host do servidor NFS. Usar o FQDN para o servidor NFS produz a melhor distribuição de tráfego para o servidor subjacente. O endereço IP também é válido, mas é usado menos frequentemente e somente em instâncias específicas. Insira o nome da pasta no formato `/foldername`.
8. Na tela **Acessibilidade do host**, selecione um ou mais hosts nos quais você deseja montar o armazenamento de dados do NFS WMware e clique em **Avançar**.
9. Revise as entradas na próxima tela e clique em **Concluir**.
10. Repita para quaisquer volumes adicionais do {{site.data.keyword.filestorage_short}}.

É recomendação do {{site.data.keyword.BluSoftlayer_full}} que os nomes FQDN sejam usados para conexão ao armazenamento de dados WMware. O uso do endereçamento IP direto pode efetuar bypass do mecanismo de balanceamento de carga fornecido com o uso do FQDN.
{:important}

Para usar o endereço IP em vez do FQDN, basta executar ping do servidor para obter o endereço IP.
```
ping <host name of the endurance/performance array>
```
{: pre}

Para obter o endereço IP por meio de um host ESXi, use o comando a seguir. O 10.2.125.80 resultante é o IP com o FQDN.
```
~ # vmkping nfsdal0902a-fz.service.softlayer.com
PING nfsdal0902a-fz.service.softlayer.com (10.2.125.80): 56 data bytes
64 bytes from 10.2.125.80: icmp_seq=0 ttl=253 time=0.187 ms
```


## Ativando o Controle de E/S de Armazenamento ESXi (Opcional)

O Storage I/O Control (SIOC) é um recurso disponível para clientes que usam uma licença Enterprise Plus. Quando o SIOC é ativado no ambiente, ele muda o comprimento da fila de dispositivo para a única VM. A mudança para o comprimento da fila de dispositivo reduz a fila de matriz de armazenamento de todas as VMs para um compartilhamento igual. O SIOC é encaixado somente se os recursos são restritos e a latência de E/S de armazenamento está acima de um limite definido.


Para que o SIOC determine quando um dispositivo de armazenamento está congestionado ou restrito, ele requer um limite definido. A latência de limite de congestionamento é diferente para tipos de armazenamento diferentes. A seleção padrão é de 90% do rendimento de pico. A porcentagem de valor de rendimento de pico indica o limite de latência estimado quando o armazenamento de dados do WMware está usando essa porcentagem de seu rendimento de pico estimado.


Configurar incorretamente o SIOC para um armazenamento de dados WMware ou para um VMDK pode impactar significativamente o desempenho.
{:important}


### Configurando o Storage I/O Control para um armazenamento de dados do WMware

Use as etapas a seguir para ativar o SIOC com os valores recomendados para armazenamento do Endurance e Performance:

1. Procure o armazenamento de dados do WMware no navegador do vSphere Web Client.
2. Clique na guia **Gerenciar**.
3. Clique em **Configurações** e clique em **Geral**.
4. Clique em **Editar** para **Recursos de armazenamento de dados**.
5. Marque a caixa de seleção **Ativar Storage I/O Control**.<br/>
   ![NSF WMware Datastore](/images/3_0.png)
6. Clique em **OK**.

Essa configuração é específica para o armazenamento de dados WMware e não para o host.
{:note}


### Configurando o Storage I/O Control para  {{site.data.keyword.BluVirtServers_short}}

Também é possível limitar discos virtuais individuais para VMs individuais ou conceder a eles compartilhamentos diferentes com SIOC. A limitação de discos e a concessão de compartilhamentos diferentes permite corresponder e alinhar o ambiente para a carga de trabalho com o número de IOPS do volume do {{site.data.keyword.filestorage_short}} adquirido. O limite é configurado por IOPS e é possível configurar um peso diferente ou "Compartilhamentos". Os discos virtuais com compartilhamentos configurados como Alto (2.000 compartilhamentos) recebem o dobro de E/S
que um disco configurado como Normal (1.000 compartilhamentos) e quatro vezes mais que um disco configurado
como Baixo (500 compartilhamentos). Normal é o valor padrão para todas as VMs, portanto, é necessário ajustar os valores acima ou abaixo de Normal para as VMs que requerem isso.

Use as etapas a seguir para mudar os compartilhamentos e o limite de VDisk.

1. Escolha um dos {{site.data.keyword.BluVirtServers_short}} no inventário **VMs e modelos**.
2. Selecione os {{site.data.keyword.BluVirtServers_short}} para controle de E/S.
3. Clique na guia **Gerenciar** e clique em **Configurações**. Clique em **Editar**.
4. Expandir a seta  ** disco rígido ** . Selecione **Modificar os compartilhamentos** ou **Limite - IOPs** conforme apropriado para seu ambiente. Escolha um disco rígido virtual na lista e modifique a seleção de Compartilhamentos para escolher o número relativo de compartilhamentos a serem alocados para o {{site.data.keyword.BluVirtServers_short}} (Baixo, normal ou alto). Também é possível clicar em **Customizado** e insira um valor compartilhado definido pelo usuário.
5. Clique na coluna **Limite - IOPS** e insira os recursos máximos de armazenamento a serem alocados para a máquina virtual.
6. Clique em **OK**


Esse processo é usado para configurar os limites de consumo de recursos de vDisks individuais em um {{site.data.keyword.BluVirtServers_short}} mesmo quando o SIOC não está ativado. Essas configurações são específicas para o guest individual e não para o host, embora elas sejam usadas por SIOC.
{:important}


## Configurando as configurações do lado do host ESXi

Configurações extras são necessárias para configurar hosts ESXi 5.x para armazenamento do NFS. Esta tabela mostra o que cada configuração precisa ser.

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


### Atualizando parâmetros de configuração avançada no host ESXi 5.x usando o CLI

Os exemplos a seguir usam a CLI para configurar os parâmetros de configuração avançada e, em seguida, verificá-los. A ferramenta `esxcfg-advcfg` usada nos exemplos pode ser localizada no diretório `/usr/sbin` nos hosts ESXi 5.x.

   - Configurando os parâmetros de configuração avançada por meio da CLI do ESXi 5.x.

     ```
     #esxcfg-advcfg -s 32 /Net/TcpipHeapSize
     #esxcfg-advcfg -s 128 /Net/TcpipHeapMax(For vSphere 5.0/5.1)
     #esxcfg-advcfg -s 512 /Net/TcpipHeapMax(For vSphere 5.5 and above)
     #esxcfg-advcfg -s 256 /NFS/MaxVolumes
     #esxcfg-advcfg -s 256 /NFS41/MaxVolumes (ESXi 6.0 and above)
     #esxcfg-advcfg -s 10 /NFS/HeartbeatMaxFailures
     #esxcfg-advcfg -s 12 /NFS/HeartbeatFrequency
     #esxcfg-advcfg -s 5 /NFS/HeartbeatTimeout   
     #esxcfg-advcfg -s 64 /NFS/MaxQueueDepth
     #esxcfg-advcfg -s 32 /Disk/QFullSampleSize
     #esxcfg-advcfg -s 8 /Disk/QFullThreshold
     ```

  - Verificando os parâmetros de configuração avançada da CLI do ESXi 5.x.

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

## Ativando Quadros gigantes no {{site.data.keyword.BluSoftlayer_notm}} para Windows e Linux

Um quadro gigante é um quadro Ethernet com uma carga útil maior que a unidade máxima de transmissão (MTU) padrão de 1.500 bytes. Os quadros gigantes são usados em redes locais que suportam pelo menos 1 Gbps e podem ser tão grandes quanto 9.000 bytes.

Os quadros gigantes precisam ser configurados da mesma maneira em todo o caminho de rede do dispositivo de origem <-> comutador <-> roteador <-> comutador <-> dispositivo de destino. Se a cadeia inteira não for configurada da mesma maneira, ela será padronizada para a configuração mais baixa ao longo da cadeia. O {{site.data.keyword.BluSoftlayer_full}} tem dispositivos de rede configurados para 9.000 atualmente. Todos os dispositivos do cliente precisam ser configurados com o mesmo valor 9.000.
{:important}

### Ativando Quadros Jumbo no Windows

1. Abra o **Centro de rede e compartilhamento**.
2. Clique em **Mudar as configurações do adaptador**.
3. Clique com o botão direito no NIC para o qual você deseja ativar quadros gigantes e selecione **Propriedades**.
4. Na guia **Rede**, clique em **Configurar** para o adaptador de rede.
5. Selecione a guia **Avançado**.
6. Selecione **Quadro gigante** e mude o valor de **disabled** para o valor desejado. O valor, como 9 kB ou 9.014 bytes, depende do NIC.
7. Clique em  ** OK **  em todas as janelas.

Quando você faz a mudança, a NIC perde a conectividade de rede por alguns segundos. Reinicie o dispositivo para confirmar que a mudança foi efetivada.
{:tip}


### Ativando Quadros Jumbo no Linux

1. Edite o arquivo de configuração de rede para a interface eth0.
   - Os usuários do CentOS/RHEL/Fedora Linux editam `/etc/sysconfig/network-script/ifcfg-eth0`
     ```
     # vi /etc/sysconfig/network-script/ifcfg-eth0
     ```
     {: pre}

   - Usuários do Debian / Ubuntu Linux editam  ` /etc/network/interfaces `.

2. Anexe a diretiva de configuração a seguir, que especifica o tamanho do quadro em bytes.
   - CentOS/RHEL/Fedora Linux
     ```
     MTU 9000
     ```
     {: pre}

   - Debian / Ubuntu Linux
     ```
     MTU=9000
     ```
     {: pre}

3. Feche e salve o arquivo. Reinicie a Interface eth0.
   ```
   # /etc/init.d/networking restart
   ```
   {: pre}

   Essa ação causa uma breve perda de conectividade de rede.

Saiba mais sobre a arquitetura de referência do Advanced Single-Site VMware [aqui](https://{DomainName}/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html){:new_window}.
{:tip}
