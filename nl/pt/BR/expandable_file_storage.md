---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-11"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacidade de compartilhamento de arquivo expansível

Com esse novo recurso, nossos usuários atuais do {{site.data.keyword.filestorage_full}} são capazes de expandir o tamanho de seu {{site.data.keyword.filestorage_short}} em incrementos de GB de até 12 TB imediatamente, sem a necessidade de criar uma duplicata ou migrar dados manualmente para um volume maior. Não há nenhuma indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento está ocorrendo. 

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada do novo preço no ciclo de faturamento atual. Então, a nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que tirar vantagem de compartilhamentos de arquivo expansíveis?

- **Gerenciamento de custos** - você pode saber que há potencial para o crescimento de seus dados, mas precisa de uma quantia menor de armazenamento para iniciar. A capacidade para expandir permite que os nossos clientes economizem em custos de armazenamento no início e depois cresçam para acomodar suas necessidades.  

- **Necessidades de armazenamento crescente** - os clientes que experimentam um rápido crescimento precisam de uma maneira rápida e fácil de aumentar o tamanho de seu armazenamento para gerenciar esse crescimento.

## Como o redimensionamento de armazenamento afeta a replicação?

Expandir a ação no armazenamento primário resultará em redimensionamento automático da réplica.

## Há alguma limitação?

Esse recurso está disponível somente para armazenamento que é provisionado em [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados. O armazenamento criptografado provisionado nesses data centers pode ser aumentado em até 12 TB. 

As limitações de tamanho existentes para o {{site.data.keyword.filestorage_short}} provisionado com o Endurance ainda se aplicam (até 4 TB para camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Como identificar se meu armazenamento provisionado é expansível?

O armazenamento provisionado com recursos aprimorados é sempre criptografado em repouso. Será possível identificar facilmente se seu armazenamento é elegível se ele tiver um ícone de "bloqueio" próximo a ele na UI do portal. 

## Como posso redimensionar meu armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento deve estar disponível em alguns minutos.

