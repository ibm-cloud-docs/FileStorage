---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Pedindo e gerenciando o {{site.data.keyword.filestorage_full_notm}}

Há dois tipos diferentes de armazenamento que é possível provisionar com base em suas necessidades e preferências. As duas opções de volumes do {{site.data.keyword.filestorage_short}} são:

- **Endurance**: provisione camadas do Endurance que apresentam níveis de desempenho e recursos predefinidos, como capturas instantâneas e replicação.
- **Performance**: construa um ambiente do Performance altamente poderoso no qual é possível alocar o input/output operations per second (IOPS) desejado.

## Provisionando o {{site.data.keyword.filestorage_short}}

### Como pedir o Endurance para o {{site.data.keyword.filestorage_short}}

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU no Catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir {{site.data.keyword.filestorage_short}}** no canto superior direito da tela. 
3. Selecione **Endurance** na lista suspensa **Selecionar tipo de armazenamento**.
4. Clique na lista suspensa **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts solicitados anteriormente.
   - Se tiver selecionado um data center com recursos melhorados (indicados com um * na lista suspensa), você terá a opção de escolher entre faturamento mensal ou por hora. 
     1. Com o faturamento **por hora**, o cálculo do número de horas do volume de arquivo que existia na conta é executado no momento em que o volume é excluído ou no término do ciclo de faturamento, o que vier primeiro.  O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não haverá reembolso se um volume de arquivo for excluído antes do término do ciclo de faturamento.  O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).
     **NOTA**: o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que não são atualizados com recursos melhorados.
5. Selecione o nível de armazenamento para os seus aplicativos:
    - **0,25 IOPS por GB** foi projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho são caracterizadas geralmente por ter uma grande porcentagem de dados inativos em um determinado momento. Aplicativos de exemplo incluem o armazenamento de caixas de correio ou compartilhamentos de arquivos de nível departamental.
    - **2 IOPS por GB** foi projetado para uso de propósito mais geral. Aplicativos de exemplo incluem a hospedagem de pequenos bancos de dados que suportam aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** foi projetado para cargas de trabalho de maior intensidade. Essas cargas de trabalho são caracterizadas geralmente por ter uma alta porcentagem de dados ativos em um determinado momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis ao desempenho.
    - **10 IOPS por GB** foi projetado para as cargas de trabalho de maior demanda, como aquelas criadas por Bancos de dados NoSQL e processamento de dados para Analytics.  Esta camada está disponível em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) para armazenamento provisionado de até 4 TB de tamanho.
6. Selecione o **Tamanho de armazenamento utilizável** na lista suspensa.
7. Escolha o **Tamanho do espaço de captura instantânea** (além de seu espaço utilizável) na lista suspensa.
8. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes da ordem.
9. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.
10. Sua nova alocação de armazenamento deve estar disponível em alguns minutos.

**Nota**: por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).

Para obter o limite em autorizações simultâneas, veja nossas [FAQs](File-Storage-FAQ.html)

### Como pedir o Performance para o {{site.data.keyword.filestorage_short}}

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** OU, no Catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** >** Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Clique em **Pedir {{site.data.keyword.filestorage_short}}** no canto superior direito da tela. 
3. Selecione **Performance** na lista suspensa Selecionar tipo de armazenamento.
4. Clique na lista suspensa **Local** e selecione seu data center.
    -  Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts solicitados anteriormente.
    -  Se tiver selecionado um data center com recursos melhorados (indicados com um * na lista suspensa), você terá a opção de escolher entre faturamento mensal ou por hora. 
       1.  Com o faturamento **por hora**, o cálculo do número de horas do volume de arquivo que existia na conta é executado no momento em que o volume é excluído ou no término do ciclo de faturamento, o que vier primeiro.  O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento provisionado nestes data centers selecionados. 
       2. Com o faturamento **mensal**, o cálculo para o preço é avaliado a partir da data de criação até o término do ciclo de faturamento e faturado imediatamente. Não haverá reembolso se um volume de arquivo for excluído antes do término do ciclo de faturamento.  O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).
       **NOTA**: o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que não são atualizados com recursos melhorados.  
5. Selecione o botão de opções próximo ao **Tamanho de armazenamento** apropriado.
6. Insira o IOPS no campo **Especificar IOPS**.
7. Clique em **Continuar**. Os encargos por mês e rateados são mostrados com uma chance final de revisar os detalhes da ordem.Clique em **Anterior** se você desejar mudar sua ordem.
8. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.
9. Sua nova alocação de armazenamento deve estar disponível em alguns minutos.

**Nota**: por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).

Para obter o limite em autorizações simultâneas, veja nossas [FAQs](File-Storage-FAQ.html)

## Gerenciando o {{site.data.keyword.filestorage_short}}

### Como autorizar hosts para acessar o {{site.data.keyword.filestorage_short}}

Os hosts “Autorizados” são hosts que receberam direitos de acesso para um volume específico. Sem autorização do host, você não será capaz de acessar ou usar o armazenamento de seu sistema. Autorizar um host a acessar seu volume gera o Nome do usuário, Senha. 

**Nota**: é possível autorizar e conectar somente hosts que residem no mesmo data center que o seu armazenamento. Se você tiver múltiplas contas, não será possível autorizar um host de uma conta para acessar seu armazenamento em outra. 

1. Clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
2. Role para a seção **Hosts autorizados** da página.
3. Clique no link **Autorizar host** no lado direito da página. Selecione os hosts que podem acessar esse volume específico.

 

### Como visualizar a lista de hosts autorizados para um volume do {{site.data.keyword.filestorage_short}}

Use as etapas a seguir para visualizar a lista de hosts autorizados para um volume.

1. Clique em **Armazenamento > {{site.data.keyword.filestorage_short}}** e clique em seu **Nome do volume**.
2. Role para baixo até a parte inferior da página na seção **Hosts autorizados**.

Aqui você verá a lista de hosts que estão atualmente autorizados a acessar o volume.


### Como visualizar os volumes do {{site.data.keyword.filestorage_short}} para os quais um host está autorizado

É possível visualizar os volumes para os quais um host tem acesso, incluindo as informações necessárias para fazer uma conexão, Nome do volume, Tipo de armazenamento, Endereço de destino, capacidade e local:

1. Clique em **Dispositivos** > **Lista de dispositivos** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e clique no dispositivo apropriado.
2. Selecione a guia Armazenamento.

Em seguida, será apresentada uma lista de volumes de armazenamento à qual esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Nos respectivos menus **Ação**, é possível autorizar armazenamento adicional ou remover o acesso.

 

### Como montar e desmontar o {{site.data.keyword.filestorage_short}}

É possível usar as informações de ponto de montagem fornecidas na visualização **Detalhes do volume** para montar o {{site.data.keyword.filestorage_short}} de um host.

Consulte o artigo a seguir com detalhes para montar e desmontar o {{site.data.keyword.filestorage_short}} de um host: [Acessando o {{site.data.keyword.filestorage_short}} no Linux](accessing-file-storage-linux.html)

 

### Como revogar o acesso de um host para o {{site.data.keyword.filestorage_short}}

Se você deseja parar o acesso de um host para um volume de armazenamento específico, é possível revogar o acesso. Após revogar o acesso, a conexão de host será eliminada do volume e nem o sistema operacional nem os aplicativos poderão se comunicar com o volume. 

**Nota:** para evitar problemas do lado do host, desmonte o volume de armazenamento de seu sistema operacional antes de revogar o acesso para evitar a ausência de unidades ou a distorção de dados.

É possível revogar o acesso do Armazenamento da Lista de dispositivos ou das visualizações de Armazenamento.

#### Como revogar o acesso da Lista de Dispositivos:

1. Clique em **Dispositivos** > **Lista de dispositivos** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e clique duas vezes no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.
3. Em seguida, será apresentada uma lista de volumes de armazenamento à qual esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outro). Selecione o respectivo menu **Ação** próximo ao volume do qual você deseja revogar o acesso e clique em **Revogar acesso**.
4. Será perguntado se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao volume ou em **Não** para cancelar a ação.

**Nota:** se você desejar desconectar múltiplos volumes de um host específico, precisará repetir a ação Revogar acesso para cada volume.

 

#### Como revogar o acesso da visualização de armazenamento:
1. Clique em **Armazenamento, {{site.data.keyword.filestorage_short}}** e selecione o **Volume** do qual você deseja revogar o acesso.
2. Role para baixo até a seção **Hosts autorizados** da página.
3. Clique na seta suspensa **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
4. Será perguntado se você deseja revogar o acesso para um volume porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao volume ou em **Não** para cancelar a ação.

**Nota:** se você desejar desconectar múltiplos hosts de um volume específico, será necessário repetir a ação Revogar acesso para cada host.

 

### Como cancelar um volume de armazenamento

Se você não precisar mais de um volume específico, será possível cancelá-lo. Para cancelar um volume de armazenamento, é necessário primeiro revogar o acesso de quaisquer hosts.

1. Clique em **Armazenamento**>**{{site.data.keyword.filestorage_short}}**.
2. Clique na seta suspensa **Ações** para o volume ser cancelado e selecione **Cancelar o {{site.data.keyword.filestorage_short}}**.
3. Será solicitado que você confirme se deseja cancelar o volume imediatamente ou na data de aniversário de quando o volume foi provisionado. Clique em **Continuar** ou **Fechar**.
**Nota**: se você selecionar a opção para cancelar o volume em sua data de aniversário, será possível anular a solicitação de cancelamento antes de sua data de aniversário.
4. Clique na caixa de seleção de confirmação e clique em **Confirmar**.

 

### Como ver os detalhes de um volume fornecido do {{site.data.keyword.filestorage_short}}

É possível visualizar um resumo das informações chave para o volume de armazenamento selecionado incluindo os recursos de captura instantânea e replicação adicionais que foram incluídos no armazenamento.

1. Clique em **Armazenamento**>**{{site.data.keyword.filestorage_short}}**.
2. Clique no **Nome do volume** apropriado na lista.

### Como identificar meus volumes do {{site.data.keyword.filestorage_short}} em minha fatura

Todos os volumes aparecerão em sua fatura como um item de linha; o Endurance aparecerá como “Serviço de armazenamento Endurance” e o Performance aparecerá como "Serviço de armazenamento Performance". A taxa irá variar com base em seu nível de armazenamento. É possível expandir no Endurance ou Performance para ver que é um volume do {{site.data.keyword.filestorage_short}}.
