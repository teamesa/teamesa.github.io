---
layout:       post
title:        "쿠버네티스 시작하기"
author:       "yunki kim"
header-style: text
catalog:      true
tags:
- kubernetes
---

<div class="tt_article_useless_p_margin contents_style"><p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 쿠버네티스를 검색하면 "쿠버네티스는 컨테이너를 오케스트레이션 하는 도구"라고 정의하고 있는 글을 심심치 않게 볼 수 있다. 하지만, 컨테이너가 뭔지, 오케스트레이션이 뭔지를 정확히 알지 못하기 때문에 저 문장 자체를 명확히 이해하지 못한다. 우선 저 문장을 이해하기 위해 필요한 개념들을 살펴보자.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 쿠버네티스를 이해하는 대 필요한 용어들을 정리하면 다음과 같다.</span></p>
<table style="border-collapse: collapse; width: 100%; height: 103px;" border="1" data-ke-align="alignLeft">
<tbody>
<tr style="height: 18px;">
<td style="width: 18.8372%; height: 18px;"><span style="font-family: 'Noto Serif KR';">용어</span></td>
<td style="width: 81.1628%; height: 18px;"><span style="font-family: 'Noto Serif KR';">뜻</span></td>
</tr>
<tr style="height: 17px;">
<td style="width: 18.8372%; height: 17px;"><span style="font-family: 'Noto Serif KR';">컨테이너</span></td>
<td style="width: 81.1628%; height: 17px;"><span style="font-family: 'Noto Serif KR';">앱이 구동되는 환경까지 감싸서 실행할 수 있도록 하는 격리 기술</span></td>
</tr>
<tr style="height: 17px;">
<td style="width: 18.8372%; height: 17px;"><span style="font-family: 'Noto Serif KR';">컨테이너 런타임</span></td>
<td style="width: 81.1628%; height: 17px;"><span style="font-family: 'Noto Serif KR';">컨테이너를 다루는 도구</span></td>
</tr>
<tr style="height: 17px;">
<td style="width: 18.8372%; height: 17px;"><span style="font-family: 'Noto Serif KR';">도커</span></td>
<td style="width: 81.1628%; height: 17px;"><span style="font-family: 'Noto Serif KR';">컨테이너를 다루는 도구 중 가장 유명한 것</span></td>
</tr>
<tr style="height: 17px;">
<td style="width: 18.8372%; height: 17px;"><span style="font-family: 'Noto Serif KR';">쿠버네티스</span></td>
<td style="width: 81.1628%; height: 17px;"><span style="font-family: 'Noto Serif KR';">컨테이너 런타임을 통해 컨테이너를 오케스트레이션 하는 도구</span></td>
</tr>
<tr style="height: 17px;">
<td style="width: 18.8372%; height: 17px;"><span style="font-family: 'Noto Serif KR';">오케스트레이션</span></td>
<td style="width: 81.1628%; height: 17px;"><span style="font-family: 'Noto Serif KR';">여러 서버에 걸친 컨테이너 및 사용하는 환경 설정을 관리하는 행위. 여러 시스템 전반에서 다양한 단계를 필요로 하는 프로세스 또는 워크플로우를 자동화 하는 방식.</span></td>
</tr>
</tbody>
</table>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위 용어를 풀어서 "쿠버네티스는 컨테이너를 오케스트레이션 하는 도구"라는 정의에 대입하면 다음과 같이 쿠버네티스를 정의할 수 있다. "쿠버네티스는 앱이 구동되는 환경까지 감싸 실행하는 격리 기술이 여러 서버에 있을 때 이 기술과 환경 설정을 관리하는 도구다." 이전의 정의보다는 조금 더 이해하기 쉬워졌지만 여전히 다음과 같은 의문이 든다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - "앱이 구동되는 환경까지 감싸 실행하는 격리 기술"을 왜 VM이 아닌 컨테이너라 칭하는가? VM과 컨테이너는 어떤 차이가 존재하나?</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 여러 서버에 걸친 컨테이너를 어떻게 관리하는가?</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 어떤 환경 설정을 관리하는가?</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 이제부터 이 의문들을 하나씩 파해쳐보자.</span></p>
<h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>컨테이너</b></span></h2>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>컨테이너와 가상머신</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 컨테이너와 가상머신의 구조적 차이는 다음과 같다.</span></p>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="1208" data-origin-height="391"><span data-url="https://blog.kakaocdn.net/dn/bBCjCF/btrWRdiIdBq/GMPNLtAKJa4B3x1rFIW9s0/img.png" data-lightbox="lightbox"><img src="/img/2023-01-23-kubernetes-begin/img.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBCjCF%2FbtrWRdiIdBq%2FGMPNLtAKJa4B3x1rFIW9s0%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" data-origin-width="1208" data-origin-height="391"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 위 구조를 보면 가장 눈에 띄는 부분이 OS 유무다. 가상 머신은 OS를 띄우는 반면, 컨테이너 구조는 별도의 OS를 띄우지 않는다. 즉, 컨테이너로 실행된 프로세스는 커널을 공유한다. 따라서 실행 속도가 빠르고 성상 손실이 거의 없다. 그에 반해 가상머신은 완전한 하나의 컴퓨터다. 따라서 애플리케이션 구동을 위해 H/W를 OS 위에서 애뮬레이션 하고, 매번 OS를 설치해야 하고, 리소스를 할당해줘야 하기 때문에 느리다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위 구조에서 Bin/Library는 애플리케이션 실행에 필요한 환경과 관련된 파일이며 hypervisor는 VM을 생성하고 구동하는 소프트웨이며 CPU, 메모리 등의 리소스를 처리하는 풀이다.&nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 비록 컨테이너가 하나의 커널을 공유하지만 컨테이너 입장에서는 독립적인 환경 처럼 보이고 호스트 머신 입장에서는 프로세스로 인식된다. 이는 컨테이너가 리눅스 기준 리눅스 namespace 등의 격리 기술을 사용하는 운영체제 수준의 가상화 기술을 사용하기 때문이다. namespace는 프로세스를 실행할 때 시스템의 리소스를 분리해서 실행할 수 있게 도와주는 기능이다. namespace에 대해서는 별도의 포스팅으로 다루겠다.</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>리눅스 컨테이너</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위에서 언급했듯 리눅스 컨테이너는 운영체제 수준의 가상화를 사용하기 때문에 성능 손실이 거의 없다(속도가 빠르고 효율적이다). 이 이점과 별개로 다음과 같은 이점도 가지고 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>운영체제 수준의 가상화</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 별도의 하드웨어 에뮬레이션 없이 리눅스 커널을 공유하기 때문에 게스트 OS가 필요하지 않다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>높은 이식성</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 모든 컨테이너는 파일로 구성된 독자적인 실행 환경을 가지고 있다. 이는 이미지 형식으로 공유될 수 있기에 리눅스 커널을 사용하고 같은 컨테이너 런타임을 사용한다면 컨테이너 실행 환경을 공유하고 재현할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>stateless</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 하나의 컨테이너가 실행되는 환경은 다른 컨테이너에게 영향을 주지 않는다. 이미지 기반으로 컨테이터를 실행한다면 특정 실행 환경을 쉽게 재사용할 수 있다.&nbsp;</span></p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>리눅스 컨테이너를 사용하는 이유</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp; </b>기존 서버 환경에서는 애플리케이션 실행을 위해 서버 컴퓨터의 상태를 지속적으로 관리해야 했다. 또 한, 사용하는 소프트웨어의 버전을 관리하는 리소스도 상당했다. 위에서 설명했듯이 컨테이너는 높은 이식성과 stateless라는 가지고 있기 때문에 컨테이너를 이용하면 애플리케이션 별로 독자적인 환경을 준비하고 관리하는 것이 가능해 셔 서버 관리가 수월해진다. 예를 들어 다음과 같은 것들이 가능해진다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - CI/CD: 컨테이너 이미지를 빌드해 배포하고 빠르게 롤백할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 환경의 일관성: 로컬이든 클라우드든 어디에서나 동일한 구동을 보장한다.</span></p>
<h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>쿠버네티스</b></span></h2>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위에서 언급했듯이 쿠버네티스는 컨테이너 및 사용하는 환경 설정을 관리하는 행위(오케스트레이션)를 가능케 하는 도구다. 여기서 말하는 행위는 다음과 같은 것들을 포함한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>서비스 디스커버리와 로드 밸런싱</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 쿠버네티스는 DNS 이름을 사용하거나 자체 IP 주소를 사용해 컨테이너를 노출할 수 있다. 컨테이너에 대한 트래픽이 많으면, 네트워크 트래픽을 로드밸런싱 하고 배포해서 배포가 안정적으로 이루어질 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>스토리지 오케스트레이션</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 원하는 저장소 시스템을 자동으로 탑재할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>자동화된 빈 패킹(bin packing)</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>컨테이너화된 작업을 실행하는데 사용할 수 있는 클러스터 노드를 제공한다. 각 컨테이너가 필요로 하는 CPU와 RAM을 알려주면 컨테이너를 노드에 맞추어서 리소스를 가장 잘 활용하게 해 준다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>자동화된 복구(self-healing)</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>실패한 컨테이너를 자동으로 재시작하고, 교체한다. 헬스체크(사용자 정의 상태 검사)에 응답하지 않는 컨테이너를 자동으로 죽인다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>시크릿과 구성 관리</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>컨테이너 이미지를 재구성하지 않고 스택 구성에 시크릿을 노출하지 않고도 시크릿과 애플리케이션 구성을 배포 및 업데이트할 수 있다.</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>쿠버네티스 기본 사용법</b></span></h3>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>쿠버네티스 클러스터</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 쿠버네티스 클러스터는 컨테이너화된 애플리케이션을 실행하는 노드라는 워커 머신의 집합이다. 한 개 이상의 노드로 이루어져 있다. 클러스터 구조는 다음과 같다.</span></p>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="1276" data-origin-height="676"><span data-url="https://blog.kakaocdn.net/dn/qSrNW/btrWS3Godi6/qHb9jYTldw6odDdnJkJM1K/img.png" data-lightbox="lightbox"><img src="/img/2023-01-23-kubernetes-begin/img_1.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqSrNW%2FbtrWS3Godi6%2FqHb9jYTldw6odDdnJkJM1K%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" data-origin-width="1276" data-origin-height="676"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - Control Plane: 클러스터 관리는 담당한다. 애플리케이션 스케줄링, 항상성 유지, rolling out 등에 관여한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - Node: 쿠버네티스 클러스터 내 워커 머신으로 동작하는 VM 또는 물리적 컴퓨터다. 각 노드는 control plane과 통신하는 kublet이라는 에이전트를 갖는다. 노드는 control plane이 제공하는 쿠버네티스 API를 통해 control plane과 통신한다. 최종 사용자 역시 쿠버네티스 API를 통해 클러스터와 상호작용할 수 있다.</span></p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>minikube를 이용한 실습</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; minikube는 가벼운 쿠버네티스 구현체이다. 하나의 노드로 구성된 클러스터를 생성한다. 설치 후 다음과 같은 명령어를 통해 클러스터를 구동시킬 수 있다.</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">minikube&nbsp;start</span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 이제 kubectl이라는 CLI를 이용해 쿠버네티스와 상호작용 해보자.</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">2</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">3</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;version&nbsp;<span style="color: #999999;">#&nbsp;설치된&nbsp;클라이언트와&nbsp;서버(쿠버네티스)의&nbsp;버전을&nbsp;확인한다.</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;cluster<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>info&nbsp;<span style="color: #999999;">#&nbsp;클러스터&nbsp;정보를&nbsp;조회한다</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;get&nbsp;nodes&nbsp;<span style="color: #999999;">#&nbsp;애플리케이션을&nbsp;호스팅&nbsp;할&nbsp;수&nbsp;있는&nbsp;모든&nbsp;노드를&nbsp;보여준다.</span></span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size18"><span style="font-family: 'Noto Serif KR';"><b>애플리케이션 배포하기</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 클러스터 구동 후 컨테이너화된 애플리케이션을 배포하기 위해선 deployment 설정을 생성해야 한다. deployment는 쿠버네티스가 애플리케이션을 생성하는 방식과 업데이터 방식을 정의한다. control plane은 deployment에 존재하는 애플리케이션 인스턴스가 노드에서 실행되게 스케줄 한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; Deployment controller는 생성된 애플리케이션 인스턴스를 모니터링하고 인스턴스를 구동하는 노드 장애에 대비한 자동 복구 메커니즘을 제공한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; Deployment 생성 시 컨테이너 이미지와 복제 수를 지정해야 한다. 이 설정은 추후 deployment 업데이트로 변경할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>kubectl 기본</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; kubectl 문법은 "kubectl action resource"형태를 가진다 위에서 서술한 "kubectl get nodes"의 경우 "get"이 action이고 "nodes"가 resource이다. 또 한 "kubectl action resource --help"를 통해 추가 옵션을 확인할 수도 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 다음과 같은 커맨드를 통해 애플리케이션을 쿠버네티스 상에서 배포할 수 있다.</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">2</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">3</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">4</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">5</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">6</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">#&nbsp;kubernets-bootcamp&nbsp;는&nbsp;deployment&nbsp;이름</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">#&nbsp;--image&nbsp;옵션으로&nbsp;배포에&nbsp;사용&nbsp;될&nbsp;이미지를&nbsp;지정한다.&nbsp;url은&nbsp;레파지토리&nbsp;url이다.</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;create&nbsp;deployment&nbsp;kubernetes<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>bootcamp&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>image<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>gcr.io<span style="color: #0086b3;"></span><span style="color: #a71d5d;">/</span>google<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>samples<span style="color: #0086b3;"></span><span style="color: #a71d5d;">/</span>kubernetes<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>bootcamp:v1</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">#&nbsp;deployment&nbsp;항목&nbsp;조회</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;get&nbsp;deployments</span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 쿠버네티스 내부에서 실행되는 팟(pod)은 독립된 네트워크에 존재한다. 따라서 같은 클러스터 내에 존재하는 다른 팟들과 서비스들은 해당 팟을 접근할 수 있지만, 외부에서는 접근할 수 없다. kuberctl을 사용하는 것은 API 엔드포인트를 통해 애플리케이션과 상호작용한다는 의미다.&nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; kuberctl은 다음과 같은 명령어로 클러스터 내부의 프라이빗 네트워크로 통하는 프락시를 생성한다.&nbsp;</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">2</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">#&nbsp;호스트와&nbsp;클러스터&nbsp;간의&nbsp;커넥션을&nbsp;생성한다,&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;proxy</span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 쿠버네티스 API는 각 팟에 대한 엔드 포인트를 자동으로 생성한다. 따라서 팟 이름과 생성한 프락시를 이용해 각 팟에 접근할 수 있다.</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">2</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">3</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">4</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">5</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">#&nbsp;실행&nbsp;중인&nbsp;팟&nbsp;리스트</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;get&nbsp;pods</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">#&nbsp;팟&nbsp;내부&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;<span style="color: #066de2;">exec</span>&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>it&nbsp;POD_NAME&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">/</span>bin<span style="color: #0086b3;"></span><span style="color: #a71d5d;">/</span>bash</span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>팟(pod)과 노드(node)</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 팟은 한 개 이상의 애플리케이션 컨테이너로 이루어진 추상적 개념이며 쿠버네티스 애플리케이션에 최소 단위다. 같은 팟에 존재하는 컨테이너들은 다음과 같은 리소스를 공유한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 볼륨 등 공유 스토리지</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 클러스터 IP 등 네트워킹</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 컨테이너 이미지 버전, 사용할 포트 등 각 컨테이너 동작 방식에 대한 정보</span></p>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="346" data-origin-height="354"><span data-url="https://blog.kakaocdn.net/dn/piqVH/btrWVLk6dXX/J1UimPw2MgGWdH0AMXq1Hk/img.png" data-lightbox="lightbox"><img src="/img/2023-01-23-kubernetes-begin/img_2.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpiqVH%2FbtrWVLk6dXX%2FJ1UimPw2MgGWdH0AMXq1Hk%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" width="223" height="228" data-origin-width="346" data-origin-height="354"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 팟은 노드 상에서 동작한다. 노드는 쿠버네티스의 워커 머신을 의미한다. 하나의 노드는 여러 개의 팟을 가질 수 있고, 쿠버네티스 control plane은 클러스터 내 노드를 통해 팟에 대해 자동으로 스케줄링한다. 이때의 스케줄링은 각 노드가 사용 가능한 리소스를 고려해 발생한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 모든 쿠버네티스 노드는 다음과 같은 동작을 한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - kubelet은, 쿠버네티스 control plane과 노드 간 통신을 담당하는 프로세스이며, 하나의 머신 상에서 동작하는 팟과 컨테이너를 관리한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 컨테이너 런타임은 레지스트리에서 컨테이너 이미지를 가져와 묶여 있는 것을 풀고 애플리케이션을 동작사키는 책임을 맡는다.</span></p>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="800" data-origin-height="536"><span data-url="https://blog.kakaocdn.net/dn/k7vdI/btrWUIa3ZZQ/OKxKbUmmQPUutp63LdkVQK/img.png" data-lightbox="lightbox"><img src="/img/2023-01-23-kubernetes-begin/img_2.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk7vdI%2FbtrWUIa3ZZQ%2FOKxKbUmmQPUutp63LdkVQK%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" width="495" height="332" data-origin-width="800" data-origin-height="536"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 다음은 배포된 애플리케이션 운용에 필요한 기본적인 명령어다.</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">2</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">3</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">4</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">5</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">6</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;get&nbsp;<span style="color: #999999;">#&nbsp;자원을&nbsp;나열한다</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;describe&nbsp;<span style="color: #999999;">#&nbsp;자원에&nbsp;대해&nbsp;상세한&nbsp;정보를&nbsp;보여준다.</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;logs&nbsp;<span style="color: #999999;">#&nbsp;팟&nbsp;내&nbsp;컨테이너의&nbsp;로그들을&nbsp;출력한다</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;<span style="color: #066de2;">exec</span>&nbsp;<span style="color: #999999;">#&nbsp;팟&nbsp;내&nbsp;컨테이너에&nbsp;대한&nbsp;명령을&nbsp;실행한다.</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;<span style="color: #066de2;">exec</span>&nbsp;POD_NAME&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>&nbsp;env&nbsp;<span style="color: #999999;">#&nbsp;팟&nbsp;내의&nbsp;환경&nbsp;변수&nbsp;조회</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">kubectl&nbsp;<span style="color: #066de2;">exec</span>&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>it&nbsp;POD_NAME&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>&nbsp;bash&nbsp;<span style="color: #999999;">#&nbsp;팟&nbsp;내부&nbsp;접속</span></span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size18"><span style="font-family: 'Noto Serif KR';"><b><b>앱 노출을 위해 서비스 이용하기</b></b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 팟은 라이프사이클을 갖는다. 워커 노드가 죽으면 해당 노드 내에서 동작하는 모든 팟들도 종료된다. 레플리카셋(replica set)은 지정한 팟 개수만큼 팟이 항상 실행되게 가용성을 보장한다. 또 한, 노드 장애 등의 이유로 파드를 사용할 수 없다면 다른 노드에 대한 파드를 다시 생성한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 쿠버네티스에서 서비스는 하나의 논리적 팟 셋과 팟들에 접근할 수 있는 정책을 정의하는 추상적인 개념이다. 서비스는 YAML(추천)이나 JSON을 이용해 정의된다. 각 팟들이 가진 고유의 IP는 서비스의 도움 없이 외부로 노출될 수 없다. 따라서 서비스를 사용해야 애플리케이션에 트래픽을 줄 수 있다. 서비스는 다음과 같은 타입을 갖고 각 타입마다 용도가 다르다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>- ClusterIP(기본값): 클러스터 내에서 내부 IP에 대해 서비스를 노출한다. 오직 클러스터 내에서만 서비스가 접근될 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - NodePort: 외부에서 노드 IP의 특정 포트(&lt;NodeIP&gt;:&lt;NodePort&gt;)로 들어오는 요청을 감지해 해당 포트와 연결된 팟으로 트래픽을 전달한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp;- LoadBalancer: 기존 클라우드에서 외부 로드밸런서를 생성하고 서비스에 고정 IP를 할당해 준다. NodePort의 상위 집합이다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp;- ExternalName: CNAME 레코드와 값을 반환해 서비스를 externalName 필드 내용에 매핑한다. 즉, 해당 타입을 이용해 생성한 서비스가 외부 도메인을 가리키게 한다. 프락시 설정을 하지 않으며 kube-dns v1.7 이상 또는 CoreDNS 0.08 이상을 사용해야 한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>Service와 Label</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 특정 파드를 생성하는 레플리카 셋을 YAML 파일로 만들고 구동하면 해당 파드들이 레플리카셋 정의에 명시한 만큼 생성된다. 또 한, 레플리카셋을 삭제하면 파드도 삭제된다. 이 때문에 레플리카셋과 파드가 연결되 있다고 오해하기 쉽다. 이 둘은 사실 느슨한 결합(loosely coupled)을 유지하고 있으며, 이를 위해 label selector를 이용하고 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; label selector를 이해하기 위해 레플리카셋을 이용해 Nginx 파드를 생성하는 YAML 예시를 보자.</span></p>
<div class="colorscripter-code" style="color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position: relative !important; overflow: auto;">
<table class="colorscripter-code-table" style="margin: 0; padding: 0; border: none; background-color: #fafafa; border-radius: 4px;" cellspacing="0" cellpadding="0" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="padding: 6px; border-right: 2px solid #e5e5e5;">
<div style="margin: 0; padding: 0; word-break: normal; text-align: right; color: #666; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">1</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">2</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">3</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">4</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">5</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">6</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">7</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">8</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">9</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">10</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">11</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">12</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">13</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">14</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">15</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">16</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">17</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">18</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">19</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">20</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">21</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">22</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">23</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #066de2;">apiVersion:</span>&nbsp;apps<span style="color: #0086b3;"></span><span style="color: #a71d5d;">/</span>v1&nbsp;#&nbsp;YAML&nbsp;파일에서&nbsp;정의한&nbsp;오브젝트의&nbsp;API&nbsp;버전</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #066de2;">kind:</span>&nbsp;ReplicaSet&nbsp;#&nbsp;리스&nbsp;종류</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #066de2;">metadata:</span>&nbsp;#&nbsp;라벨,&nbsp;주석,&nbsp;이름&nbsp;등&nbsp;리소스의&nbsp;부가&nbsp;정보</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;<span style="color: #066de2;">name:</span>&nbsp;replicaset<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>nginx</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #066de2;">spec:</span>&nbsp;#&nbsp;리소스&nbsp;생성을&nbsp;위한&nbsp;자세한&nbsp;정보</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;replicas:&nbsp;<span style="color: #0099cc;">3</span>&nbsp;#&nbsp;유지할&nbsp;동일&nbsp;파드&nbsp;갯수</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;<b>selector:</b></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">matchLabels:</span></b></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">app:</span>&nbsp;my<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>nginx<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>pods<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>label</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">#&nbsp;윗&nbsp;부분이&nbsp;레플리카셋&nbsp;정의</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">#&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">#&nbsp;아래&nbsp;부분이&nbsp;파드&nbsp;정의</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;<span style="color: #066de2;">template:</span>&nbsp;#&nbsp;파드를&nbsp;생성할&nbsp;때&nbsp;사용할&nbsp;템플릿&nbsp;정의.&nbsp;일반적으로&nbsp;파드&nbsp;생성&nbsp;방식과&nbsp;같은&nbsp;내용을&nbsp;포함한다.&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">metadata:</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">name:</span>&nbsp;my<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>nginx<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>pod</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b><span style="color: #066de2;">labels:</span></b></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">app:</span>&nbsp;my<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>nginx<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>pods<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>label</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">spec:</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">containers:</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>&nbsp;<span style="color: #066de2;">name:</span>&nbsp;nginx</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">image:</span>&nbsp;nginx:latest</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">ports:</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>&nbsp;<span style="color: #066de2;">containerPort:</span>&nbsp;<span style="color: #0099cc;">80</span></span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 라벨은 파드 등 쿠버네티스 리소스를 분류할 때 사용할 수 있는 메타데이터다. 또 한, 서로 다른 오브젝트가 서로를 찾을 때도 사용한다. 레플리카셋은 spec.selector.matchLabels에 정의된 라벨을 통해 생성하야 하는 파드를 찾는다. spec.selector.matchLabels에 정의 된 라벨이 붙은 파드 갯수가 spec.replicas에 정의 된 개수보다 부족하다면 파드 템플릿의 내용을 이용해 파드를 생성한다.</span></p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">참고자료</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://www.samsungsds.com/kr/insights/220222_kubernetes1.html" target="_blank" rel="noopener">쿠버네티스 알아보기 1편: 쿠버네티스와 컨테이너, 도커에 대한 기본 개념</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://www.redhat.com/ko/topics/virtualization/what-is-a-hypervisor" target="_blank" rel="noopener">하이퍼바이저란?</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://www.44bits.io/ko/keyword/linux-container" target="_blank" rel="noopener">컨테이너란? 리눅스의 프로세스 격리 기능</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://www.redhat.com/ko/topics/automation/what-is-orchestration" target="_blank" rel="noopener">오케스트레이션이란?</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://kubernetes.io/ko/docs/concepts/overview/" target="_blank" rel="noopener">쿠버네티스란 무엇인가?</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="http://www.yes24.com/Product/Goods/84927385" target="_blank" rel="noopener">시작하세요! 도커/쿠버네티스</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/" target="_blank" rel="noopener">쿠버네티스 기초 학습</a></span></p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p></div>