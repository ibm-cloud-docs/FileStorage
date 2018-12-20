---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}

# Expandindo Capacidade de Compartilhamento de Arquivo

Com esse novo recurso, os usuários atuais do {{site.data.keyword.filestorage_full}} são capazes de expandir o tamanho do {{site.data.keyword.filestorage_short}} em incrementos de GB de até 12 TB imediatamente. Eles não precisam criar uma duplicata nem migrar dados manualmente para um volume maior. Não há nenhuma indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento está ocorrendo.

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada do novo preço no ciclo de faturamento atual. Então, a nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html).

## Vantagens de Armazenamento Expandível

- **Gerenciamento de custo** - Talvez você saiba que há potencial para crescimento de seus dados, mas que precisa de uma quantia menor de armazenamento para iniciar. A capacidade de expandir permite que os clientes economizem em custos de armazenamento no início e, em seguida, cresçam para acomodar suas necessidades.  

- **Necessidades de armazenamento crescente** - os clientes que experimentam um rápido crescimento precisam de uma maneira rápida e fácil de aumentar o tamanho de seu armazenamento para gerenciar esse crescimento.

## Efeitos da expansão da capacidade de armazenamento na Replicação

A ação de expansão no armazenamento primário resulta no redimensionamento automático da réplica.

## Limitações

Esse recurso está disponível somente para armazenamento que é provisionado em [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados. O armazenamento criptografado provisionado nesses data centers pode ser aumentado em até 12 TB.

As limitações de tamanho existentes para o {{site.data.keyword.filestorage_short}} que foram provisionadas com o Endurance ainda se aplicam (até 4 TB para a camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Redimensionando o

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise sua seleção e a nova precificação.
5. Clique em **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.
