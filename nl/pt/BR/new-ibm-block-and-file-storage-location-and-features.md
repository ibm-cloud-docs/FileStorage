---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: File Storage, file storage, NFS, locations, data centers

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Novos Locais e Recursos
{: #news}

O {{site.data.keyword.BluSoftlayer_full}} está introduzindo uma nova versão do
{{site.data.keyword.filestorage_full}}. O novo armazenamento está disponível em data centers selecionados é suportado pelo armazenamento flash a níveis de IOPS mais altos com criptografia para dados em repouso no nível de disco. Todo o armazenamento pedido nos data centers selecionados são criados automaticamente com a nova versão do {{site.data.keyword.filestorage_short}}.

O ponto de montagem NFS para novos volumes foi mudado. Consulte a seção [Novo ponto de montagem para volumes aprimorados do {{site.data.keyword.filestorage_short}}](#new-mount-point-for-enhanced-file-storage-volumes) para obter detalhes.
{:important}

## Novos Locais
{: #new-locations}

O novo {{site.data.keyword.filestorage_short}} está disponível nas regiões e nos data centers a seguir com mais disponibilidade de data center incluída posteriormente.

<table role="presentation">
  <tr>
    <td><strong>EUA 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Austrália</strong></td>
    <td><strong>Canadá</strong></td>
    <td><strong>América Latina</strong></td>
    <td><strong>Ásia-Pacífico</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*A Tabela 1 mostra nossa Disponibilidade do Data center. Cada região possui sua própria coluna. Algumas cidades, como Dallas, São José, Washington DC, Amsterdã, Frankfurt, Londres e Sydney, têm múltiplos data centers.*

## Novos recursos e capacidades
{: #features}

- [Criptografia gerenciada por provedor para dados em repouso](/docs/infrastructure/FileStorage?topic=FileStorage-encryption). <br/> Todos os volumes {{site.data.keyword.filestorage_short}} são provisionados automaticamente como criptografados sem encargo adicional.
- 10 IOPS por opção de camada GB. <br/> Uma nova camada foi incluída no {{site.data.keyword.filestorage_short}} de tipo Endurance para suportar as cargas de trabalho de maior demanda.
- Armazenamento suportado todo em flash. <br/> O {{site.data.keyword.filestorage_short}} que é provisionado com as opções Endurance ou Performance em 2 IOPS por GB ou superior é suportado por armazenamento totalmente em flash.
- Suporte de Captura instantânea e Replicação.
- Opção de faturamento por hora incluído para armazenamento que é planejado para ser usado por menos de um mês integral.
- Até 48.000 IOPS para o {{site.data.keyword.filestorage_short}} provisionado com o tipo Performance.
- As taxas de IOPS são ajustáveis para melhorar o desempenho de mudanças sazonais de carga. Leia mais sobre esse recurso [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-adjustingIOPS).
- Crie um clone de seus dados com o [Recurso de duplicação de volume do {{site.data.keyword.filestorage_short}}](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume).
- O armazenamento é expansível em incrementos de GB até 12 TB imediatamente, sem a necessidade de criar uma duplicata ou de mover manualmente os dados para um volume maior. Leia mais sobre esse recurso [aqui](/docs/infrastructure/FileStorage?topic=FileStorage-expandCapacity).

## Novo ponto de montagem para volumes aprimorados do {{site.data.keyword.filestorage_short}}

Todos os volumes aprimorados do {{site.data.keyword.filestorage_short}} provisionados nesses data centers têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os volumes de armazenamento, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** no console. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Para poder acessar todos os novos recursos, selecione `Storage-as-a-Service Package 759` ao fazer seu pedido por meio da API. Para obter mais informações sobre como pedir o {{site.data.keyword.filestorage_short}} por meio da API, consulte [order_file_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/file/#SoftLayer.managers.file.FileStorageManager.order_file_volume){: external}.
{:important}

Consulte esta página novamente para ver quando mais data centers serão submetidos a upgrade e os novos recursos e capacidades que estão sendo incluídos no {{site.data.keyword.filestorage_short}}.
{:tip}
