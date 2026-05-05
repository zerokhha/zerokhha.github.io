---
layout: post
title: "Vertica projections"
date: 2026-05-03 10:00:00 +0900
category: [kb]
subcategory: "Vertica"
---

# 프로젝션 (Projection)

---

## Slide 1 — 프로젝션 개요

> Vertica는 테이블이 아닌 **프로젝션**에 실제로 데이터 분산 및 정렬 수행하여 저장한다. (테이블은 논리적 오브젝트)

### 프로젝션 (Projection)

- **데이터가 실제로 저장되는 물리적 Object**
- 테이블의 전체 컬럼을 포함하는 1개 이상의 프로젝션 (**Super Projection**)과 일부 컬럼만을 포함하는 (**Query Specific Projection**)을 추가로 생성할 수 있다.
- SQL을 통해서는 테이블을 조회하지만 내부적으로 버티카에서는 **프로젝션에서 데이터를 조회**하는 구조이다.
- 데이터를 저장하는 Projection 구성에 따라서 쿼리 성능에 큰 영향을 주기 때문에 테이블 생성 및 프로젝션 생성 시에 **반드시 고려가 필요**하다.

<!-- diagram: Vertica 프로젝션 구조 -->
<svg width="100%" viewBox="0 0 680 260" role="img">
  <title>Vertica 프로젝션 구조</title>
  <desc>Client가 Vertica의 Table에 접근하면, 내부적으로 Projection(컬럼 A, B, C)에서 데이터가 조회되는 구조</desc>
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <!-- Client -->
  <g class="c-gray">
    <ellipse cx="340" cy="38" rx="64" ry="22" stroke-width="0.5"/>
    <text class="th" x="340" y="38" text-anchor="middle" dominant-baseline="central">Client</text>
  </g>

  <!-- Arrow: Client → Table -->
  <line x1="340" y1="61" x2="340" y2="90" class="arr" marker-end="url(#arrow)" stroke-width="1.5"/>
  <line x1="340" y1="90" x2="340" y2="61" class="arr" marker-end="url(#arrow)" stroke-width="1.5"/>

  <!-- VERTICA label -->
  <text class="ts" x="128" y="118" text-anchor="middle" style="fill:var(--color-text-secondary)">VERTICA</text>

  <!-- Table outer box -->
  <g class="c-blue">
    <rect x="200" y="96" width="280" height="148" rx="10" stroke-width="0.5"/>
    <text class="th" x="340" y="116" text-anchor="middle" dominant-baseline="central">Table</text>
  </g>

  <!-- Projection inner box -->
  <g class="c-teal">
    <rect x="220" y="128" width="240" height="104" rx="8" stroke-width="0.5"/>
    <text class="th" x="340" y="150" text-anchor="middle" dominant-baseline="central">Projection</text>
  </g>

  <!-- Column bars inside Projection -->
  <g class="c-purple">
    <rect x="246" y="166" width="44" height="52" rx="4" stroke-width="0.5"/>
    <text class="th" x="268" y="193" text-anchor="middle" dominant-baseline="central">A</text>
  </g>
  <g class="c-purple">
    <rect x="318" y="166" width="44" height="52" rx="4" stroke-width="0.5"/>
    <text class="th" x="340" y="193" text-anchor="middle" dominant-baseline="central">B</text>
  </g>
  <g class="c-purple">
    <rect x="390" y="166" width="44" height="52" rx="4" stroke-width="0.5"/>
    <text class="th" x="412" y="193" text-anchor="middle" dominant-baseline="central">C</text>
  </g>
</svg>

---

## Slide 2 — 프로젝션 생성 조건

| 경우 | 예시 |
|------|------|
| 테이블 생성 및 데이터 초기 적재 시 | `CREATE TABLE TEST(col1 int, col2 int);`<br>`INSERT INTO TEST VALUES(1,1);` |
| 테이블 생성 시 프로젝션 속성을 지정하는 경우 | `CREATE TABLE TEST ( col1 int, col2 int ) ORDER BY col1 SEGMENTED BY HASH(col1) ALL NODES;` |
| 프로젝션을 명시적으로 선언하는 경우 | `CREATE PROJECTION TEST_PJ ( col1, col2 ) AS SELECT col1, col2 FROM TEST ORDER BY col1 SEGMENTED BY HASH(col1) ALL NODES;` |

---

## Slide 3 — 프로젝션 DDL

<!-- diagram: 프로젝션 DDL 구조 -->
<svg width="100%" viewBox="0 0 680 400" role="img">
  <title>프로젝션 DDL 구조</title>
  <desc>CREATE PROJECTION DDL의 각 절(column list, base query, order by, segmented by, ksafe)과 그 목적을 설명하는 구조도</desc>
  <defs>
    <marker id="arrow2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <!-- Section 1: Column list -->
  <line x1="40" y1="20" x2="40" y2="95" stroke="var(--color-border-secondary)" stroke-width="0.5"/>
  <text class="th" x="56" y="34" dominant-baseline="central">CREATE PROJECTION</text>
  <text class="ts" x="56" y="52" dominant-baseline="central" style="fill:var(--color-text-secondary)">customer (</text>
  <text class="ts" x="72" y="68" dominant-baseline="central" style="fill:var(--color-text-secondary)">customer_no    encoding rle,</text>
  <text class="ts" x="72" y="82" dominant-baseline="central" style="fill:var(--color-text-secondary)">customer_number  encoding rle,</text>
  <text class="ts" x="72" y="96" dominant-baseline="central" style="fill:var(--color-text-secondary)">date_ordered   encoding deltaval</text>

  <!-- Label: column list -->
  <line x1="420" y1="60" x2="490" y2="60" stroke="var(--color-border-secondary)" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="498" y="52" dominant-baseline="central">Column list and encodings</text>
  <g class="c-teal">
    <rect x="498" y="64" width="140" height="22" rx="4" stroke-width="0.5"/>
    <text class="th" x="568" y="75" text-anchor="middle" dominant-baseline="central">Minimize disk space</text>
  </g>

  <!-- Divider 1 -->
  <line x1="40" y1="110" x2="640" y2="110" stroke="var(--color-border-tertiary)" stroke-width="0.5" stroke-dasharray="6 4"/>

  <!-- Section 2: Base query -->
  <text class="th" x="56" y="136" dominant-baseline="central">AS SELECT</text>
  <text class="ts" x="72" y="154" dominant-baseline="central" style="fill:var(--color-text-secondary)">customer_no, customer_number, date_ordered</text>
  <text class="ts" x="56" y="170" dominant-baseline="central" style="fill:var(--color-text-secondary)">FROM customer</text>
  <line x1="420" y1="154" x2="490" y2="154" stroke="var(--color-border-secondary)" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="498" y="154" dominant-baseline="central">Base query</text>

  <!-- Divider 2 -->
  <line x1="40" y1="192" x2="640" y2="192" stroke="var(--color-border-tertiary)" stroke-width="0.5" stroke-dasharray="6 4"/>

  <!-- Section 3: ORDER BY -->
  <text class="th" x="56" y="218" dominant-baseline="central">ORDER BY</text>
  <text class="ts" x="72" y="236" dominant-baseline="central" style="fill:var(--color-text-secondary)">customer_no, date_ordered</text>
  <line x1="420" y1="222" x2="490" y2="222" stroke="var(--color-border-secondary)" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="498" y="214" dominant-baseline="central">Sort order</text>
  <g class="c-blue">
    <rect x="498" y="226" width="130" height="22" rx="4" stroke-width="0.5"/>
    <text class="th" x="563" y="237" text-anchor="middle" dominant-baseline="central">Minimize filtering</text>
  </g>

  <!-- Divider 3 -->
  <line x1="40" y1="268" x2="640" y2="268" stroke="var(--color-border-tertiary)" stroke-width="0.5" stroke-dasharray="6 4"/>

  <!-- Section 4: SEGMENTED BY -->
  <text class="th" x="56" y="293" dominant-baseline="central">SEGMENTED BY hash (</text>
  <text class="ts" x="72" y="311" dominant-baseline="central" style="fill:var(--color-text-secondary)">customer_no</text>
  <text class="th" x="56" y="328" dominant-baseline="central">) ALL NODES</text>
  <line x1="420" y1="305" x2="490" y2="305" stroke="var(--color-border-secondary)" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="498" y="296" dominant-baseline="central">Segmentation clause</text>
  <g class="c-amber">
    <rect x="498" y="308" width="158" height="22" rx="4" stroke-width="0.5"/>
    <text class="th" x="577" y="319" text-anchor="middle" dominant-baseline="central">Minimize processing load</text>
  </g>

  <!-- Divider 4 -->
  <line x1="40" y1="352" x2="640" y2="352" stroke="var(--color-border-tertiary)" stroke-width="0.5" stroke-dasharray="6 4"/>

  <!-- Section 5: KSAFE -->
  <text class="th" x="56" y="376" dominant-baseline="central">KSAFE;</text>
  <line x1="420" y1="376" x2="490" y2="376" stroke="var(--color-border-secondary)" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="498" y="376" dominant-baseline="central">K-safety</text>
</svg>

---

## Slide 4 — 인코딩 예시

| 인코딩 방식 | 설명 |
|-------------|------|
| **BLOCK_DICT** | 낮은 cardinality 문자 필드에 적용. 데이터 정렬 불필요. 디스크 공간 적게 차지함. |
| **DELTAVAL / DELTARANGE_COMP** | 정렬된 높은 cardinality 숫자 데이터에 적용. `DELTAVAL`은 현재 값과 첫 번째 값 사이의 차이를 계산하여 인코딩. `DELTARANGE_COMP`는 각 값 간의 차이를 계산하여 인코딩. |
| **RLE (Run Length Encoding)** | 동일한 값의 정렬된 시퀀스를 발생 횟수 + 값의 단일 쌍으로 변환. Projection의 `ORDER BY` 절에 있는 낮은 cardinality 열에 가장 적합. |

---

## Slide 5 — 쿼리 성능에 영향을 주는 이유

**예시 쿼리:** `SELECT A.col1, B.col2 FROM TEST1 A INNER JOIN TEST2 B ON A.col1=B.col1;`

<!-- diagram: 조인 분산 키에 따른 성능 비교 -->
<svg width="100%" viewBox="0 0 680 340" role="img">
  <title>조인 분산 키에 따른 쿼리 성능 비교</title>
  <desc>조인 키가 동일 컬럼으로 분산된 경우(노드 간 데이터 이동 없음)와 다른 컬럼으로 분산된 경우(RESEG 발생, 성능 저하) 비교</desc>
  <defs>
    <marker id="arrow3" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <!-- === LEFT SIDE: 분산 동일 (Good) === -->
  <!-- Title -->
  <text class="ts" x="160" y="18" text-anchor="middle" style="fill:var(--color-text-secondary)">조인키로 사용된 컬럼 기준으로</text>
  <text class="ts" x="160" y="33" text-anchor="middle" style="fill:var(--color-text-secondary)">프로젝션이 동일하게 분산된 상태</text>

  <!-- TEST1 (top center) -->
  <g class="c-blue">
    <rect x="120" y="44" width="80" height="36" rx="6" stroke-width="0.5"/>
    <text class="th" x="160" y="62" text-anchor="middle" dominant-baseline="central">TEST1</text>
  </g>

  <!-- 3 nodes (bottom row) -->
  <g class="c-blue">
    <rect x="44" y="138" width="60" height="56" rx="6" stroke-width="0.5"/>
    <text class="th" x="74" y="166" text-anchor="middle" dominant-baseline="central">N1</text>
  </g>
  <g class="c-blue">
    <rect x="130" y="138" width="60" height="56" rx="6" stroke-width="0.5"/>
    <text class="th" x="160" y="166" text-anchor="middle" dominant-baseline="central">N2</text>
  </g>
  <g class="c-blue">
    <rect x="216" y="138" width="60" height="56" rx="6" stroke-width="0.5"/>
    <text class="th" x="246" y="166" text-anchor="middle" dominant-baseline="central">N3</text>
  </g>

  <!-- TEST2 (bottom center) -->
  <g class="c-blue">
    <rect x="120" y="226" width="80" height="36" rx="6" stroke-width="0.5"/>
    <text class="th" x="160" y="244" text-anchor="middle" dominant-baseline="central">TEST2</text>
  </g>

  <!-- Arrows: TEST1 → nodes → TEST2 -->
  <line x1="148" y1="80" x2="78" y2="136" class="arr" stroke="#378ADD" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="160" y1="80" x2="160" y2="136" class="arr" stroke="#378ADD" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="172" y1="80" x2="242" y2="136" class="arr" stroke="#378ADD" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="78"  y1="194" x2="148" y2="224" class="arr" stroke="#378ADD" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="160" y1="194" x2="160" y2="224" class="arr" stroke="#378ADD" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="242" y1="194" x2="172" y2="224" class="arr" stroke="#378ADD" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>

  <!-- Result badge: Good -->
  <g class="c-teal">
    <rect x="44" y="280" width="232" height="46" rx="6" stroke-width="0.5"/>
    <text class="th" x="160" y="298" text-anchor="middle" dominant-baseline="central">✓ 노드 간 데이터 이동 없음</text>
    <text class="ts" x="160" y="316" text-anchor="middle" dominant-baseline="central">쿼리 성능 극대화</text>
  </g>

  <!-- SQL below -->
  <text class="ts" x="44" y="340" style="fill:var(--color-text-secondary)">order by col1 / segmented by hash(col1)</text>

  <!-- Divider -->
  <line x1="340" y1="10" x2="340" y2="330" stroke="var(--color-border-tertiary)" stroke-width="0.5" stroke-dasharray="6 4"/>

  <!-- === RIGHT SIDE: 분산 다름 (Bad) === -->
  <text class="ts" x="510" y="18" text-anchor="middle" style="fill:var(--color-text-secondary)">조인키로 사용된 컬럼 기준으로</text>
  <text class="ts" x="510" y="33" text-anchor="middle" style="fill:var(--color-text-secondary)">프로젝션이 동일하게 분산 안된 상태</text>

  <!-- TEST1 (top) -->
  <g class="c-pink">
    <rect x="470" y="44" width="80" height="36" rx="6" stroke-width="0.5"/>
    <text class="th" x="510" y="62" text-anchor="middle" dominant-baseline="central">TEST1</text>
  </g>

  <!-- 3 nodes (mid row, pink = mismatched) -->
  <g class="c-pink">
    <rect x="388" y="120" width="56" height="50" rx="6" stroke-width="0.5"/>
    <text class="th" x="416" y="145" text-anchor="middle" dominant-baseline="central">N1</text>
  </g>
  <g class="c-pink">
    <rect x="482" y="120" width="56" height="50" rx="6" stroke-width="0.5"/>
    <text class="th" x="510" y="145" text-anchor="middle" dominant-baseline="central">N2</text>
  </g>
  <g class="c-pink">
    <rect x="576" y="120" width="56" height="50" rx="6" stroke-width="0.5"/>
    <text class="th" x="604" y="145" text-anchor="middle" dominant-baseline="central">N3</text>
  </g>

  <!-- RESEG bar -->
  <g class="c-coral">
    <rect x="388" y="196" width="244" height="26" rx="6" stroke-width="0.5"/>
    <text class="th" x="510" y="209" text-anchor="middle" dominant-baseline="central">RESEG (노드 간 데이터 재분산)</text>
  </g>

  <!-- TEST2 (bottom) -->
  <g class="c-blue">
    <rect x="470" y="238" width="80" height="36" rx="6" stroke-width="0.5"/>
    <text class="th" x="510" y="256" text-anchor="middle" dominant-baseline="central">TEST2</text>
  </g>

  <!-- Arrows: TEST1 → nodes (pink) -->
  <line x1="496" y1="80" x2="420" y2="118" stroke="#D4537E" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="510" y1="80" x2="510" y2="118" stroke="#D4537E" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>
  <line x1="524" y1="80" x2="600" y2="118" stroke="#D4537E" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>

  <!-- Arrows: nodes → RESEG -->
  <line x1="416" y1="170" x2="440" y2="194" stroke="#D4537E" stroke-width="1" stroke-dasharray="4 3" marker-end="url(#arrow3)" fill="none"/>
  <line x1="510" y1="170" x2="510" y2="194" stroke="#D4537E" stroke-width="1" stroke-dasharray="4 3" marker-end="url(#arrow3)" fill="none"/>
  <line x1="604" y1="170" x2="580" y2="194" stroke="#D4537E" stroke-width="1" stroke-dasharray="4 3" marker-end="url(#arrow3)" fill="none"/>

  <!-- Arrow: RESEG → TEST2 -->
  <line x1="510" y1="222" x2="510" y2="236" stroke="#D4537E" stroke-width="1" marker-end="url(#arrow3)" fill="none"/>

  <!-- Result badge: Bad -->
  <g class="c-coral">
    <rect x="388" y="285" width="244" height="46" rx="6" stroke-width="0.5"/>
    <text class="th" x="510" y="303" text-anchor="middle" dominant-baseline="central">✗ 노드 간 데이터 이동 발생</text>
    <text class="ts" x="510" y="321" text-anchor="middle" dominant-baseline="central">쿼리 성능 저하</text>
  </g>
</svg>

### 요약

- **조인 키가 동일 컬럼으로 분산**된 경우 → 노드 간 데이터 이동 없이 로컬에서 조인 처리 → **성능 극대화**
- **조인 키가 다른 컬럼으로 분산**된 경우 → RESEG 발생, 노드 간 데이터 재분산 필요 → **성능 저하**
