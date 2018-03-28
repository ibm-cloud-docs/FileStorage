---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# {{site.data.keyword.filestorage_short}} 시작하기

간단한 설명 섹션에는 시스템 관리자 또는 개발 운영 엔지니어가 이 인프라 오퍼링이나 서비스를 사용하려고 하는 이유에 대해 설명하는 한두 문장이 포함되어야 합니다. 사용자의 학습 목표에 대해 간단히 언급하고 제목 및/또는 간단한 설명에 IBM Cloud, ServiceName과 같은 SEO 키워드를 포함합니다. 대화형 스타일을 사용해야 합니다. 자세한 내용은 Carbon Design System의 기존 스타일에 대한 안내(http://www.carbondesignsystem.com/guidelines/content/general)를 참조하십시오. 

예제:
리소스 요청이 급증하는 경우 이러한 새로운 요구사항을 즉시 충족할 수 있는 스케일 확장 가능한 클라우드 인프라 서비스가 필요하며 이를 제어할 수 있어야 합니다. {{site.data.keyword.Bluemix}} 가상 서버는 워크로드에 적합한 지리적 지역에서 사용자가 선택한 가상 서버 이미지로부터 몇 분 안에 배치할 수 있습니다. 워크로드가 진정되는 대로 클라우드 환경이 인프라 요구사항을 완전히 충족하도록 이러한 가상 서버를 일시중단하거나 종료할 수 있습니다.

태스크 섹션은 디바이스, 스토리지 또는 네트워킹을 시작하고 실행하는 단계를 포함합니다. 
- 태스크 기반 기술 정보를 통해 간결하고 직접적인 지시를 선호하며 대화형 스타일을 자제합니다. 
- 인프라 서비스를 사용하기 위한 기본적이며 가장 공통적으로 사용하는 시나리오 단계를 포함하십시오. 
- Bluemix 카탈로그의 서비스를 추가하는 단계를 포함하지 마십시오. 여기에서는 사용자가 서비스를 추가하기 위해 이미 UI에 단계를 수행했다고 가정합니다. 
- VCAP 서비스 정보뿐만 아니라 복사 가능한 코드 스니펫을 모든 언어에 포함하십시오. VCAP 서비스 정보에 대해서는 다음을 참조하십시오. https://console.ng.bluemix.net/docs/cli/vcapsvc.html
- 구성, 관리 등과 같은 추가 태스크에 대해 태스크 섹션 또는 "정보" 섹션(사용되는 경우) 아래에 태스크 섹션(## Gerund_task_title)을 추가하십시오. "x 구성", "y 관리", "x 관리"와 같은 태스크 제목을 사용하십시오. -->

## 전제조건
관리자가 해당 인프라 오퍼링을 프로비저닝 또는 관리하기 전에 관리자는 {{site.data.keyword.Bluemix}} 계정을 업그레이드해야 합니다. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} 및 SoftLayer 청구 계정 업그레이드 및 통합](../docs/admin/softlayerlink.html)을 참조하십시오. 

## 태스크 중심 제목 및 설명
이 인프라 서비스를 빠르게 시작하고 실행하려면 다음 단계를 수행하십시오. 또는,
다음 단계를 완료하여 블록 스토리지 서비스를 시작하십시오. 

<!-- Use ordered list markup for the step section. For code examples:
- use three backticks ahead of and after the example (```)
- For copyable code snippet, multi-line, include {: codeblock} following the last set of backticks. A copy button will display in framework in output.
- For copyable command, single line, include {: pre} following the last set of backticks. When displayed, it will show "$" at the beginning of the command example and a copy button, but the copy button will include just the command example.
- For non-copyable output snippet, include {: screen} following the last set of backticks.
 -->

1. 서비스를 설정하는 1단계.
2. 서비스를 설정하는 2단계.

	```
	Copyable example for this step.
	This example might be multiline code
	to copy into a file.
	When displayed in the doc framework,
	it will have a copy button on the right.
	The user can click to copy the example
	so they can paste it into their code editor.
	```
	{: codeblock}

3. 3단계. 이 단계에서는 단일 행 명령 예제를 사용합니다. doc 프레임워크로 표시되는 경우 행 시작 위치에 $가 표시되며, 오른쪽에 복사 단추가 있습니다. 복사 단추는 $가 아닌 명령을 복사합니다. 

	```
	my command -and -options
	```
	{: pre}

4. 4단계
	```
	This is a bunch of output from
		a command or program I ran
			and it can run lots of lines
			and the doc framework will show it as
			output with no copy button.
	```
	{: screen}

## 다음 단계

