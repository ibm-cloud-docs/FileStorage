---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# IOPS ajustável

Com esse novo recurso, nossos usuários de armazenamento do {{site.data.keyword.filestorage_full}} podem ajustar o IOPS de seu {{site.data.keyword.filestorage_short}} existente imediatamente. Não há necessidade de criar uma duplicata ou migrar dados manualmente para um novo armazenamento. Os usuários não terão nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste estiver ocorrendo. 

O faturamento para o armazenamento é atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual. Então, a nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que você deve tirar vantagem de IOPS ajustável?

- Gerenciamento de custos - alguns dos nossos clientes podem precisar de IOPS alto somente durante os horários de pico de uso. Por exemplo, uma loja de varejo grande tem pico de uso durante os feriados e pode precisar de IOPS mais alto no armazenamento do que no meio do verão. Esse recurso permite que eles gerenciem seus custos e paguem mais IOPS somente quando eles realmente precisam disso.

## Há alguma limitação?

Esse recurso está disponível somente para armazenamento que é provisionado em [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados.

Os clientes não podem alternar entre o Endurance e o Performance quando eles ajustam seu IOPS. Os usuários podem especificar uma nova camada de IOPS ou nível de IOPS para o armazenamento com base nos critérios/restrições a seguir: 

- Se o volume original é a camada de 0,25 do Endurance, a camada de IOPS não pode ser atualizada.
- Se o volume original é Performance com < 0,30 IOPS/GB, as opções disponíveis incluem somente as combinações de tamanho e IOPS que resultam em < 0,30 IOPS/GB. 
- Se o volume original é Performance com >= 0,30 IOPS/GB, as opções disponíveis incluem somente as combinações de tamanho e IOPS que resultam em >= 0,30 IOPS/GB. 

## Como ajustar o IOPS afeta a replicação?

Se o volume tem a replicação em vigor, a réplica é atualizada automaticamente para corresponder à seleção de IOPS do primário. 

## Como posso ajustar o IOPS em meu armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Endurance (IOPS em camada): selecione uma Camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, a diminuição está disponível somente uma vez por mês.
    - Performance (IOPS alocado): especifique a nova opção de IOPS para seu armazenamento inserindo um valor no intervalo de 100 a 48.000 IOPS. (Certifique-se de olhar quaisquer limites específicos que são requeridos por tamanho no formulário de ordem.)
4. Revise sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.
