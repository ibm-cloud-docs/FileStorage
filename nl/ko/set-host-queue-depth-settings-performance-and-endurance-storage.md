---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 호스트 큐 깊이 설정 조정
{: #hostqueuesettings}

{{site.data.keyword.BluSoftlayer_full}}에서는 각 성능 계층에 대한 최대 호스트 및 애플리케이션 입출력(I/O) 큐 깊이를 제안합니다.

<table align="center">
  <caption>각 IOPS 계층에 권장되는 큐 깊이</caption>
        <thead>
	    <tr>
		<th>성능 계층</th>
		<th>최대 호스트 큐 깊이</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0.25IOPS/GB</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2IOPS/GB</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4IOPS/GB</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>


호스트 설정은 디스크 및 제어기 대기 시간에 영향을 주지 않습니다. 호스트와 애플리케이션이 관찰하는 대기 시간에만 영향을 줍니다.

큐 깊이가 나열된 숫자를 초과하면 호스트 I/O 대기 시간이 늘어날 수 있습니다. 큐 깊이가 나열된 숫자에 미치지 못하는 경우에는 호스트 I/O 성능이 떨어질 수 있습니다. 각 애플리케이션이 서로 다르므로 조정과 관찰을 통해 최대 스토리지 성능에 도달해야 합니다.

호스트 큐 깊이는 일반적으로 호스트 버스 어댑터 드라이버 또는 하이퍼바이저(그리고 가끔은 애플리케이션)에서 조정됩니다. 32 또는 64 등의 표준 기본값은 과도한 호스트 또는 애플리케이션 대기 시간을 발생시킬 수 있습니다.

하나의 호스트 또는 하이퍼바이저가 다수의 성능 계층을 사용하는 경우에는 가장 빠른 계층의 큐 깊이를 사용하고 가장 느린 성능 계층의 대기 시간을 관찰하십시오.

최하위 계층의 대기 시간이 허용할 수 없는 수준인 경우에는 모든 계층에서 성능과 대기 시간이 균형을 이룰 때까지 큐 깊이를 조정하십시오.
