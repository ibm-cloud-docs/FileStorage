---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Recomendação para as configurações de profundidade da fila do host

Recomendamos uma profundidade máxima da fila de entrada/saída (E/S) do host e do aplicativo para cada camada de desempenho. A configuração de host não afeta a latência de disco e de controlador, somente a latência observada pelo host e aplicativo.

A profundidade da fila acima dos números recomendados pode aumentar a latência de E/S do host; enquanto a profundidade da fila abaixo do número recomendado pode reduzir o desempenho de E/S do host. Como cada aplicativo é diferente, o ajuste e a observação são necessários para alcançar o desempenho máximo de armazenamento.

A profundidade da fila do host é geralmente ajustada no driver do Adaptador de barramento de host ou no hypervisor e, às vezes, no aplicativo. Os padrões, como 32 ou 64, podem causar latência excessiva do host ou aplicativo.

Se um host ou hypervisor está usando múltiplas camadas de desempenho, use a profundidade da fila para a camada mais rápida e observe a latência na camada de desempenho mais lenta. Se a latência na camada mais baixa é inaceitável, ajuste a profundidade da fila até que o balanceamento entre a latência e o desempenho para todas as camadas seja observado.

<table align="center">
	<tbody>
		<tr>
			<td><strong>Camada de desempenho</strong></td>
			<td style="text-align: center; vertical-align: middle;">0,25 IOPS por GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS por GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS por GB</td>
		</tr>
		<tr>
			<td><strong>Profundidade máxima da fila do host</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
