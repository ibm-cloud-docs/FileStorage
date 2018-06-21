---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Criando um {{site.data.keyword.filestorage_short}} duplicado

Oferecemos a capacidade de criar uma duplicata de um volume existente do {{site.data.keyword.filestorage_full}}. O volume duplicado herda as opções de capacidade e de desempenho do volume original por padrão e tem uma cópia dos dados até o momento de uma captura instantânea.   
Como a duplicata é baseada nos dados em uma captura instantânea de um momento, o espaço de captura instantânea é necessário no volume original antes de poder criar uma duplicata. Para saber mais sobre capturas instantâneas e como ordenar espaço de captura instantânea, consulte a [Documentação de captura instantânea](snapshots.html).

As duplicatas podem ser criadas por meio dos volumes primário e de réplica, com a nova duplicata sendo criada no mesmo data center que o volume original. Por exemplo, se você criar uma duplicata de um volume de réplica, o novo volume será criado no mesmo data center que o volume de réplica.    

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento é provisionado. As capturas instantâneas e a replicação não serão permitidas até que a cópia de dados do original para a duplicata esteja completa. 

Quando a cópia de dados estiver completa, a duplicata poderá ser gerenciada e usada como um volume completamente independente do original. 

Esse recurso está disponível somente para armazenamento que é provisionado com criptografia. Clique [aqui](new-ibm-block-and-file-storage-location-and-features.html) para obter uma lista de data centers disponíveis. 

Alguns usos comuns para um volume duplicado:
  - **Teste de recuperação de desastre**: crie uma duplicata de seu volume de réplica para verificar se os dados estão intactos e poderão ser usados no caso de um desastre, sem interromper a replicação. 
  - **Cópia golden**: use um volume de armazenamento como cópia golden por meio do qual é possível criar múltiplas instâncias para vários usos. 
  - **Atualização de dados**: crie uma cópia de seus dados de produção para montar em seu ambiente de não produção para teste. 
  - **Restaurar da captura instantânea**: restaure dados no volume original com arquivos/data específicos de uma captura instantânea sem sobrescrever o volume original inteiro com uma função de restauração de captura instantânea. 
  - **Desenvolvimento/teste**: crie até quatro duplicatas simultâneas de um volume de cada vez para criar volumes com dados duplicados para desenvolvimento e teste. 
  - **Redimensionamento de armazenamento**: crie um volume com o novo tamanho e/ou IOPS sem precisar executar uma migração de seus dados baseada em host.  
	

Existem algumas maneiras de criar um volume duplicado por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}: 

## Como criar uma duplicata de um volume específico na lista de armazenamento

Navegue para a sua lista de {{site.data.keyword.filestorage_short}}:

No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.filestorage_short}}**. 

1.	Selecione um volume na lista e clique em **Ações** > **Duplicar LUN (volume)** 
2.	Escolha sua opção de captura instantânea: 
    -	Se pedir por meio de um volume sem réplica:
      -	Selecione **Criar por meio da nova captura instantânea** - essa opção criará uma nova captura instantânea que será usada para a duplicata. Use essa opção se atualmente não houver nenhuma captura instantânea para seu volume ou se você desejar criar uma duplicata nesse momento. 
                      OU 

      -	Selecione **Criar por meio da captura instantânea mais recente** - essa opção criará uma duplicata por meio de qualquer captura instantânea mais recente que existir para esse volume. 
    -	Se pedir por meio de um volume de réplica - a única opção para captura instantânea será usar a captura instantânea mais recente disponível. 
3.	Faturamento por hora ou mensal – é possível escolher provisionar o novo volume duplicado com faturamento por hora ou mensal. O tipo de faturamento para o volume original é selecionado automaticamente. No entanto, se você quiser escolher um tipo de faturamento diferente para seu novo armazenamento de duplicata, será possível fazer essa seleção aqui.
4. 	O tipo de armazenamento (Resistência ou Desempenho) e Local permanecerão os mesmos que o volume original. 
5.	Se quiser, será possível especificar IOPS ou Camada de IOPS para o novo volume. A designação de IOPS do volume original é configurada por padrão. 
      -	Se seu volume original for a camada de 0,25 IOPS do Endurance, não será possível fazer uma nova seleção. 
      -	Se seu volume original for a camada de 2, 4 ou 10 IOPS do Endurance, será possível se mover em qualquer lugar entre essas camadas para o novo volume. 
      -	As combinações disponíveis de Desempenho e tamanho serão exibidas. 
6.	Se quiser, será possível atualizar o tamanho do novo volume para que seja maior que o original. O tamanho do volume original é configurado por padrão. 
  	-	**Nota**: o {{site.data.keyword.filestorage_short}} só pode ser redimensionado para 10 vezes o tamanho original do volume. 
7.	Se quiser, será possível atualizar o espaço de captura instantânea do novo volume para incluir mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original será configurado por padrão. 
8.	Clique em **Continuar** para fazer seu pedido para a duplicata. 



## Como criar uma duplicata de uma captura instantânea específica

Navegue para a sua lista de {{site.data.keyword.filestorage_short}}:

1.	Clique em um **LUN/volume** na lista para visualizar a página de detalhes. (Pode ser um volume de réplica ou não réplica) 
2.	Role para baixo e selecione uma captura instantânea existente na lista na página de detalhes e clique em **Ações > Duplicar**.   
3.	Tipo de armazenamento (Resistência ou Desempenho) e Local permanecerão iguais aos do volume original. 
4.	Se quiser, será possível especificar IOPS ou Camada de IOPS para o novo volume. A designação de IOPs do volume original é configurada por padrão. 
      - Se seu volume original for a camada de 0,25 IOPS do Endurance, não será possível fazer uma nova seleção. 
      - Se seu volume original for a camada de 2, 4 ou 10 IOPS do Endurance, será possível mudar entre essas camadas para o novo volume. 
      - As combinações disponíveis de Desempenho e tamanho serão exibidas. 
5.	Se quiser, será possível atualizar o tamanho do novo volume para que seja maior que o original. O tamanho do volume original é configurado por padrão. 
      - **Nota**: o {{site.data.keyword.filestorage_short}} só pode ser redimensionado para 10 vezes o tamanho original do volume. 
6.	Se quiser, será possível atualizar o espaço de captura instantânea do novo volume para incluir mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original será configurado por padrão. 
7.	Clique em **Continuar** para fazer seu pedido para a duplicata. 


## Como gerenciar o seu volume duplicado

Enquanto os dados estiverem sendo copiados do volume original para a duplicata, você verá um status na página de detalhes que indica que a duplicação está em andamento. Durante esse tempo, é possível se conectar a um host e ler/gravar no volume, mas não é possível criar planejamentos de captura instantânea. Quando o processo de duplicação estiver completo, o novo volume será completamente independente do volume original e poderá ser gerenciado com capturas instantâneas, replicação e assim por diante, normalmente. 
