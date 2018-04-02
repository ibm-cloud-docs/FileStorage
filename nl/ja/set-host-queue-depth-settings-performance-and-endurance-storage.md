---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# ホストのキュー長の設定に関する推奨事項

ホストとアプリケーションの入出力 (I/O) キュー長は各パフォーマンス・ティアの最大にすることを推奨します。ホストの設定は、ディスクとコントローラーの待ち時間には影響しません。ホストとアプリケーションで見られる待ち時間にだけ影響します。

キュー長を推奨値より大きくするとホスト入出力待ち時間が増えることがありますが、推奨値より小さくするとホストの入出力パフォーマンスが低下する可能性があります。アプリケーションごとに異なるため、最大のストレージ・パフォーマンスを実現するには調整と監視が必要です。

ホストのキュー長は、通常はホスト・バス・アダプター・ドライバーまたはハイパーバイザーで調整されますが、アプリケーションで調整されることもあります。32 や 64 などの標準のデフォルトでは、ホストまたはアプリケーションに過度の待ち時間が発生する場合があります。

1 つのホストまたはハイパーバイザーで複数のパフォーマンス・ティアを使用する場合は、最も速いティアのキュー長が使用され、最も遅いパフォーマンス・ティアの待ち時間になります。最も遅いティアの待ち時間が許容できない場合は、キュー長を調整してすべてのティアの待ち時間とパフォーマンスのバランスを確認してください。

<table align="center">
	<tbody>
		<tr>
			<td><strong>パフォーマンス・ティア</strong></td>
			<td style="text-align: center; vertical-align: middle;">GB あたり 0.25 IOPS</td>
			<td style="text-align: center; vertical-align: middle;">GB あたり 2 IOPS</td>
			<td style="text-align: center; vertical-align: middle;">GB あたり 4 IOPS</td>
		</tr>
		<tr>
			<td><strong>ホストのキューの最大長</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
