---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Novos locais e recursos do {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}

O {{site.data.keyword.BluSoftlayer_full}} está apresentando uma nova versão do {{site.data.keyword.blockstorageshort}} e do {{site.data.keyword.filestorage_full}}! O novo armazenamento está disponível em data centers selecionados e é suportado pelo armazenamento flash em níveis mais altos de IOPS com criptografia de nível de disco para dados em repouso. Todo o armazenamento provisionado nos data centers selecionados será provisionado automaticamente com a nova versão do {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}.

**Nota:** o ponto de montagem do NFS para novos volumes mudou. Veja **Novo ponto de montagem para volumes do {{site.data.keyword.filestorage_short}} criptogtafados ** abaixo para obter detalhes.

O novo {{site.data.keyword.filestorage_short}} está atualmente disponível em regiões/data centers a seguir com disponibilidade de data centers adicionais em breve!
<table style="width:100%;">
	<caption>Disponibilidade de data center</caption>
	<tbody>
		<tr>
			<td><strong>EUA 2</strong></td>
			<td><strong>UE</strong></td>
			<td><strong>Austrália</strong></td>
			<td><strong>Canadá</strong></td>
			<td><strong>América Latina</strong></td>
			<td><strong>Ásia-Pacífico</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				   SJC04<br />
					WDC04<br />
					WDC06<br />
					WDC07<br />
					DAL09<br />
					DAL10<br />
					DAL12<br />
					DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
					MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
				SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


O novo armazenamento tem os recursos e capacidades a seguir:

-  [Criptografia gerenciada por provedor para dados em repouso](block-file-storage-encryption-rest.html). Todo {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} será provisionado automaticamente como criptografado sem encargo adicional.
-  10 IOPS por opção de camada GB. Uma nova camada foi incluída no {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} do tipo Endurance para suportar as cargas de trabalho com maior demanda.
-  Armazenamento suportado todo em flash. O {{site.data.keyword.blockstorageshort}} e o {{site.data.keyword.filestorage_short}} provisionados com o Endurance ou Performance em 2 IOPS por GB ou superior com armazenamento todo em flash suportado.
-  O suporte de captura instantânea e replicação com o {{site.data.keyword.blockstorageshort}} e o {{site.data.keyword.filestorage_short}} provisionados com Endurance ou Performance.
-  Opção de faturamento por hora incluído para armazenamento que é planejado para ser usado por menos de um mês integral. 
-  Até 48.000 IOPS para o {{site.data.keyword.blockstorageshort}} e o {{site.data.keyword.filestorage_short}} provisionados com Performance.
-  As taxas de IOPS são ajustáveis para melhorar o desempenho em caso de mudanças sazonais de carga. Leia mais sobre esse recurso [aqui](adjustable-iops.html).
-  Crie um novo clone de seus dados com o [recurso de Duplicação de volume do {{site.data.keyword.filestorage_short}}](how-to-create-duplicate-volume.html).
- O armazenamento é expansível em incrementos de GB de até 12 TB instantaneamente, sem a necessidade de criar uma duplicata ou migrar manualmente os dados para um volume maior. Leia mais sobre esse recurso [aqui](expandable_file_storage.html).

## Novo ponto de montagem para volumes criptografados do {{site.data.keyword.filestorage_short}}

Todos os volumes criptografados do {{site.data.keyword.filestorage_short}} provisionados nestes data centers têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os volumes de armazenamento de arquivo criptografados e não criptografados, é possível visualizar as informações do ponto de montagem na página Detalhes do volume na UI, assim como acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Verifique novamente aqui para ver quando data centers adicionais foram submetidos a upgrade e para recursos e capacidades recentemente disponíveis que estão sendo incluídos para o {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}.
