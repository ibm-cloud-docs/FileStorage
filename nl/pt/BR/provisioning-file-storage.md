---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# Solicitando o {{site.data.keyword.filestorage_short}}

É possível provisionar o Armazenamento do {{site.data.keyword.filestorage_short}} e ajustá-lo com precisão para atender às suas necessidades de capacidade e IOPS. Obtenha o máximo de seu armazenamento com duas opções para especificar desempenho.

- É possível escolher entre camadas de IOPs do Endurance que apresentam níveis de desempenho predefinidos para ajustar cargas de trabalho que não têm requisitos de desempenho bem definidos. 
- É possível ajustar com precisão seu armazenamento para atender a requisitos de desempenho muito específicos, especificando o número total de IOPS com o Performance.

## Solicitando  {{site.data.keyword.filestorage_short}}  com Camadas IOPS predefinidas (Endurance)

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Na parte superior direita, clique em **Pedir {{site.data.keyword.filestorage_short}}**.
3. Selecione sua implementação  ** Local **  (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host de cálculo ou os hosts que você possui.
4. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal. 
     1. Com o faturamento **por hora**, o número de horas que o LUN de bloco existiu na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento. O que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para o armazenamento provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um LUN de bloco é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento que é usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos (mês ou mais).
        >**NOTA** - o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que **não** estão atualizados com recursos melhorados.
5. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
6. Selecione **Endurance (IOPS em camadas)** na seção **Opções de IOPS de armazenamento**.
7. Selecione a camada IOPS que seu aplicativo precisa.
    - **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos de cada vez. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.
    - **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos de cada vez. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.
    - **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) para armazenamento provisionado até 4 TB.
8. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
9. Marque as caixas de seleção de **Termos e condições** e clique em **Fazer pedido**.
10. Sua nova alocação de armazenamento estará disponível em alguns minutos.

>**Nota** - por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).<br/><br/>Para obter o limite de autorizações simultâneas, consulte as [FAQs](File-Storage-FAQ.html).

## Solicitando  {{site.data.keyword.filestorage_short}}  com IOPS Customizado (Desempenho)

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** >** Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Na parte superior direita, clique em **Pedir {{site.data.keyword.filestorage_short}}**.
3. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host de cálculo ou os hosts que você possui.
4. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal. 
     1. Com o faturamento **por hora**, o número de horas que o LUN de bloco existiu na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento. O que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para o armazenamento provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um LUN de bloco é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento que é usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos (mês ou mais).
        >**NOTA** - o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que **não** estão atualizados com recursos melhorados.
5. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
6. Selecione **Performance (IOPS alocado)** na seção **Opções de IOPS de armazenamento**.
7. Insira o IOPS no campo **Performance (IOPS alocado)**.
8. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
9. Marque as caixas de seleção de **Termos e condições** e clique em **Fazer pedido**.
10. Sua nova alocação de armazenamento estará disponível em alguns minutos.

>**Nota** - por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).<br/><br/>Para obter o limite de autorizações simultâneas, consulte as [FAQs](File-Storage-FAQ.html).


## Conectando seu novo armazenamento

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional de seu host, siga o link apropriado.
- [Acessando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)
- [Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com cPanel](configure-backup-cpanel.html)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com Plesk](configure-backup-plesk.html)
- [Montando o volume do {{site.data.keyword.filestorage_short}} em hosts ESXi](architecture-guide-file-storage-vmware.html)


## Identificando  {{site.data.keyword.filestorage_short}}  volumes na fatura

Todos os LUNs aparecem em sua fatura como um item de linha. O Endurance aparece como "Serviço de armazenamento do Endurance" e o Performance aparece como "Serviço de armazenamento do Performance". A taxa varia com base em seu nível de armazenamento. É possível expandir o Endurance ou o Performance para ver que ele é {{site.data.keyword.filestorage_short}}.
