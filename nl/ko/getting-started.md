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

짧은 설명 섹션에는 시스템 관리자 또는 dev ops 엔지니어가 이 인프라 오퍼링이나 서비스를 사용하고자 하는 이유를 설명하는 1 - 2개의 문장이 포함되어야 합니다.
사용자의 학습 목표를 간략하게 언급하고 제목 및/또는 짧은 설명에 IBM Cloud, ServiceName 등의 SEO 키워드를 포함하십시오. 반드시 전통적인 스타일을 사용하십시오. 세부사항은 http://www.carbondesignsystem.com/guidelines/content/general 사이트에서 Carbon Design System의 전통적 스타일에 관한 지침을 참조하십시오. 

예제:
리소스 요구사항이 최대치인 경우, 제어 없이 해당 새 요구사항을 즉각적으로 충족하기 위해 용량 확장이 가능한 클라우드 인프라 서비스가 필요합니다. {{site.data.keyword.Bluemix}} 가상 서버는 사용자의 워크로드에 합당한 지리적 지역에서 선택된 가상 서버 이미지로부터 수 분 내에 배치될 수 있습니다. 워크로드가 진정되는 순간, 해당 가상 서버는 클라우드 환경이 인프라 요구사항에 완벽하게 맞도록 일시중단되거나 전원이 차단될 수 있습니다. 

태스크 섹션에는 디바이스, 스토리지 가져오기나 네트워크 시작 및 실행 단계가 포함되어 있습니다. 
- 태스크 기반의 기술 정보로 간결하고 직접적인 지시사항을 위해 전통적 스타일을 줄이십시오. 
- 인프라 서비스에 사용할 기본적이고 가장 흔히 사용하는 시나리오 단계를 포함하십시오. 
- Bluemix 카탈로그에서 서비스를 추가하는 단계는 포함하지 마십시오. 사용자가 서비스를 추가하는 UI의 단계를 이미 실행했다고 가정합니다. 
- VCAP 서비스 정보는 물론 복사 가능한 모든 언어의 코드 스니펫을 포함하십시오. VCAP 서비스 정보에 대한 정보는 https://console.ng.bluemix.net/docs/cli/vcapsvc.html 사이트를 참조하십시오. 
- 구성, 관리 등의 추가 태스크의 경우, "정보" 섹션 또는 태스크 섹션 아래의 태스크 섹션(## Gerund_task_title)을 추가하십시오(사용되는 경우). "x 구성", "y 관리", "z 관리" 등의 태스크 제목을 사용하십시오. -->

## 전제조건
관리자가 인프라 오퍼링을 프로비저닝하거나 관리하려면, 우선 관리자에게 업그레이드된 {{site.data.keyword.Bluemix}} 계정이 있어야 합니다. 자세한 정보는 [{{site.data.keyword.Bluemix_notm}} 및 SoftLayer 청구 계정 업그레이드 및 통합](../docs/admin/softlayerlink.html)을 참조하십시오. 

## 태스크 지향 제목 및 설명
이 인프라 서비스에서 빠르게 시작하고 실행하려면 다음 단계를 따르십시오. 또는 다음 단계를 완료하여 블록 스토리지 서비스를 시작하십시오. 

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

3. 3단계. 이 단계에는 단일 행 명령 예제가 있습니다. 문서 프레임워크에 의해 표시되는 경우, 여기에는 행 앞에 $가 표시되며 오른쪽에는 복사 단추가 있습니다. 복사 단추는 명령은 복사하지만 $는 복사하지 않습니다. 

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

