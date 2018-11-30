---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Solicitando o {{site.data.keyword.filestorage_short}}

É possível provisionar o {{site.data.keyword.filestorage_short}} e ajustá-lo com precisão para atender às suas necessidades de capacidade e IOPS. Obtenha o máximo de seu armazenamento com duas opções para especificar desempenho.

- É possível escolher entre camadas de IOPs do Endurance que apresentam níveis de desempenho predefinidos para ajustar cargas de trabalho que não têm requisitos de desempenho bem definidos.
- É possível ajustar com precisão seu armazenamento para atender a requisitos de desempenho muito específicos, especificando o número total de IOPS com o Performance.

## Solicitando  {{site.data.keyword.filestorage_short}}  com Camadas IOPS predefinidas (Endurance)

1. Efetue login no [Catálogo do IBM Cloud](https://{DomainName}/catalog/){:new_window} e clique em **Armazenamento**. Em seguida, selecione {{site.data.keyword.filestorage_short}}. Clique em **Criar**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} } e clicar em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. Na parte superior direita, clique em **Pedir {{site.data.keyword.filestorage_short}}**.
2. Selecione sua implementação  ** Local **  (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host de cálculo ou os hosts que você possui.
3. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas que o volume de arquivo existiu na conta é calculado no momento em que o LUN é excluído ou ao final do ciclo de faturamento. O que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para o armazenamento provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html).
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um volume de arquivo é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento que é usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos (mês ou mais).

     O tipo de faturamento mensal é usado por padrão para um armazenamento que é provisionado em data centers que **não** são atualizados com recursos aprimorados.
     {:note}
4. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
5. Selecione **Endurance (IOPS em camadas)** na seção **Opções de IOPS de armazenamento**.
6. Selecione a camada IOPS que seu aplicativo precisa.
    - **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos de cada vez. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.
    - **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos de cada vez. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.
    - **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) para armazenamento provisionado até 4 TB.
7. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
8. À direita, revise o resumo do pedido e aplique o Código promocional, se tiver um.
9. Depois de ter revisado os termos e as condições, selecione a caixa **Eu li e concordo com os Contratos de serviço de terceiros**.
10. Clique em **Criar**. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).<br/><br/>Para obter mais informações sobre o limite de autorizações simultâneas, consulte as [FAQs](File-Storage-FAQ.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## Solicitando  {{site.data.keyword.filestorage_short}}  com IOPS Customizado (Desempenho)

1. Efetue login no [Catálogo do IBM Cloud](https://{DomainName}/catalog/){:new_window} e clique em **Armazenamento**. Em seguida, selecione {{site.data.keyword.filestorage_short}}. Clique em **Criar**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} } e clicar em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. Na parte superior direita, clique em **Pedir {{site.data.keyword.filestorage_short}}**.
2. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host de cálculo ou os hosts que você possui.
3. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas que o volume de arquivo existiu na conta é calculado no momento em que o LUN é excluído ou ao final do ciclo de faturamento. O que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para o armazenamento provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html).
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um volume de arquivo é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento que é usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos (mês ou mais).

     O tipo de faturamento mensal é usado por padrão para um armazenamento que é provisionado em data centers que **não** são atualizados com recursos aprimorados.
     {:note}
4. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
5. Selecione **Performance (IOPS alocado)** na seção **Opções de IOPS de armazenamento**.
6. Insira o IOPS no campo **Performance (IOPS alocado)**.
7. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
8. À direita, revise o resumo do pedido e aplique o Código promocional, se tiver um.
9. Depois de ter revisado os termos e as condições, selecione a caixa **Eu li e concordo com os Contratos de serviço de terceiros**.
10. Clique em **Criar**. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).<br/><br/>Para obter mais informações sobre o limite de autorizações simultâneas, consulte as [FAQs](File-Storage-FAQ.html#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Conectando seu novo armazenamento

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional de seu host, siga o link apropriado.
- [Montando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CentOS](mounting-nsf-file-storage.html)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](mounting-storage-coreos.html)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com cPanel](configure-backup-cpanel.html)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com Plesk](configure-backup-plesk.html)
- [Montando o volume do {{site.data.keyword.filestorage_short}} em hosts ESXi](architecture-guide-file-storage-vmware.html)


## Identificando  {{site.data.keyword.filestorage_short}}  volumes na fatura

Todos os compartilhamentos de arquivo aparecem em sua fatura como um item de linha. Os volumes do Endurance aparecerão como “Serviço de armazenamento do Endurance”, e os volumes de Desempenho aparecerão como "Serviço de armazenamento de Desempenho". A taxa varia com base em seu nível de armazenamento. É possível expandir o Endurance ou o Performance para ver que ele é {{site.data.keyword.filestorage_short}}.
