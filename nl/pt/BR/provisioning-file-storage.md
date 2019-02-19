---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Pedindo o {{site.data.keyword.filestorage_short}} por meio do Console
{: #orderingConsole}

É possível fornecer o {{site.data.keyword.filestorage_short}} e ajustar com precisão para atender as suas necessidades de capacidade e de IOPS. Obtenha o máximo de seu armazenamento com duas opções para especificar desempenho.

- É possível escolher entre as camadas de IOPs do Endurance que apresentam níveis de desempenho predefinidos para ajustar as cargas de trabalho que não têm requisitos de desempenho bem definidos.
- É possível ajustar com precisão o seu armazenamento para atender a requisitos de desempenho específicos, determinando o número total de IOPS com o Performance.

## Solicitando  {{site.data.keyword.filestorage_short}}  com Camadas IOPS predefinidas (Endurance)
{: #endurance}

1. Efetue login no [Catálogo do IBM Cloud](https://{DomainName}/catalog/){:new_window} e clique em **Armazenamento**. Em seguida, selecione {{site.data.keyword.filestorage_short}}. Clique em **Criar**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window} } e clicar em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. Na parte superior direita, clique em **Pedir {{site.data.keyword.filestorage_short}}**.
2. Selecione sua implementação  ** Local **  (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host de cálculo ou os hosts que você possui.
3. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas em que o volume de arquivo existiu na conta é calculado no momento em que o LUN é excluído ou no final do ciclo de faturamento. O que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para o armazenamento provisionado nestes [data centers selecionados](/docs/infrastructure/FileStorage?topic=FileStorage-news).
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um volume de arquivo é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento que é usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos (mês ou mais).

     O tipo de faturamento mensal é usado por padrão para um armazenamento que é provisionado em data centers que **não** são atualizados com recursos aprimorados.
     {:note}
4. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
5. Selecione **Endurance (IOPS em camadas)** na seção **Opções de IOPS de armazenamento**.
6. Selecione a camada IOPS que seu aplicativo precisa.
    - **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos de cada vez. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.
    - **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos de cada vez. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.
    - **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível em [data centers selecionados](/docs/infrastructure/FileStorage?topic=FileStorage-news) para armazenamento provisionado até 4 TB.
7. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots).
8. À direita, revise o resumo do pedido e aplique o Código promocional, se tiver um.

   Os descontos são aplicados quando o pedido é processado.
   {:note}
9. Depois de ter revisado os termos e as condições, selecione a caixa **Eu li e concordo com os Contratos de serviço de terceiros**.
10. Clique em **Criar**. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).<br/><br/>Para obter mais informações sobre o limite de autorizações simultâneas, consulte as [FAQs](/docs/infrastructure/FileStorage?topic=FileStorage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:tip}

## Pedindo o {{site.data.keyword.filestorage_short}} com IOPS customizado (Performance)
{: #performance}

1. Efetue login no [Catálogo do IBM Cloud](https://{DomainName}/catalog/){:new_window} e clique em **Armazenamento**. Em seguida, selecione {{site.data.keyword.filestorage_short}}. Clique em **Criar**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window} } e clicar em **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. Na parte superior direita, clique em **Pedir {{site.data.keyword.filestorage_short}}**.
2. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host de cálculo ou os hosts que você possui.
3. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas em que o volume de arquivo existiu na conta é calculado no momento em que o LUN é excluído ou no final do ciclo de faturamento. O que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para o armazenamento provisionado nestes [data centers selecionados](/docs/infrastructure/FileStorage?topic=FileStorage-news).
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um volume de arquivo é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento que é usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos (mês ou mais).

     O tipo de faturamento mensal é usado por padrão para um armazenamento que é provisionado em data centers que **não** são atualizados com recursos aprimorados.
     {:note}
4. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
5. Selecione **Performance (IOPS alocado)** na seção **Opções de IOPS de armazenamento**.
6. Insira o IOPS no campo **Performance (IOPS alocado)**.
7. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-ordering-snapshots).
8. À direita, revise o resumo do pedido e aplique o Código promocional, se tiver um.

   Os descontos são aplicados quando o pedido é processado.
   {:note}
9. Depois de ter revisado os termos e as condições, selecione a caixa **Eu li e concordo com os Contratos de serviço de terceiros**.
10. Clique em **Criar**. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-managinglimits).<br/><br/>Para obter mais informações sobre o limite de autorizações simultâneas, consulte as [FAQs](/docs/infrastructure/FileStorage?topic=FileStorage-faqs#how-many-instances-can-share-the-use-of-a-provisioned-file-storage-volume-).
{:important}


## Conectando seu novo armazenamento

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional de seu host, siga o link apropriado.
- [Montando o {{site.data.keyword.filestorage_short}} no Linux](/docs/infrastructure/FileStorage?topic=FileStorage-mountingLinux)
- [Montando o {{site.data.keyword.filestorage_short}} no CentOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCentOS)
- [Montando o {{site.data.keyword.filestorage_short}} no CoreOS](/docs/infrastructure/FileStorage?topic=FileStorage-mountingCoreOS)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com o cPanel](/docs/infrastructure/FileStorage?topic=FileStorage-cPanelBackups)
- [Configurando o {{site.data.keyword.filestorage_short}} para backup com o Plesk](/docs/infrastructure/FileStorage?topic=FileStorage-PleskBackup)
- [Montando o volume do {{site.data.keyword.filestorage_short}} em hosts ESXi](/docs/infrastructure/FileStorage?topic=FileStorage-architectureguide)

## Considerações sobre recuperação de desastre

Para evitar perda de dados e para assegurar a continuidade dos negócios, considere replicar seus servidores e armazenamento em outro data center. A replicação mantém seus dados em sincronização em dois locais diferentes com base em seu planejamento de captura instantânea. Para obter mais informações, consulte [Replicando dados](/docs/infrastructure/FileStorage?topic=FileStorage-replication).

Se desejar clonar seu volume e utilizá-lo independentemente do volume original, consulte [Criando um volume de bloco duplicado](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).

## Identificando  {{site.data.keyword.filestorage_short}}  volumes na fatura

Todos os compartilhamentos de arquivo aparecem em sua fatura como um item de linha. Os volumes do Endurance aparecerão como “Serviço de armazenamento do Endurance”, e os volumes de Desempenho aparecerão como "Serviço de armazenamento de Desempenho". A taxa varia com base em seu nível de armazenamento. É possível expandir o Endurance ou o Performance para ver que ele é {{site.data.keyword.filestorage_short}}.
