---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-24"

---
{:new_window: target="_blank"}

# Ajustando o IOPS

Com esse novo recurso, os usuários de armazenamento do {{site.data.keyword.blockstoragefull}} podem ajustar o IOPS de seu {{site.data.keyword.blockstorageshort}} existente imediatamente. Eles não precisam criar uma duplicata nem copiar dados manualmente para um novo armazenamento. Os usuários não enfrentam nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste está ocorrendo. 

O faturamento para o armazenamento é atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia total será faturada no próximo ciclo de faturamento.


## Vantagens de IOPS ajustável

- Gerenciamento de custos - alguns dos nossos clientes podem precisar de IOPS alto somente durante os horários de pico de uso. Por exemplo, uma grande loja de varejo tem pico de uso durante os feriados e pode precisar de IOPS mais alto em seu armazenamento do que no meio do verão. Esse recurso permite que eles gerenciem seus custos e paguem por IOPS mais altos somente quando precisam dele.

## Limitações

Esse recurso está disponível somente nos [data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html). 

Os clientes não podem alternar entre o Endurance e o Performance quando eles ajustam seu IOPS. Os usuários podem especificar uma nova camada de IOPS ou nível de IOPS para seu armazenamento com base nos critérios/restrições a seguir.

- Se o volume original é a camada de 0,25 do Endurance, a camada de IOPS não pode ser atualizada.
- Se o volume original for Performance com um valor menor que 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultem em um valor menor que 0,30 IOPS/GB. 
- Se o volume original for Performance com um valor maior ou igual a 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultem em um valor maior ou igual a 0,30 IOPS/GB. 

## Efeito de ajuste de IOPS na replicação

Se o volume tiver a replicação em vigor, a réplica será atualizada automaticamente para corresponder à seleção de IOPS do primário. 

## Ajustando o IOPS em seu Armazenamento

1. Acesse sua lista de {{site.data.keyword.blockstorageshort}}
    - No portal do cliente, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU
    - No catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**. 
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Endurance (IOPS em camada): selecione uma Camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, o decréscimo está disponível somente uma vez por mês.
    - Performance (IOPS alocado): especifique a nova opção de IOPS para seu armazenamento inserindo um valor no intervalo de 100 a 48.000 IOPS. (Certifique-se de olhar quaisquer limites específicos que são requeridos por tamanho no formulário de ordem.)
4. Revise sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.
