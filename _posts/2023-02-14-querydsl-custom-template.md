---
layout:       post
title:        "Querydsl 구조와 mysql 무작위 데이터 조회하기"
author:       "yunki kim"
header-style: text
catalog:      true
tags:
- 프로젝트
- kilomter
- MySQL
- JPA
- Querydsl
---

<div class="tt_article_useless_p_margin contents_style"><p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 프로젝트에서 홈 모듈 도메인을 맡아서 기능을 구현하던 도중 다음과 같이 native query를 사용한 코드를 발견했습니다.</span></p>
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
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">@Query(value&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #63a35c;">"select&nbsp;i.id,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.listImageUrl,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.title,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.exhibitionType,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.feeType,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.pickCount,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.startDate,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.endDate,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;p.isHearted,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;count(a.id)&nbsp;as&nbsp;archiveCount,"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;AVG(a.starRating)&nbsp;as&nbsp;grade&nbsp;"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"FROM&nbsp;item&nbsp;as&nbsp;i"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;LEFT&nbsp;JOIN&nbsp;pick&nbsp;p&nbsp;on&nbsp;i.id&nbsp;=&nbsp;p.pickedItem"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;LEFT&nbsp;JOIN&nbsp;archive&nbsp;a&nbsp;on&nbsp;i.id&nbsp;=&nbsp;a.item&nbsp;and&nbsp;a.isVisibleAtItem=true&nbsp;"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"WHERE"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;i.feeType&nbsp;=&nbsp;'FREE'"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;AND&nbsp;i.exposureType&nbsp;=&nbsp;'ON'"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"&nbsp;AND&nbsp;DATE_FORMAT(i.startDate,&nbsp;'%Y-%m')&nbsp;=&nbsp;DATE_FORMAT(:requestTime,&nbsp;'%Y-%m')&nbsp;"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"GROUP&nbsp;BY&nbsp;i.id&nbsp;"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"ORDER&nbsp;BY&nbsp;RAND()&nbsp;"</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">+</span>&nbsp;<span style="color: #63a35c;">"LIMIT&nbsp;5"</span>,&nbsp;nativeQuery&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;<span style="color: #0099cc;">true</span>)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">List<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>MonthlyFreeTicket<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;findRand5ByUserIdAndRequestTime(@Param(<span style="color: #63a35c;">"pickedUser"</span>)Long&nbsp;userId,&nbsp;@Param(<span style="color: #63a35c;">"requestTime"</span>)LocalDateTime&nbsp;requestTime);</span></div>
</div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; Native query를 사용한 코드도 동작에는 문제가 없지만, 가독성이 떨어지고 컴파일 단계에서 문법적 오류를 걸러내지 못한다는 문제를 가지고 있습니다. 따라서 이 문제들을 개선하기 위해 QueryDSL로 native query를 대체하기로 했습니다. QueryDSL에 대한 간단한 소개와 이점은 다음과 같습니다.</span></p>
<h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>Querydsl과 Querydsl의 이점</b></span></h2>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; Querydsl은 정적 타입을 이용해 쿼리를 생성할 수 있게 해주는 프레임워크입니다. Querydsl이 제공하는 Fluent API를 이용해 쿼리를 작성할 수 있으며 JPA, JDBC 등을 지원합니다. QueryDSL을 사용하면 다음과 같은 이점을 누릴 수 있습니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - IDE의 코드 자동 완성 기능을 사용할 수 있다..</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 문법적으로 잘못된 쿼리를 허용하지 않는다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 도메인 타입과 프로퍼티를 안전하게 참조할 수 있다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; - 도메인 타입의 리팩터링에 더 유연하게 대처한다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; Querydsl이 위와 같은 이점을 가지는 이유는 String을 사용하지 않고 Q클래스를 사용해 쿼리가 type-safe 하기 때문입니다.. String을 사용한다면 오타 등의 문제로 잘못된 참조를 하게 됩니다. 또 한, 도메인 변경이 직접적으로 쿼리에 반영되지 않고, 자동완성도 되지 않습니다.</span></p>
<h2 data-ke-size="size26"><span style="font-family: 'Noto Serif KR';"><b>QueryDSL로 native query 대체하기</b></span></h2>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 다시 본론으로 돌아가서, 위에서 설명했던 native query를 querydsl로 바꾸었습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">35</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">36</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">37</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">38</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">39</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">40</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">41</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">42</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">43</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">44</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">45</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">46</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">47</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;ItemRepositoryCustomImpl&nbsp;<span style="color: #a71d5d;">implements</span>&nbsp;ItemRepositoryCustom&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">private</span>&nbsp;<span style="color: #a71d5d;">final</span>&nbsp;JPAQueryFactory&nbsp;queryFactory;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;ItemRepositoryCustomImpl(EntityManager&nbsp;entityManager)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">this</span>.queryFactory&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;<span style="color: #a71d5d;">new</span>&nbsp;JPAQueryFactory(entityManager);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;List<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>MonthlyFreeTicketDto<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;findTopRandomFiveMonthlyFreeTicket(LocalDate&nbsp;now,&nbsp;Long&nbsp;userId)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">return</span>&nbsp;queryFactory.select(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Projections.fields(MonthlyFreeTicketDto.<span style="color: #a71d5d;">class</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.id.as(<span style="color: #63a35c;">"itemId"</span>),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.thumbnailImageUrl,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.title,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.exhibitionType,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.feeType,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.pickCount,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.exposureType,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pick.isHearted,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archive.id.count().as(<span style="color: #63a35c;">"archiveCount"</span>),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archive.starRating.avg().as(<span style="color: #63a35c;">"grade"</span>)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.from(itemEntity)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.leftJoin(pick)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.on(pick.pickedItem.eq(itemEntity),&nbsp;pick.pickedUser.id.eq(userId))</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.leftJoin(archive)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.on(archive.item.eq(itemEntity))</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.where(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.feeType.eq(FeeType.FREE),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.exposureType.eq(ExposureType.ON),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.startDate.loe(now).and(itemEntity.endDate.gt(now)),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archive.isVisibleAtItem.isTrue()</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.groupBy(itemEntity.id)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.orderBy(NumberExpression.random().asc())</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.limit(MAX_MONTLY_FREE_TICKETS)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.fetch();</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 그리고 테스트를 돌려보니 다음과 같은 쿼리가 나갔습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">35</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">36</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">37</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">38</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #ff3399; font-family: 'Noto Serif KR';">select</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.id&nbsp;as&nbsp;col_0_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.thumbnailImageUrl&nbsp;as&nbsp;col_1_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.title&nbsp;as&nbsp;col_2_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.exhibitionType&nbsp;as&nbsp;col_3_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.feeType&nbsp;as&nbsp;col_4_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.pickCount&nbsp;as&nbsp;col_5_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.exposureType&nbsp;as&nbsp;col_6_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;pick1_.isHearted&nbsp;as&nbsp;col_7_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;count(archiveent2_.id)&nbsp;as&nbsp;col_8_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;avg(cast(archiveent2_.starRating&nbsp;as&nbsp;double))&nbsp;as&nbsp;col_9_0_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #ff3399; font-family: 'Noto Serif KR';">from</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;item&nbsp;itementity0_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">left&nbsp;outer&nbsp;join</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;pick&nbsp;pick1_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">on</span>&nbsp;(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pick1_.pickedItem<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>itementity0_.id&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;pick1_.pickedUser<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">left&nbsp;outer&nbsp;join</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;archive&nbsp;archiveent2_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">on</span>&nbsp;(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archiveent2_.item<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>itementity0_.id</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #ff3399; font-family: 'Noto Serif KR';">where</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.isDeleted<span style="color: #010101;"></span><span style="color: #0099cc;">=</span><span style="color: #004fc8;">0</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;)&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.feeType<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.exposureType<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.startDate<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span><span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.endDate<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;archiveent2_.isVisibleAtItem<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">group&nbsp;<span style="color: #ff3399;">by</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.id&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #ff3399;">order</span>&nbsp;<span style="color: #ff3399;">by</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<b>random()&nbsp;asc</b>&nbsp;<span style="color: #ff3399;">limit</span>&nbsp;?</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위 쿼리에는 아무런 문제가 없어 보입니다. 하지만 현재 MySQL을 사용하고 있다는 것을 생각해 본다면 "order by random()"에 문제가 있을 것을 알 수 있습니다. MySQL에서 랜덤 한 레코드를 액세스 하기 위해선 random()이 아닌 rand()가 사용됩니다. 따라서 위 쿼리는 이동한 동작을 하지 않습니다.</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>Querydsl 동작 과정</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위와 같은 문제가 발생한 이유를 파악하기 위해선 QueryDSL의 구조를 알아야 합니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 우선 JPAQueryFactory의 내부를 보면 update(), insert(), query() 메서드를 사용하는 것을 알 수 있습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">35</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">36</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">37</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">38</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">39</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">40</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">41</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">42</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">43</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">44</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">45</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">46</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">47</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">48</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">49</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">50</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">51</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">52</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">53</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">54</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">55</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">56</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">57</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">58</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">59</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">60</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">61</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">62</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">/**</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;Factory&nbsp;class&nbsp;for&nbsp;query&nbsp;and&nbsp;DML&nbsp;clause&nbsp;creation</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@author&nbsp;tiwe</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">public&nbsp;class&nbsp;JPAQueryFactory&nbsp;implements&nbsp;JPQLQueryFactory&nbsp;&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;@Nullable</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;private&nbsp;final&nbsp;JPQLTemplates&nbsp;templates;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;private&nbsp;final&nbsp;Provider<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>EntityManager<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>&nbsp;entityManager;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;JPAQueryFactory(final&nbsp;EntityManager&nbsp;entityManager)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.entityManager&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;new&nbsp;Provider<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>EntityManager<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>()&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;EntityManager&nbsp;get()&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;entityManager;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.templates&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;<span style="color: #ff3399;">null</span>;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;JPAQueryFactory(JPQLTemplates&nbsp;templates,&nbsp;final&nbsp;EntityManager&nbsp;entityManager)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.entityManager&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;new&nbsp;Provider<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>EntityManager<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>()&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;EntityManager&nbsp;get()&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;entityManager;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this.templates&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;templates;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;JPAUpdateClause&nbsp;<span style="color: #ff3399;">update</span>(EntityPath<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>?<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>&nbsp;path)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">if</span>&nbsp;(templates&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">!</span><span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;<span style="color: #ff3399;">null</span>)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;new&nbsp;JPAUpdateClause(entityManager.get(),&nbsp;path,&nbsp;templates);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;else&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;new&nbsp;JPAUpdateClause(entityManager.get(),&nbsp;path);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;JPAInsertClause&nbsp;<span style="color: #ff3399;">insert</span>(EntityPath<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>?<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>&nbsp;path)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">if</span>&nbsp;(templates&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">!</span><span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;<span style="color: #ff3399;">null</span>)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;new&nbsp;JPAInsertClause(entityManager.get(),&nbsp;path,&nbsp;templates);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;else&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;new&nbsp;JPAInsertClause(entityManager.get(),&nbsp;path);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;JPAQuery<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>?<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>&nbsp;query()&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">if</span>&nbsp;(templates&nbsp;<span style="color: #010101;"></span><span style="color: #0099cc;">!</span><span style="color: #010101;"></span><span style="color: #0099cc;">=</span>&nbsp;<span style="color: #ff3399;">null</span>)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;new&nbsp;JPAQuery<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>Void<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>(entityManager.get(),&nbsp;templates);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;else&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;new&nbsp;JPAQuery<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span>Void<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>(entityManager.get());</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 여기서 update()는 수정, insert()는 삽입, query()는 조회에 사용되는 객체를 생성합니다. 또 한, 이 세 메서드는 공통적으로 templates가 null인지를 체크하고, null이 아니라면 지정된 template을 인자로 넘기고, template이 없다면 넘기지 않는 것을 알 수 있습니다. 지정한 template이 없다면 내부에서 default로 지정된 template을 사용합니다. 여기서 말하는 template은 서로 다른 RDBMS의 SQL 직렬화를 지원하기 위한 템플릿입니다.</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위 코드에서 60번째 줄에서 사용한 JpaQuery의 생성자 구현을 보면 다음과 같이 JPAProvider에서 기본으로 지정된 템플릿을 가져오는 것을 알 수 있습니다. 이는 insert(), update()도 마찬가지입니다.</span></p>
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
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;JPAQuery(EntityManager&nbsp;em)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">super</span>(em,&nbsp;JPAProvider.getTemplates(em),&nbsp;<span style="color: #a71d5d;">new</span>&nbsp;DefaultQueryMetadata());</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">따라서 Querydsl을 사용하기 위해 JPAQueryFactory를 사용한다면, 다음과 같은 호출 구조를 갖게 됩니다.</span></p>
<p></p><figure class="imageblock alignCenter" data-ke-mobilestyle="widthOrigin" data-origin-width="897" data-origin-height="251"><span data-url="https://blog.kakaocdn.net/dn/cdEr55/btrZhSXvtUj/OwjtsvXVQDF2zUSOt10410/img.png" data-lightbox="lightbox"><img src="/img/2023-02-14-querydsl-custom-template" srcset="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&amp;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcdEr55%2FbtrZhSXvtUj%2FOwjtsvXVQDF2zUSOt10410%2Fimg.png" onerror="this.onerror=null; this.src='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png'; this.srcset='//t1.daumcdn.net/tistory_admin/static/images/no-image-v1.png';" data-origin-width="897" data-origin-height="251"></span></figure>
<p></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 이제 JPAProvider 내부를 보면 다음과 같이 하이버네이트용 default template이 존재하는 것을 볼 수 있습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">35</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">36</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">37</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">38</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">39</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">40</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">41</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">42</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">43</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">44</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">45</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">/**</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;{@code&nbsp;JPAProvider}&nbsp;provides&nbsp;detection&nbsp;of&nbsp;the&nbsp;JPA&nbsp;provider&nbsp;based&nbsp;on&nbsp;the&nbsp;EntityManager&nbsp;instance</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@author&nbsp;tiwe</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">final</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;JPAProvider&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">private</span>&nbsp;<span style="color: #a71d5d;">static</span>&nbsp;<span style="color: #a71d5d;">final</span>&nbsp;Map<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>Class<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>?<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>,&nbsp;JPQLTemplates<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;mappings&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;Maps.newHashMap();</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">private</span>&nbsp;<span style="color: #a71d5d;">static</span>&nbsp;<span style="color: #a71d5d;">final</span>&nbsp;Map<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span><span style="color: #066de2;">String</span>,&nbsp;JPQLTemplates<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;templatesByName&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;Maps.newHashMap();</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">static</span>&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">boolean</span>&nbsp;hibernate5;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">try</span>&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">String</span>&nbsp;version&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;Class.forName(<span style="color: #63a35c;">"org.hibernate.Session"</span>).getPackage().getImplementationVersion();</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">String</span>[]&nbsp;versionParts&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;version.<span style="color: #066de2;">split</span>(<span style="color: #63a35c;">"\\."</span>);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">int</span>&nbsp;major&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;Integer.<span style="color: #066de2;">parseInt</span>(versionParts[<span style="color: #0099cc;">0</span>]);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hibernate5&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;major&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;<span style="color: #0099cc;">5</span>;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;<span style="color: #a71d5d;">catch</span>&nbsp;(ClassNotFoundException&nbsp;e)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hibernate5&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;<span style="color: #0099cc;">false</span>;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JPQLTemplates&nbsp;hibernateTemplates&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;hibernate5&nbsp;?&nbsp;Hibernate5Templates.DEFAULT&nbsp;:&nbsp;HQLTemplates.DEFAULT;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addMapping(<span style="color: #63a35c;">"org.hibernate.Session"</span>,&nbsp;hibernateTemplates);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addMapping(<span style="color: #63a35c;">"org.hibernate.ejb.HibernateEntityManager"</span>,&nbsp;hibernateTemplates);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;addMapping(<span style="color: #63a35c;">"org.hibernate.jpa.HibernateEntityManager"</span>,&nbsp;hibernateTemplates);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;templatesByName.put(<span style="color: #63a35c;">"hibernate"</span>,&nbsp;HQLTemplates.DEFAULT);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;templatesByName.put(<span style="color: #63a35c;">"hibernate5"</span>,&nbsp;Hibernate5Templates.DEFAULT);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">static</span>&nbsp;JPQLTemplates&nbsp;getTemplates(EntityManager&nbsp;em)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 여기서 하이버네이트용 템플릿인 HQLTemplates(Hibernate5Templates는 HQLTemplates를 extends 한다.)를 보면 JPQLTemplates를 extends 하는 것을 알 수 있습니다. 그리고 JPQLTemplates가 extends 하고 있는 Template이라는 클래스에 Random이 지정된 있는 것을 볼 수 있습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">/**</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;HQLTemplates&nbsp;extends&nbsp;{@link&nbsp;JPQLTemplates}&nbsp;with&nbsp;Hibernate&nbsp;specific&nbsp;extensions</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@author&nbsp;tiwe</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;HQLTemplates&nbsp;<span style="color: #a71d5d;">extends</span>&nbsp;JPQLTemplates&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">/**</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;{@code&nbsp;JPQLTemplates}&nbsp;extends&nbsp;{@link&nbsp;Templates}&nbsp;to&nbsp;provide&nbsp;operator&nbsp;patterns&nbsp;for&nbsp;JPQL</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;serialization</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@author&nbsp;tiwe</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@see&nbsp;HQLTemplates</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@see&nbsp;EclipseLinkTemplates</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;JPQLTemplates&nbsp;<span style="color: #a71d5d;">extends</span>&nbsp;Templates&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">/**</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;{@code&nbsp;Templates}&nbsp;provides&nbsp;operator&nbsp;patterns&nbsp;for&nbsp;query&nbsp;expression&nbsp;serialization</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@author&nbsp;tiwe</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;Templates&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">add</span>(Ops.MathOps.RANDOM,&nbsp;<span style="color: #63a35c;">"random()"</span>);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">add</span>(Ops.MathOps.RANDOM2,&nbsp;<span style="color: #63a35c;">"random({0})"</span>);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 따라서 MySQL에서 무작위 데이터를 조회하기 위해선 별도의 템플릿을 지정해야 합니다.</span></p>
<h3 data-ke-size="size23"><span style="font-family: 'Noto Serif KR';"><b>커스텀 템플릿 정의하기</b></span></h3>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 위 구조 설명에서 언급했듯 하이버네이트를 이용하기 위해선 HQLTemplates를 사용해야 합니다. 따라서 HQLTemplates를 extends하는 커스텀 템플릿을 다음과 같이 만들면 됩니다.</span></p>
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
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;MysqlTemplates&nbsp;<span style="color: #a71d5d;">extends</span>&nbsp;HQLTemplates&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;MysqlTemplates()&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">super</span>();</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">add</span>(MathOps.RANDOM,&nbsp;<span style="color: #63a35c;">"rand()"</span>);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">add</span>(MathOps.RANDOM2,&nbsp;<span style="color: #63a35c;">"rand({0})"</span>);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 이제 <span style="background-color: #fafafa; color: #010101;">ItemRepositoryCustomImpl 가 해당 템플릿을 사용하게 지정하고 테스트를 돌리면 됩니다.</span></span></p>
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
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;ItemRepositoryCustomImpl&nbsp;<span style="color: #a71d5d;">implements</span>&nbsp;ItemRepositoryCustom&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">private</span>&nbsp;<span style="color: #a71d5d;">final</span>&nbsp;JPAQueryFactory&nbsp;queryFactory;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;ItemRepositoryCustomImpl(EntityManager&nbsp;entityManager)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">this</span>.queryFactory&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">=</span>&nbsp;<span style="color: #a71d5d;">new</span>&nbsp;JPAQueryFactory(<span style="color: #a71d5d;">new</span>&nbsp;MysqlTemplates(),&nbsp;entityManager);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;@Override</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;List<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>MonthlyFreeTicketDto<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;findTopRandomFiveMonthlyFreeTicket(LocalDate&nbsp;now,&nbsp;Long&nbsp;userId)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 그러면 다음과 같이 올마른 쿼리가 나가는 것을 확인할 수 있습니다</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">35</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">36</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">37</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">38</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #ff3399; font-family: 'Noto Serif KR';">select</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.id&nbsp;as&nbsp;col_0_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.thumbnailImageUrl&nbsp;as&nbsp;col_1_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.title&nbsp;as&nbsp;col_2_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.exhibitionType&nbsp;as&nbsp;col_3_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.feeType&nbsp;as&nbsp;col_4_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.pickCount&nbsp;as&nbsp;col_5_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.exposureType&nbsp;as&nbsp;col_6_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;pick1_.isHearted&nbsp;as&nbsp;col_7_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;count(archiveent2_.id)&nbsp;as&nbsp;col_8_0_,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;avg(cast(archiveent2_.starRating&nbsp;as&nbsp;double))&nbsp;as&nbsp;col_9_0_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #ff3399; font-family: 'Noto Serif KR';">from</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;item&nbsp;itementity0_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">left&nbsp;outer&nbsp;join</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;pick&nbsp;pick1_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">on</span>&nbsp;(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pick1_.pickedItem<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>itementity0_.id&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;pick1_.pickedUser<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">left&nbsp;outer&nbsp;join</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;archive&nbsp;archiveent2_&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #ff3399;">on</span>&nbsp;(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archiveent2_.item<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>itementity0_.id</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #ff3399; font-family: 'Noto Serif KR';">where</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.isDeleted<span style="color: #010101;"></span><span style="color: #0099cc;">=</span><span style="color: #004fc8;">0</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;)&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.feeType<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.exposureType<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.startDate<span style="color: #010101;"></span><span style="color: #0099cc;">&lt;</span><span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;itementity0_.endDate<span style="color: #010101;"></span><span style="color: #0099cc;">&gt;</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;and&nbsp;archiveent2_.isVisibleAtItem<span style="color: #010101;"></span><span style="color: #0099cc;">=</span>?&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">group&nbsp;<span style="color: #ff3399;">by</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;itementity0_.id&nbsp;</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #ff3399;">order</span>&nbsp;<span style="color: #ff3399;">by</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<b>rand()&nbsp;asc</b>&nbsp;<span style="color: #ff3399;">limit</span>&nbsp;?</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; 만약 별도로 클래스를 정의하는 것이 귀찮다면, 또는 당장은 한 곳에서만 커스텀한 내용이 필요하다면, 다음과 같이 Expressions를 사용하는 방법도 있습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">javaqueryFactory.select(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;Projections.fields(MonthlyFreeTicketDto.<span style="color: #a71d5d;">class</span>,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.id.as(<span style="color: #63a35c;">"itemId"</span>),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.thumbnailImageUrl,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.title,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.exhibitionType,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.feeType,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.pickCount,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.exposureType,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pick.isHearted,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archive.id.count().as(<span style="color: #63a35c;">"archiveCount"</span>),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archive.starRating.avg().as(<span style="color: #63a35c;">"grade"</span>)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.from(itemEntity)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.leftJoin(pick)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.on(pick.pickedItem.eq(itemEntity),&nbsp;pick.pickedUser.id.eq(userId))</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.leftJoin(archive)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.on(archive.item.eq(itemEntity))</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.where(</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.feeType.eq(FeeType.FREE),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.exposureType.eq(ExposureType.ON),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemEntity.startDate.loe(now).and(itemEntity.endDate.gt(now)),</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;archive.isVisibleAtItem.isTrue()</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.groupBy(itemEntity.id)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.orderBy(<b>Expressions.numberTemplate(Long.<span style="color: #a71d5d;">class</span>,&nbsp;<span style="color: #63a35c;">"function('rand')"</span>).asc()</b>)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.limit(MAX_MONTLY_FREE_TICKETS)</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;.fetch();</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">&nbsp; Expressions를 사용하면 Querydsl에 구현돼&nbsp;있는 로직을 통해 템플릿을 생성할 수 있습니다. Expressions.numberTemplate()의 경우 다음과 같이 템플릿을 생성하고 있습니다.</span></p>
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
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">24</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">25</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">26</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">27</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">28</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">29</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">30</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">31</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">32</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">33</span></div>
<div style="line-height: 130%;"><span style="font-family: 'Noto Serif KR';">34</span></div>
</div>
</td>
<td style="padding: 6px 0; text-align: left;">
<div style="margin: 0; padding: 0; color: #010101; font-family: Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; line-height: 130%;">
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">/**</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;Factory&nbsp;class&nbsp;for&nbsp;{@link&nbsp;Expression}&nbsp;creation.</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*&nbsp;@author&nbsp;tiwe</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';"><span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">final</span>&nbsp;<span style="color: #a71d5d;">class</span>&nbsp;Expressions&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #999999;">/**</span></span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*&nbsp;Create&nbsp;a&nbsp;new&nbsp;Template&nbsp;expression</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*&nbsp;@param&nbsp;cl&nbsp;type&nbsp;of&nbsp;expression</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*&nbsp;@param&nbsp;template&nbsp;template</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*&nbsp;@param&nbsp;args&nbsp;template&nbsp;parameters</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*&nbsp;@return&nbsp;template&nbsp;expression</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="color: #999999; font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*/</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">public</span>&nbsp;<span style="color: #a71d5d;">static</span>&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>T&nbsp;<span style="color: #a71d5d;">extends</span>&nbsp;Number&nbsp;<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&amp;</span>&nbsp;Comparable<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>?<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span><span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;NumberTemplate<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>T<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;numberTemplate(Class<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&lt;</span>?&nbsp;<span style="color: #a71d5d;">extends</span>&nbsp;T<span style="color: #0086b3;"></span><span style="color: #a71d5d;">&gt;</span>&nbsp;cl,</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #066de2;">String</span>&nbsp;template,&nbsp;Object...&nbsp;args)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">return</span>&nbsp;numberTemplate(cl,&nbsp;createTemplate(template),&nbsp;ImmutableList.copyOf(args));</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">private</span>&nbsp;<span style="color: #a71d5d;">static</span>&nbsp;Template&nbsp;createTemplate(<span style="color: #066de2;">String</span>&nbsp;template)&nbsp;{</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color: #a71d5d;">return</span>&nbsp;TemplateFactory.DEFAULT.create(template);</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">&nbsp;&nbsp;&nbsp;&nbsp;...</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;"><span style="font-family: 'Noto Serif KR';">}</span></div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
<div style="padding: 0 6px; white-space: pre; line-height: 130%;">&nbsp;</div>
</div>
<div style="text-align: right; margin-top: -13px; margin-right: 5px; font-size: 9px; font-style: italic;"><span style="font-family: 'Noto Serif KR';"><a style="color: #e5e5e5text-decoration:none;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener">Colored by Color Scripter</a></span></div>
</td>
<td style="vertical-align: bottom; padding: 0 2px 4px 0;"><span style="font-family: 'Noto Serif KR';"><a style="text-decoration: none; color: white;" href="http://colorscripter.com/info#e" target="_blank" rel="noopener"><span style="font-size: 9px; word-break: normal; background-color: #e5e5e5; color: white; border-radius: 10px; padding: 1px;">cs</span></a></span></td>
</tr>
</tbody>
</table>
</div>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">참고 자료</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- <a href="http://querydsl.com/static/querydsl/latest/reference/html/index.html" target="_blank" rel="noopener">Querydsl Reference Guide</a></span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- <a href="https://tecoble.techcourse.co.kr/post/2021-08-08-basic-querydsl/" target="_blank" rel="noopener">Spring Boot에 QueryDSL을 사용해보자</a>&nbsp;</span></p>
<p data-ke-size="size16"><span style="font-family: 'Noto Serif KR';">- <a href="http://www.yes24.com/Product/Goods/19040233" target="_blank" rel="noopener">자바 ORM 표준 JPA 프로그래밍</a></span></p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p></div>