---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: File Storage, modify volume, NFS, file storage, expand capacity

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Expandindo Capacidade de Compartilhamento de Arquivo
{: #expandCapacity}

Com esse novo recurso, os usuários atuais do {{site.data.keyword.filestorage_full}} são capazes de expandir o tamanho do {{site.data.keyword.filestorage_short}} em incrementos de GB de até 12 TB imediatamente. Eles não precisam criar uma duplicata nem migrar dados manualmente para um volume maior. Não há nenhuma indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento está ocorrendo.

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada do novo preço no ciclo de faturamento atual. Então, a nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers de seleção](/docs/infrastructure/FileStorage?topic=FileStorage-news).

## Vantagens de Armazenamento Expandível

- **Gerenciamento de custo** - Talvez você saiba que há potencial para crescimento de seus dados, mas que precisa de uma quantia menor de armazenamento para iniciar. A capacidade de expandir permite que os clientes economizem em custos de armazenamento no início e, em seguida, cresçam para acomodar suas necessidades.  

- **Necessidades de armazenamento crescente** - os clientes que experimentam um rápido crescimento precisam de uma maneira rápida e fácil de aumentar o tamanho de seu armazenamento para gerenciar esse crescimento.

## Efeitos da expansão da capacidade de armazenamento na Replicação

A ação de expansão no armazenamento primário resulta no redimensionamento automático da réplica.

## Limitações
{: #limitsofextension}

Esse recurso está disponível somente para armazenamento que é provisionado em [data centers](/docs/infrastructure/FileStorage?topic=FileStorage-news) com recursos aprimorados. O armazenamento criptografado provisionado nesses data centers pode ser aumentado em até 12 TB.

As limitações de tamanho existentes para o {{site.data.keyword.filestorage_short}} que foram provisionadas com o Endurance ainda se aplicam (até 4 TB para a camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Redimensionando o
{: #resizingsteps}

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.filestorage_short}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.filestorage_short}}**.
2. Selecione o volume na lista e clique em **Ações** > **Modificar volume**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise sua seleção e a nova precificação.
5. Clique em **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Como alternativa, é possível usar o comando a seguir no SLCLI.
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
