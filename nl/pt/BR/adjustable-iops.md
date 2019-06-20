---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: File Storage, adjusting IOPS, increase IOPS, decrease IOPS, modify IOPS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ajustando o IOPS
{: #adjustingIOPS}

Com esse novo recurso, os usuários de armazenamento do {{site.data.keyword.filestorage_full}} podem ajustar o IOPS de seu {{site.data.keyword.filestorage_short}} existente imediatamente. Eles não precisam criar uma duplicata nem copiar dados manualmente para um novo armazenamento. Os usuários não enfrentam nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste está ocorrendo.

O faturamento para o armazenamento é atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia total será faturada no próximo ciclo de faturamento.


## Vantagens de IOPS ajustável

- Gerenciamento de custo: alguns de nossos clientes podem precisar de IOPS alto apenas durante os horários de pico de uso. Por exemplo, uma grande loja de varejo tem pico de uso durante os feriados e pode precisar de IOPS mais alto em seu armazenamento do que no meio do verão. Esse recurso permite que eles gerenciem seus custos e paguem por IOPS mais altos somente quando precisam dele.

## Limitações
{: #limitsofadjustIOPS}

Esse recurso está disponível somente nos [data centers de seleção](/docs/infrastructure/FileStorage?topic=FileStorage-news).

Os clientes não podem alternar entre o Endurance e o Performance quando eles ajustam seu IOPS. Os usuários podem especificar um novo nível de IOPS ou camada de IOPS para seu armazenamento com base nos critérios e nas restrições a seguir.

- Se o volume original é a camada de 0,25 do Endurance, a camada de IOPS não pode ser atualizada.
- Se o volume original for Desempenho, com um valor menor ou igual a 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultarem em um valor menor ou igual a 0,30 IOPS/GB.
- Se o volume original for Desempenho, com mais de 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultarem em mais de 0,30 IOPS/GB.

## Efeito de ajuste de IOPS na replicação

Se o volume tiver a replicação em vigor, a réplica será atualizada automaticamente para corresponder à seleção de IOPS do primário.

## Ajustando o IOPS em seu Armazenamento
{: #adjustingsteps}

1. Acesse a sua lista de {{site.data.keyword.filestorage_short}}. No console do {{site.data.keyword.cloud}}, clique em **Infraestrutura clássica** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Para o Endurance (IOPS em camadas), selecione uma camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, o decréscimo está disponível somente uma vez por mês.
    - Para o Performance (IOPS alocado), especifique a nova opção de IOPS para seu armazenamento inserindo um valor no intervalo de 100 a 48.000 IOPS. (Certifique-se de olhar quaisquer limites específicos que são requeridos por tamanho no formulário de ordem.)
4. Revise sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Como alternativa, é possível atualizar seu IOPS por meio do SLCLI.
```
# slcli file volume-modify --help Usage: slcli file volume-modify [OPTIONS] VOLUME_ID

Opções: -c, --new-size INTEGER Novo tamanho de volume de arquivo em GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. Se o IOPS/GB original para o volume for maior que ou igual a 0,3, o novo IOPS/GB para o volume também deverá ser maior que ou igual a 0,3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. Se o IOPS/GB original para o volume for maior que 0,25, o novo IOPS/GB para o volume também deverá ser maior que 0,25.]
  -h, --help      Show this message and exit.
```
