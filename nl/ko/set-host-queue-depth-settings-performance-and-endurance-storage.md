---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 호스트 큐 깊이 설정에 대한 권장사항

각 성능 계층에 대해 최대 호스트 및 애플리케이션 입출력(I/O) 큐 깊이를 권장합니다. 호스트 설정은 디스크와 제어기 대기 시간에 영향을 주지 않으며, 오직 호스트와 애플리케이션이 관찰하는 대기 시간에만 영향을 줍니다.

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

큐 깊이가 권장 수치를 초과하면 호스트 I/O 대기 시간이 늘어날 수 있는 한편, 큐 깊이가 권장 수치를 하회하면 호스트 I/O 성능이 떨어질 수 있습니다. 각 애플리케이션이 서로 다르므로 조정과 관찰을 통해 최대 스토리지 성능에 도달해야 합니다.

호스트 큐 깊이는 일반적으로 호스트 버스 어댑터 드라이버 또는 하이퍼바이저(그리고 가끔은 애플리케이션)에서 조정됩니다. 32 또는 64 등의 표준 기본값을 사용하면 과도한 호스트 또는 애플리케이션 대기 시간이 발생할 수 있습니다.

하나의 호스트 또는 하이퍼바이저가 다수의 성능 계층을 사용하는 경우에는 가장 빠른 계층의 큐 깊이를 사용하고 가장 느린 성능 계층의 대기 시간을 관찰하십시오. 최하위 계층의 대기 시간이 허용될 수 없는 경우에는 모든 계층에 대해 성능과 대기 시간의 밸런스가 관찰될 때까지 큐 깊이를 조정하십시오.
