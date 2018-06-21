---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Solicitando o {{site.data.keyword.filestorage_short}} 

Há duas maneiras diferentes de provisionar o {{site.data.keyword.filestorage_full}} com base em suas necessidades e preferências. As duas opções são:

- **Endurance**: provisione camadas do Endurance que apresentam níveis de desempenho e recursos predefinidos, como capturas instantâneas e replicação.
- **Performance**: construa um ambiente do Performance altamente poderoso no qual é possível alocar o input/output operations per second (IOPS) desejado.

## Como pedir o Endurance para o {{site.data.keyword.filestorage_short}}

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir o {{site.data.keyword.filestorage_short}}** no canto superior direito. 
3. Selecione **Endurance** na lista **Selecionar tipo de armazenamento**.
4. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host pedido anteriormente.
   - Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal. 1. Com o faturamento **por hora**, o cálculo do número de horas que o volume de arquivo existiu na conta é executado no momento em que o volume é excluído ou no término do ciclo de faturamento, o que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não haverá reembolso se um volume de arquivo for excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).
     **NOTA**: o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que não são atualizados com recursos melhorados.
5. Selecione o nível de armazenamento para os seus aplicativos:
    - **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho são caracterizadas geralmente por ter uma grande porcentagem de dados inativos em um determinado momento. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.
    - **2 IOPS por GB** é projetado para uso de propósito geral. Aplicativos de exemplo incluem a hospedagem de pequenos bancos de dados que suportam aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho são caracterizadas geralmente por ter uma alta porcentagem de dados ativos em um determinado momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.
    - **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) para armazenamento provisionado de até 4 TB de tamanho.
6. Selecione o **Tamanho de armazenamento utilizável** na lista.
7. Escolha o **Tamanho do espaço de captura instantânea** (além de seu espaço utilizável) na lista.
8. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes da ordem.
9. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.
10. Sua nova alocação de armazenamento deve estar disponível em alguns minutos.

**Nota**: por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.filestorage_short}}. Entre em contato com o representante de vendas para aumentar o número de seus volumes. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).

Para o limite sobre autorizações simultâneas, veja as nossas [FAQs](File-Storage-FAQ.html)

## Como pedir o Performance para o {{site.data.keyword.filestorage_short}}

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** >** Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir o {{site.data.keyword.filestorage_short}}** no canto superior direito. 
3. Selecione **Desempenho** na lista Selecionar tipo de armazenamento.
4. Clique na lista **Local** e selecione seu data center.
    - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host pedido anteriormente.
    - Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal. 
       1. Com o faturamento **por hora**, o número de horas que o volume de arquivo existiu na conta é calculado no momento em que o volume é excluído ou no término do ciclo de faturamento, o que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento provisionado nestes data centers selecionados. 2. Com o faturamento **mensal**, o cálculo para o preço é rateado desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um volume de arquivo é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).
       **NOTA**: o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que não são atualizados com recursos melhorados.  
5. Selecione o **Tamanho do armazenamento** apropriado.
6. Insira o IOPS no campo **Especificar IOPS**.
7. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes do pedido. Clique em **Anterior** se você desejar mudar sua ordem.
8. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.
9. Sua nova alocação de armazenamento deve estar disponível em alguns minutos.

**Nota**: por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.filestorage_short}}. Entre em contato com o representante de vendas para aumentar o número de seus volumes. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).

Para o limite sobre autorizações simultâneas, veja as nossas [FAQs](File-Storage-FAQ.html)

## Como identificar meus volumes do {{site.data.keyword.filestorage_short}} em minha fatura

Todos os volumes aparecem em sua fatura como itens de linha. O Endurance aparecerá como “Serviço de armazenamento do Endurance” e Performance aparecerá como "Serviço de armazenamento do Performance". A taxa irá variar com base em seu nível de armazenamento. É possível expandir no Endurance ou Performance para ver que é um volume do {{site.data.keyword.filestorage_short}}.

