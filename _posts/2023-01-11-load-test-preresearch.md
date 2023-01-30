---
layout:       post
title:        "성능 테스트 사전 조사"
author:       "yunki kim"
header-style: text
catalog:      true
tags:
- 성능 테스트
- 부하 테스트
---

<div class="tt_article_useless_p_margin contents_style"><h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>개요</b></span></h2>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 프로젝트 기능 구현은 완료되었으나 프로젝트 성능 최적화가 아직 이루어지지 않은 상태입니다. 따라서 전체 시스템 성능을 확인할 수 있는 성능 테스트를 진행한 뒤 개선점을 찾고 개선하고자 합니다. 성능 테스트는 여러 가지 테스트를 하위 테스트로 가지고 있으며 테스트 진행에 사용하는 도구 역시 다양합니다. 따라서 본 글은 본격적인 성능 테스트에 앞서 다양한 성능 테스트 중 목적에 맞는 테스트를 선택하고 합당한 툴 선택을 목적으로 합니다.&nbsp;또 한, 테스트는 AWS 환경에서 이루어 지므로 AWS 환경에서 고려해야 할 사항 또한 조사하고자 합니다.</span></p>
<h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>성능 테스트(Performance Test)</b></span></h2>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>성능 테스트는 사용자, 네트워크 등 변동되는 조건 속에서 서비스가 얼마나 안정적인지를 평가하는 테스트입니다. 성능 테스트를 진행하면서 다음과 같은 몇 가지 지표를 모니터링하고 개선해 성능을 개선합니다. 이를 통해 예상치 못한 느려짐 등 서비스 이용에 방해되는 점들을 제거해 사용자 만족도를 높일 수 있습니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 디스크 시간 - 요청을 작성하거나 읽기를 실행하는 데 소요된 시간</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 대역폭 사용량 - 네트워크 인터페이스에서 사용하는 초당 비트 수</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 처리량 - 초당 수신된 요청 비율</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 스레드 수 - 활성 스레드 수</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 메모리 사용 - 가상 메모리 사용 패턴</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>부하 테스트 VS 스트레스 테스트</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 시스템 성능을 측정하는 테스트를 거론할 때 부하 테스트와 스트레스 테스트가 주로 언급됍니다. 이 둘의 차이를 간단히 짚고 넘어가겠습니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 부하 테스트: 시스템 성능 개선을 목적으로 실행. 애플리케이션의 성능이 악화되기 시작하는 임계치를 찾아내기 위한 전략.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 스트레스 테스트: 시스템이 부하 임계값이 이상의 부하를 줘서 동작 확인을 위한 테스트. 시스템 복구 절차, 보안 허점 감지를 위해 사용.</span></p>
<table style="border-collapse: collapse; width: 100%;" border="1" data-ke-align="alignLeft">
<tbody>
<tr>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">구분</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">부하 테스트</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">스트레스 테스트</span></td>
</tr>
<tr>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">테스트 목적</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">전체 시스템 성능 확인</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">중단점에서의 동작, 복구 가능성</span></td>
</tr>
<tr>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">주요 성능 지표</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">동시 사용자 수, 초당 트랜잭션</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">동시 사용자수, 메모리, CPU, 네트워크 등</span></td>
</tr>
<tr>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">테스트 대상</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">전체 시스템</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">식별된 트랝개션에서만 집중</span></td>
</tr>
<tr>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">테스트 완료 시기</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">예상 부하가 모두 적용되었을 경우</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">시스템 동작이 중단되었을 경우</span></td>
</tr>
<tr>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">보안성 확인</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">불가능</span></td>
<td style="width: 33.3333%; text-align: center;"><span style="font-family: 'Noto Serif KR';">가능</span></td>
</tr>
</tbody>
</table>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 본 성능 테스트는 하드웨어적 사양을 최대한 높이지 않는 선에서 전체 시스템의 각 성능을 확인해 TPS를 높이는 것을 목적으로 하므로 부하 테스트를 진행하는 것이 적절합니다.</span></p>
<h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>부하 테스트</b></span></h2>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 부하 테스트는 요구되는 부하를 서비스가 수용할 수 있는지를 확인하기 위한 작업입니다. 유저 시나리오를 작성하고 이를 시뮬레이션해서 인프라 및 서버 동작을 모니터링해 병목 현상을 제거합니다. 부하 테스트를 통해 다음과 같은 목적을 달성할 수 있습니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 현재 서비스 구성의 제한을 찾는다</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 원하는 부하를 수용할 수 있게 구성되었는지 확인한다</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 병목 지점을 찾고 병목 현상을 제거한다</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>부하 테스트의 단계</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 빠른 병목 현상 발견과 수정을 위해, 애플리케이션 로직이 적용되기 전 순수 인프라 수준에서 시작해 다음과 같이 작은 컴포넌트들, 솔루션, 애플리케이션 순으로 부하 테스트를 확대할 수 있다.&nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 비결합(Loosely Coupled)된 개별 컴포넌트에 대한 부하 테스트: 각 컴포넌트 별 병목 현상을 보다 빠르게 발견해 수정할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 내부 서비스에 대한 부하 테스트: 로그 기록 서비스와 같이 높은 처리량이 요구되는 서비스, 전체 서비스 품질에 있어 중요한 내부 서비스를 대상으로 테스트 진행</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 외부 서비스에 대한 부하 테스트: 플랫폼에 대한 서비스 등 외부 서비스를 대상으로 테스트 진행</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 전체 스택에 대해 부하 테스트: 개별 컴포넌트들에 대한 테스트 완료 후, 컴포넌트 간의 상호작용을 알기 위해 처음부터 끝까지 전체 스택에 대해 테스트 진행.</span></p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>단계 별 세부 수행법</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>전형적인 3-tier 웹 서비스에서 다음과 같이 테스트를 진행할 수 있다.</span></p>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="716" data-origin-height="110"><span data-url="https://blog.kakaocdn.net/dn/v53uN/btrVAaUZg4H/lGlzAAQs48XNxCfWBCAIPK/img.png" data-lightbox="lightbox"><img src="/img/2023-01-11-load-test-prereserach/img.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fv53uN%2FbtrVAaUZg4H%2FlGlzAAQs48XNxCfWBCAIPK%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" data-origin-width="716" data-origin-height="110"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 1. 최초 'WEB'을 출력하는 웹 페이지를 대상으로 동시 연결성에 대한 테스트 수행 -&gt; 결과 평가 -&gt; 최적화 진행(Client -&gt; WS)</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 2. WS를 통해 WAS에서 넘겨받은 API를 대상으로 동시 연결성에 대한 테스트 수행 -&gt; 결과 평가 -&gt; 최적화 진행(Client -&gt; WS -&gt; WAS - I/O logic)</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 3. DB에서 최소한의 쿼리 결과를 전달받아 출력하는 로직을 대상으로 동시 연결성에 대한 테스트 수행 -&gt; 결과 평가 -&gt; 최적화 진행(Client -&gt; WS -&gt; WAS - I/O logic -&gt; DB)</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 4. 3-tier stack 전체를 대상으로 애플리케이션 로직이 적용된 API에 동시 연결성에 대한 테스트 수행 -&gt; 결과 평가 -&gt; 최적화 진행(Client -&gt; WS -&gt; WAS - I/O logic -&gt; DB)</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 5. 4번을 기반으로 다양한 시나리오를 지정해 테스트 수행 -&gt; 결과 평가 -&gt; 최적화 진행</span></p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>부하 테스트 시, 각 레이어 별 고려 사항</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>1. 네트워크 용량 확인</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; EC2는 인스턴스마다 타입 서로 다른 네트워킹 성능을 제공하기 이를 인지하고, 인스턴스가 수용 가능한 성능 내에서 테스트가 이루어져야 합니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; EC2 인스턴스의 네트워크 밴드폭은 vCPU 개수와 관련 있습니다. 예를 들어 32개의 vCPU를 가지고 있는 m5.8 xlarge 인스턴스의 경우 10 Gbps의 밴드 폭을, 64개의 vCPU를 가지고 있는 m5.16 xlarge 인스턴스의 경우 20 Gbps의 밴드 폭을 가지고 있습니다. 또 한, 이 밴드 폭은 같은 리전에 존재하는 인스턴스끼리 통신할 때만 100% 사용 가능하며 다른 리전의 인스턴스와 통신한다면 50%에 해당하는 밴드폭만 사용 가능합니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 만일 인스턴스의 vCPU 개수가 16개 이하라면 'up to X Gbps'와 같은 식으로 밴드폭을 명시합니다. 그 이유는 이러한 인스턴스들은 각자의 기준 밴드 폭을 가지고 크레딧 사용에 따라 최대 X Gbps까지 밴드폭을 넓힐 수 있기 때문입니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 각 인스턴스 타입의 네트워크 밴드 폭은 다음과 같은 AWS CLI 명령어를 통해 확인할 수 있습니다. 아래 명령어는 t2타입의 모든 인스턴스 타입의 네트워크 대역폭을 조회합니다.</span></p>
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
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">aws&nbsp;ec2&nbsp;describe<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>instance<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>types&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>filters&nbsp;<span style="color: #63a35c;">"Name=instance-type,Values=t2.*"</span>&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>query&nbsp;<span style="color: #63a35c;">"InstanceTypes[].[InstanceType,&nbsp;NetworkInfo.NetworkPerformance]"</span>&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">-</span>output&nbsp;table</span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="263" data-origin-height="188"><span data-url="https://blog.kakaocdn.net/dn/FXPAb/btrVIvx5Bhg/AdurVkOV5lvjObRnmLl2Q1/img.png" data-lightbox="lightbox"><img src="/img/2023-01-11-load-test-prereserach/img_1.png" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFXPAb%2FbtrVIvx5Bhg%2FAdurVkOV5lvjObRnmLl2Q1%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" data-origin-width="263" data-origin-height="188"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><i>&nbsp; </i></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><i>&nbsp; 위 명령어를 사용하기 위해선 AWS CLI에 등록한 IAM 계정에 다음과 같은 DescribeInstances권한이 설정돼 있어야 합니다.</i>&nbsp;&nbsp;</span></p>
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
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"Version"</span>:&nbsp;<span style="color: #63a35c;">"2012-10-17"</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"Statement"</span>:&nbsp;[</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"Effect"</span>:&nbsp;<span style="color: #63a35c;">"Allow"</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"Action"</span>:&nbsp;[</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"ec2:DescribeInstances"</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"ec2:DescribeImages"</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"ec2:DescribeTags"</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"ec2:DescribeSnapshots"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;],</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"Resource"</span>:&nbsp;<span style="color: #63a35c;">"*"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;]</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>2. 부하 생성 클라이언트</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>필요한 만큼의 부하를 충분히 생성할 수 있는 클라이언트가 존재해야 한다. 인스턴스를 클라이언트로 사용하는 경우, 만약 클라이언트가 처리할 수 있는 동시성에 제한이 있다면 이는 인스턴스 비용 증가로 이어진다. 이 때는 Thread 기반 툴 보다 높은 동시성을 제공하는 Async<span style="letter-spacing: 0px;">IO 기반 툴을 사용하는 것이 좋다.</span></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b><span style="letter-spacing: 0px;">3. 로드벨런싱</span></b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><span style="letter-spacing: 0px;">&nbsp; ELB의 경우 여러 이유로 기대치 보다 낮은 결과나 500번째 에러가 발생한다. 이때 SurgeQueueLength와 SpilloverCount 지표를 주의해서 봐야 한다. SurgeQueueLength는 백엔드 인스턴스가 요청을 처리하지 못해 큐에 쌓이는 최대 크기다(기본 1024). 이 크기를 넘어가면 500번대 에러가 나면서 SpilloverCount가 기록된다.</span><span style="letter-spacing: 0px;"></span></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>4. 서버 인스턴스:</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp;&nbsp;</b>서버 인스턴스 설정 값에 따라 WAS 자원 사용 효율성이 달라진다. 예컨대 리눅스 서버의 <a href="https://linuxhint.com/check-open-files-in-linux/" target="_blank" rel="noopener">open files</a> 숫자를 올바르게 세팅해서 서버 인스턴스를 효과적으로 사용할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>5. 애플리케이션 서버</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 애플리케이션 서버마다 다르지만, Tomcat 같은 스레드 기반 애플리케이션 서버의 경우 Thread Pool이 너무 작다면 처리할 요청들을 기다리는 상태가 길어져 전체 부하 테스트의 효율을 떨어트린다. 이로 인해 최근에는 이벤트 기반 비동기 형태의 애플리케이션 서버가 자주 사용된다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>6. 애플리케이션</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 불필요한 코드 포함, blocking 코드 등이 성능의 원인이 된다. 적절한 Unit Test와 Lint 등을 통해 문제를 조기에 해결해야 한다. 애플리케이션 관련 문제 해결을 위해 APM(Application Perfromance Monitoring)을 활용해 성능을 모니터링하는 것도 하나의 방법이다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>7. 데이터베이스</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; CPU 사용률과 응답 시간 등을 확인해야 한다. 성능 향상을 위해 별도로 설정한 값이 있다면, 해당 값들을 별도로 체크해 병목 지점을 확인해야 한다.</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>부하 테스트 툴 선택</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 이제 부하 테스트에 사용할 툴을 선택해보자. 이번 테스트에서 부하 테스트 툴은 다음과 같은 조건을 만족해야 한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 1. 원하는 지표를 선택해 확인할 수 있어야 한다(GUI 선호).</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 2. 간편하게 시나리오 작성이 가능해야 한다(스크립트 선호).</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 3. 추가 요금 없이 원하는 값을 세팅할 수 있어야 한다(오픈소스 선호)</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; &nbsp; 위 조건을 모두 만족하는 대표적인 툴로는 Jmeter, K6가 있다. 이제 이 툴들을 비교해 보자.</span></p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>Jmeter</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp; </b>아파치 재단에 의해 만들어진&nbsp;자바 기반 부하 테스트 툴이다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>장점</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>-&nbsp;</b>다양한 종류의 프로토콜과 <a href="https://jmeter-plugins.org/" target="_blank" rel="noopener">플러그인</a>을 지원하며 GUI를 제공한다(HTTP/2는 Jmeter 5.0부터 별도 플러그인 설치 없이 지원한다). GUI 툴을 이용하면 처음 부하 테스트를 하는 입장에서 어떤 기능을 제공하는지 쉽게 파악할 수 있기 때문에 코드 기반 부하 테스트에 비해 장벽이 낮다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- 테스트 결과는 다양한 수치를 포함한 HTML 형식 리포트로 export 할 수 있다. 리포트에 포함되는 정보를 <a href="https://jmeter.apache.org/usermanual/generating-dashboard.html" target="_blank" rel="noopener">설정할</a> 수도 있다. </span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- Groovy나 bash script 등 여러 <a href="https://www.blazemeter.com/blog/jmeter-groovy" target="_blank" rel="noopener">스크립트를 지원한다</a>.&nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- 커뮤니티가 크다는 장점도 존재한다. Jmeter는 출시된 지 1998년에 출시되었기 때문에 그동안 상당한 양의 커뮤니티를 구축해 왔다. 2023년 1월 기준 커밋 개수는 17,799개다</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>단점</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>- </b>다만, GUI 툴에 자잘한 버그가 존재한다(차트를 셀이 늘어났다 안 줄어드는 문제 등).&nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- 2 - 3 GHz CPU 기준 하나의 JMeter 클라이언트는 1000 - 2000개의 vuser를 감당할 수 있다. 따라서 경우에 따라 <a href="https://jmeter.apache.org/usermanual/jmeter_distributed_testing_step_by_step.html" target="_blank" rel="noopener">distributed load testing</a>을 진행해야 할 수도 있다. 이는 클라이언트를 띄우는데 필요한 비용과 직결된다.</span></p>
<p data-ke-size="size16">&nbsp;</p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>K6</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>&nbsp; </b>LoadImpact에 의해 개발된 부하테스트를 위한 툴이다. JS로 테스트를 작성할 수 있다. JS에 익숙한 입장에서 이 부분이 가장 맘에 든다. </span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>장점</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- Postman으로 작성한 요청을 K6 스크립트로 변환해 주는 컨버터를 지원해 준다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- 설치가 쉽다. JS를 사용하긴 하지만 노드나 다른 애플리케이션을 별도로 설치하지 않아도 된다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- 많은 기능을 제공해 준다. Jmeter가 플러그인으로 많은 기능을 제공해 주기만, 처음 사용하는 사용자 입장에선 어떤 플러그인이 필요한지 모른다. 그에 반해 k6는 디폴트로 많은 기능들을 제공해 준다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- Go를 이용해 만들어긴 툴이기에 성능상 이점이 존재한다. Jmeter 같이 1 개의 스레드를 1 개의 vuser로 사용하는 테스트 툴에서는 스레드가 응답을 기대릴 때까지 다른 일을 하지 못한다. 그에 반해 k6는 내부적으로 goroutine을 사용한다. goroutine은 유휴 상태인 스레드를 체크해서 해서 작업을 할당한다. 또 한, 메모리를 적게 사용한다. k6는 스레드 하나당 최대 100kb 메모리를 사용한다. 반면 JVM을 사용한 다른 툴들은 스레드 하나당 기본 1MB(java 14 기준) 메모리를 사용한다. 즉, k6를 이용하면 같은 환경에서 더 많은 vuser를 생성할 수 있다. 단일 k6 인스턴스로 30,000~40,000 개의 vuser를 생성할 수 있으며 이 는 곧 300,000 RPS이상의 성능을 낼 수 있다는 의미다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>단점</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>-&nbsp;</b>커뮤니티가 크지 않다. 2017년에 출시되었기 때문에 오래된 JMeter 같은 툴에 비하면 <a href="https://github.com/topics/xk6" target="_blank" rel="noopener">익스텐션</a>이 다양하지 않다. 2023년 1월 기준 커밋 개수는 4,915개다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><b>기타</b></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- 자체 K6 Cloud나 그라파나 등 여러 GUI 툴로 결과를 확인할 수 있다. 이 것이 장점이 될 수도 있지만 별도로 툴을 선택하고 연동해줘야 한다는 점에서 단점이 될 수도 있다. 반면, 원하는 툴로 원하는 지표를 볼 수 있다는 점에서는 장점이 된다.</span></p>
<h4 data-ke-size="size20"><span style="font-family: 'Noto Serif KR';"><b>결론</b></span></h4>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; k6를 선택하기로 했다. Jmeter가 GUI를 제공한다는 것이 큰 장점이라고는 하지만 테스트해 본 결과 GUI가 불친절하다고 느껴졌다. 또 한, 예산이 제한된 있는 개인 프로젝트 특성상 성능을 최대한 덜 소비하는 툴을 선택하는 것이 좋다고 생각했다. 별도의 GUI툴을 선택해 결과를 시각화해야 한다는 것이 단점 이긴 하지만, 레퍼런스에 시각화 툴 별로 세팅하는 방식이 명시돼 있어 어렵지 않게 세팅할 수 있을 거라 예상된다. 또 한, 세팅을 마치고 도커로 이미지화해서 공유하면 다른 팀원들 역시 쉽게 사용할 수 있을 거라 판단한다.</span></p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">참고 자료:</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="http://softflow.io/%EC%84%B1%EB%8A%A5%ED%85%8C%EC%8A%A4%ED%8A%B8-vs-%EB%B6%80%ED%95%98%ED%85%8C%EC%8A%A4%ED%8A%B8/">비기능 테스트: 성능 테스트 vs 스트레스 테스트</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://aws.amazon.com/ko/blogs/korea/how-to-loading-test-based-on-aws/">AWS 기반 웹 및 애플리케이선 서버 부하 테스트: A to Z</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-network-bandwidth.html">Amazon EC2 instance network bandwidth</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-ec2-console.html" target="_blank" rel="noopener">Example policites for working in the Amazon EC2 console</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://linuxhint.com/check-open-files-in-linux/" target="_blank" rel="noopener">How to check open files in Linux</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://jmeter.apache.org/usermanual/generating-dashboard.html" target="_blank" rel="noopener">Generating Report Dashboard</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://www.blazemeter.com/blog/jmeter-groovy" target="_blank" rel="noopener">How to write Grooby Jemeter Functions</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://k6.io/docs/integrations/" target="_blank" rel="noopener">K6 Integrations</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://k6.io/blog/k6-vs-jmeter/" target="_blank" rel="noopener">Comparing k6 and JMeter for load testing</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://jmeter.apache.org/usermanual/jmeter_distributed_testing_step_by_step.html" target="_blank" rel="noopener">Apache JMeter Distributed Testing step-by-step</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';"><a href="https://www.baeldung.com/jvm-configure-stack-sizes" target="_blank" rel="noopener">Configuring Stack Sizes in the JVM</a></span></p></div>