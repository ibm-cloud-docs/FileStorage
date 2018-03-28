---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 호스트 큐 깊이 설정에 대한 권장사항

각 성능 티어에 대해 최대 호스트 및 애플리케이션 입출력(I/O) 큐 깊이를 권장하고 있습니다. 호스트 설정은 디스크 및 제어기 대기 시간에 영향을 주지 않고, 호스트 및 애플리케이션이 관찰한 대기 시간에만 영향을 줍니다. 

권장 수치를 초과하는 큐 깊이는 호스트 I/O 대기 시간을 증가시킬 수 있으며, 권장 수치 미만의 큐 깊이는 호스트 I/O 성능을 줄일 수 있습니다. 각 애플리케이션은 서로 다르기 때문에 스토리지 성능을 최대화하려면 조정 및 관찰이 필요합니다. 

호스트 큐 깊이는 보통 호스트 버스 어댑터 드라이버 또는 하이퍼바이저에서 조정되며, 종종 애플리케이션에서 조정되기도 합니다. 32 또는 64와 같은 표준 기본값을 사용할 경우 호스트 또는 애플리케이션에서 과도한 대기 시간이 발생할 수 있습니다. 

하나의 호스트 또는 하이퍼바이저가 여러 성능 티어를 사용하는 경우 가장 빠른 티어에 대한 큐 깊이를 사용하고 가장 느린 성능 티어에서 대기 시간을 관찰하십시오. 최저 티어에서 대기 시간이 허용되지 않으면 모든 티어에서 대기 시간 및 성능의 균형이 관찰될 때까지 큐 깊이를 조정하십시오. 

<table align="center">
	<tbody>
		<tr>
			<td><strong>성능 티어</strong></td>
			<td style="text-align: center; vertical-align: middle;">0.25 IOPS/GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS/GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS/GB</td>
		</tr>
		<tr>
			<td><strong>최대 호스트 큐 깊이</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
