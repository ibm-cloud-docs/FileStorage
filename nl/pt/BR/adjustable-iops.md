---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# IOPS ajustável

Com esse novo recurso, nossos usuários de armazenamento do {{site.data.keyword.filestorage_full}} poderão ajustar o IOPS de seu {{site.data.keyword.filestorage_short}} existente instantâneo, sem a necessidade de criar uma duplicata ou migrar manualmente dados para o novo armazenamento. O usuário não terá nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste estiver ocorrendo. 

O faturamento para o armazenamento será atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual e, então, a nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que tirar vantagem de IOPS ajustável?

- Gerenciamento de custos - alguns dos nossos clientes podem precisar de IOPS alto somente durante os horários de pico de uso. Por exemplo, uma loja de varejo grande tem pico de uso durante as férias e pode precisar de IOPS mais alto no armazenamento em comparação com o meio do verão. Esse recurso permite que eles gerenciem seus custos e paguem mais IOPS somente quando eles realmente precisam disso.

## Há alguma limitação?

Esse recurso está disponível somente para armazenamento provisionado nos [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados.

Os clientes não podem alternar entre o Endurance e o Performance quando eles ajustam seu IOPS. Os usuários podem especificar uma nova camada de IOPS ou nível de IOPS para o armazenamento com base nos critérios/restrições a seguir: 

- Se o volume original é a camada Endurance 0,25, a camada de IOPS não pode ser atualizada.
- Se o volume original é Performance com < 0,30 IOPS/GB, as opções disponíveis devem incluir somente as combinações de tamanho e IOPS que resultam em < 0,30 IOPS/GB. 
- Se o volume original é Performance com >= 0,30 IOPS/GB, as opções disponíveis devem incluir somente as combinações de tamanho e de IOPS que resultem em >= 0,30 IOPS/GB. Tamanho (maior que ou igual ao volume original)

## Como o ajuste de IOPS afeta a replicação?

Se o volume tem a replicação em vigor, a réplica é atualizada automaticamente para corresponder à seleção de IOPS do primário. 

## Como posso ajustar o IOPS em meu Armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU no Catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Endurance (IOPS em camada): selecione uma Camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. Você pode aumentar a camada de IOPS a qualquer momento. No entanto, a diminuição está disponível somente uma vez por mês.
    - Performance (IOPS alocado): especifique a nova opção de IOPS para seu armazenamento inserindo um valor entre 100 a 48.000 IOPS. (Certifique-se de olhar quaisquer limites específicos requeridos por tamanho no formulário de ordem.)
4. Revise sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento deve estar disponível em alguns minutos.
