> 이 문서는 OASIS의 [SAML V2.0 Technical Overview](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html)를 DeepL을 사용하여 번역하고 설명과 요약을 추가한 문서입니다.

# Table of Contents

1. [Introduction](#1-introduction)
2. [Overview](#2-overview)
   1. [Drivers of SAML Adoption](#21-drivers-of-saml-adoption)
   2. [Documenation Roadmap](#22-documentation-roadmap)
3. [High-Levl SAML Use Cases](#3-high-level-saml-use-cases)
   1. [SAML Participants](#31-saml-participants)
   2. [Web Single Sign-On Use Cases](#32-web-single-sign-on-use-case)
   3. [Identity Federation Use Cases](#33-identity-federation-use-cases)
4. SAML Architecture
   1. Basic Concepts
   2. Advanced Concepts
      1. Subject Confirmation
   3. SAML Components
   4. SAML XML Constructs and Examples
      1. Relationship of SAML Components
      2. Assertion, Subject, and Statement Structure
      3. Attribute Statement Structure
      4. Message Structure and the SOAP Binding
   5. Privacy in SAML
   6. Security in SAML
5. Major Profiles and Federation Use Cases
   1. Web Browser SSO Profile
      1. Introduction
      2. SP-Initiated SSO: Redirect/POST Bindings
      3. SP-Initiated SSO: POST/Artifact Bindings
      4. IdP-Initiated SSO: POST Binding
   2. ECP Profile
      1. Introduction
      2. ECP Profile Using PAOS Binding
   3. Single Logout Profile
      1. Introduction
      2. SP-Initiated Single Logout with Multiple SPs
   4. Establishing and Managing Federated Identities
      1. Introduction
      2. Federation Using Out-of-Band Account Linking
      3. Federation Using Persistent Pseudonym Identifiers
      4. Fedrattion Using Transient Pseudonym Identifiers
      5. Federation Termination
   5. Use of Attributes
6. Extending and Profiling SAML for Use in Other Frameworks
   1. Web Services Security (WS-Security)
   2. eXtensible Access Control Markup Language (XACML)

# 1. Introduction

SAML(Security Assertion Markup Language) 표준은 온라인 비즈니스 파트너 간의 보안 정보 교환을 위한 프레임워크를 정의합니다. 이 표준은 표준 단체인 OASIS(the Organization for the Advancement of Structured Information Standards)의 보안 서비스 기술 위원회(SSTC, Security Services Technical Committee)에서 개발했습니다. 이 문서는 SAML V2.0에 대한 기술적 설명을 제공합니다.

# 2. Overview

OASIS 보안 어설션 마크업 언어(SAML) 표준은 온라인 비즈니스 파트너 간에 보안 정보를 기술하고 교환하기 위한 XML 기반 프레임워크를 정의합니다. 이 보안 정보는 보안 도메인 경계를 넘나들며 작동하는 애플리케이션이 신뢰할 수 있는 이식 가능한 SAML 어설션의 형태로 표현됩니다. OASIS SAML 표준은 이러한 SAML 어설션을 요청, 생성, 통신 및 사용하기 위한 정확한 구문과 규칙을 정의합니다.

OASIS 보안 서비스 기술 위원회(SSTC)는 SAML 표준을 개발 및 유지 관리합니다. SSTC는 이 기술 개요를 작성하여 SAML이 다루는 비즈니스 사용 사례, SAML 배포를 구성하는 높은 수준의 기술 구성 요소, 일반적인 사용 사례에 대한 메시지 교환 세부 정보, 추가 정보를 얻을 수 있는 곳을 설명함으로써 SAML에 대해 더 자세히 알고자 하는 사람들을 돕고자 합니다.

## 2.1. Drivers of SAML Adoption

<details>
   <summary>요약</summary>
   <div markdown="1">
      <H2>SAML이 필요한 이유<H2/><p/>
      <H3>SSO</H3><p/>
         쿠키에 의존하는 SSO 솔루션들은 다른 도메인에서는 사용이 불가능함(브라우저 쿠키는 DNS 도메인 간에 전송되지 않기 때문). 이 문제를 해결하기 위해서 독점적인 방법들을 사용하여 멀티도메인 SSO(MDSSO)을 지원함. 그러나 비즈니스 파트너들의 환경이 각각 다르기 때문에 독점 프로토콜을 사용하는 것은 비현실적임. SAML은 공급업체에 독립적인 표준 문법과 프로토콜을 제공하여 이 문제를 해결함.
      <p/><H3>연합 ID</H3><p/>
         온라인 서비스들이 협업하여 서비스를 제공하려고 하는 경우에, 각 서비스 제공자는 서로의 사용자들에 대해서 공통으로 알고 있어야 함. SAML은 여러 서비스들이 공통의 사용자 ID(연합 ID)를 사용하도록 해서 ID 관리 비용을 줄이는 데 도움을 줄 수 있음.
      <p/><H3>다른 서비스/기관 표준에도 사용 가능</H3><p/>
         SAML은 보안 어설션 형식을 "네이티브" SAML 기반 프로토콜 컨텍스트 외부에서 사용할 수 있도록 허용함. 예를 들어 SOAP 메시지 교환시 보안을 위해 사용하는 <a href="https://www.ibm.com/docs/ko/integration-bus/10.0?topic=configuration-ws-security">WS-Security</a> 보안 토큰에 SAML 어설션을 사용하여 다른 토큰 방식보다 풍부한 속성들을 전달할 수 있음
   </div>
</details>

보안 정보 교환에 SAML이 필요한 이유는 무엇인가요? SAML 표준을 채택하게 된 배경에는 다음과 같은 몇 가지 요인이 있습니다.

- **Single Sign-On**
  - 지난 몇 년 동안 웹 기반 SSO를 지원한다고 주장하며 다양한 제품이 판매되었습니다. 이러한 제품은 일반적으로 브라우저 쿠키에 의존하여 사용자 인증 상태 정보를 유지하므로 웹 사용자가 시스템에 액세스할 때마다 재인증이 필요하지 않습니다. 그러나 브라우저 쿠키는 DNS 도메인 간에 전송되지 않기 때문에 한 도메인의 쿠키에 있는 인증 상태 정보를 다른 도메인에서 사용할 수 없습니다. 따라서 이러한 제품들은 일반적으로 도메인 간에 인증 상태 정보를 전달하기 위해 독점적인 메커니즘을 사용하여 멀티도메인 SSO(MDSSO)를 지원했습니다. 단일 공급업체의 제품을 단일 기업 내에서 사용할 수 있는 경우도 있지만, 비즈니스 파트너는 일반적으로 이질적인 환경을 가지고 있어 독점 프로토콜을 사용하는 것이 MDSSO에 비현실적입니다. SAML은 서버 DNS 도메인과 무관하게 한 웹 서버에서 다른 웹 서버로 사용자에 대한 정보를 전송하기 위한 공급업체에 독립적인 표준 문법과 프로토콜을 제공함으로써 MDSSO 문제를 해결합니다.
- **Federation Identity**
  - 온라인 서비스가 상호 사용자를 위한 협업 애플리케이션 환경을 구축하려면 시스템이 정보 교환과 관련된 프로토콜 구문과 의미를 이해할 수 있어야 할 뿐만 아니라 교환에서 참조되는 사용자가 누구인지에 대한 공통의 이해가 있어야 합니다. 사용자는 상호 작용하는 각 파트너의 보안 도메인 내에 개별 로컬 사용자 ID를 가지고 있는 경우가 많습니다. ID 페더레이션(ID 연합)은 이러한 파트너 서비스가 조직 경계를 넘어 사용자에 대한 정보를 공유하기 위해 사용자를 지칭하는 공통의 공유 이름 식별자에 동의하고 설정할 수 있는 수단을 제공합니다. 파트너가 사용자를 지칭하는 방법에 대해 이러한 합의를 설정한 경우 해당 사용자는 연합된 ID(***federated identity***)를 가지고 있다고 합니다. 관리 관점에서 이러한 유형의 공유는 여러 서비스가 ID 관련 데이터(예: 비밀번호, ID 특성)를 독립적으로 수집하고 유지할 필요가 없으므로 ID 관리 비용을 줄이는 데 도움이 될 수 있습니다. 또한 이러한 서비스의 관리자는 일반적으로 공유 식별자를 수동으로 설정하고 유지 관리할 필요가 없으며, 이에 대한 제어 권한이 사용자에게 있을 수 있습니다.
- **Web services and other industry standards**
  - SAML은 보안 어설션 형식을 "네이티브" SAML 기반 프로토콜 컨텍스트 외부에서 사용할 수 있도록 허용합니다. 이러한 모듈성은 인증 서비스(IETF, OASIS), ID 프레임워크, 웹 서비스(OASIS, 자유 연합) 등을 다루는 다른 업계의 노력에 유용하다는 것이 입증되었습니다. OASIS WS-Security 기술 위원회는 예를 들어 웹 서비스 SOAP 메시지 교환을 보호하는 데 사용할 수 있는 WS-Security 보안 토큰 내에서 SAML의 풍부한 어설션 구성을 사용하는 방법에 대한 프로필을 정의했습니다. 특히, SAML 어설션을 사용하면 다른 WS-Security 토큰 형식으로는 쉽게 전달할 수 없는 속성을 포함한 정보 교환에 표준 기반 접근 방식을 제공한다는 이점이 있습니다.

## 2.2. Documentation Roadmap

OASIS SSTC는 SAML V2.0과 관련된 수많은 문서를 작성했습니다. 여기에는 공식 OASIS 표준 자체를 구성하는 문서, 대중이 SAML V2.0을 더 잘 이해할 수 있도록 돕기 위한 홍보 자료, 특정 환경에서의 사용을 용이하게 하거나 다른 기술과 통합하기 위한 SAML의 여러 확장 문서가 포함됩니다.

SAML V2.0 OASIS 표준을 정의하고 지원하는 문서는 Figure 1에 나와 있습니다. 밝은 색 상자는 비규범적 정보를 나타냅니다.


![Figure 1: SAML V2.0 Document Set](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02_html_7077e985.gif)

*Figure 1: SAML V2.0 Document Set*


- **Conformance Requirements**는 제품 간 호환성의 한 척도이기 때문에 소프트웨어 공급업체가 일반적으로 중요하게 생각하는 상태인 SAML 적합성에 대한 기술 요구사항을 문서화합니다. 다른 문서에서 SAML V2.0을 공식적으로 참조해야 하는 경우 이 문서를 가리키기만 하면 됩니다.

- **Assertions and Protocol**은 인증, 속성 및 권한 부여 정보를 설명하기 위해 XML로 인코딩된 어설션을 만들고 시스템 간에 이 정보를 전달하는 프로토콜 메시지에 대한 구문 및 의미를 정의합니다. 여기에는 어설션과 프로토콜에 대한 스키마가 연관되어 있습니다.

- **Bindings**는 공통 기본 통신 프로토콜 및 프레임워크를 사용하여 시스템 간에 SAML 어설션 및 요청-응답 프로토콜 메시지를 교환할 수 있는 방법을 정의합니다.

- **Profiles**는 특정 비즈니스 문제를 해결하기 위해(예: 웹 SSO 교환 수행) 보안 정보를 전달하기 위한 SAML의 풍부하고 유연한 구문을 사용하고 제한하기 위한 특정 규칙 집합을 정의합니다. 여기에는 속성 프로필의 구문 측면을 다루는 몇 가지 관련 작은 스키마가 있습니다.

- **Metadata**는 SAML 엔터티가 파트너 엔터티가 사용할 수 있도록 표준 방식으로 구성 데이터(예: 서비스 엔드포인트 URL, 서명 확인을 위한 주요 자료)를 설명하는 방법을 정의합니다. 여기에는 연관된 스키마가 있습니다.

- **Authentication Context**는 다양한 인증 메커니즘을 설명하는 인증 컨텍스트 선언을 설명하기 위한 구문을 정의합니다. 연관된 스키마 집합이 있습니다.

- **Executive Overview**는 SAML에 대한 간략한 경영진 수준의 개요와 주요 이점을 제공합니다. 이것은 비규범 문서입니다.

- **Technical Overview**는 현재 읽고 있는 문서입니다.

- **Gloassary**는 SAML 사양 전체에서 사용되는 용어를 규범적으로 정의합니다. 가능한 경우, 용어는 다른 보안 용어집에 정의된 용어와 일치합니다.

- **Errata**는 최종 게시된 버전의 정보가 상충되거나 불분명한 경우 SAML V2.0 표준에 대한 해석을 명확히 합니다. 이 문서에서 제공하는 조언은 규범적이지 않지만, SAML 준수 소프트웨어 구현자가 사용할 수 있는 해석에 대한 지침으로 유용하며 향후 표준 개정에 포함될 가능성이 높습니다. 이 문서는 지속적으로 업데이트됩니다.

- **Security and Privacy Considerations**에서는 SAML의 보안 및 개인정보 보호 속성에 대해 설명 및 분석합니다.

SAML V2.0 OASIS 표준의 출시 이후, OASIS SSTC는 몇 가지 개선 작업을 계속해 왔습니다. 이 글을 쓰는 현재 다음 개선 사항에 대한 문서가 OASIS 위원회 초안 사양으로 승인되었으며 OASIS SSTC 웹 사이트에서 확인할 수 있습니다.

- **SAML Metadata Extension for Query Requesters**. 미리 정의된 세 가지 쿼리 유형 각각에 대해 독립 실행형 SAML V1.x 또는 V2.0 쿼리 요청자를 설명하는 역할 설명자 유형을 정의합니다.

- **SAML Attribute Sharing Profile for X.509 Authentication-Based Systems**. 특성 요청자 엔터티가 X.509 클라이언트 인증서를 사용하여 요청자 엔터티에서 인증한 사용자에 대해 SAML 특성 쿼리를 할 수 있도록 하는 SAML 프로필에 대해 설명합니다.

- **SAML V1.x Metadata**. SAML V2.0 메타데이터 구성을 사용하여 SAML V1.x OASIS 표준을 지원하는 SAML 엔터티를 설명하는 방법을 설명합니다.

- **SAML XPath Attribute Profile**. XPath URI를 속성 이름으로 사용하기 위한 SAML 속성 사용을 프로파일링합니다.

- **SAML Protocol Extension for Third-Party Requests**. 의도한 응답 수신자가 아닌 다른 엔티티의 요청을 용이하게 하기 위해 SAML 프로토콜의 확장을 정의합니다.

# 3. High-Level SAML Use Cases

SAML 표준의 세부 사항을 검토하기 전에 이 표준이 다루는 몇 가지 high-level의 사용 사례를 설명하는 것이 유용합니다. 보다 자세한 사용 사례는 이 문서의 뒷부분에서 특정 SAML 프로필과 함께 설명합니다.

## 3.1. SAML Participants

SAML 상호 작용에 관련된 참여자는 누구일까요? 최소한 SAML 교환은 ***SAML asserting party*** 와 ***SAML relying party*** 라고 하는 시스템 엔터티 간에 이루어집니다. 많은 SAML 사용 사례에서 웹 브라우저를 실행하거나 SAML 지원 애플리케이션을 실행하는 사용자도 참여자이며, 심지어 ***asserting party*** 일 수도 있습니다.

***Asserting party*** 는 SAML 어설션을 만드는 시스템 엔터티입니다. SAML authority라고도 합니다. ***Relying party*** 는 받은 어설션을 사용하는 시스템 엔터티입니다. SAML ***asserting party*** 또는 ***relying party*** 가 다른 SAML 엔터티에 직접 요청을 하는 경우, 요청을 하는 당사자를 ***SAML requester*** 라고 하고 상대방을 ***SAML responder*** 라고 합니다. ***relying party*** 가 ***asserting party*** 의 정보에 의존할 의향이 있는지는 ***asserting party*** 와의 신뢰 관계의 존재 여부에 따라 달라집니다.

SAML 시스템 엔터티는 사용할 SAML 서비스 및 프로토콜 메시지와 생성 또는 소비할 어설션 유형을 정의하는 다양한 SAML 역할로 운영할 수 있습니다. 예를 들어, 멀티도메인 Single Sign-On(MDSSO, 또는 종종 SSO라고도 함)을 지원하기 위해 SAML은 ***identity provider(IdP, ID 공급자)*** 및 ***service provider(SP, 서비스 공급자)*** 라는 역할을 정의합니다. 또 다른 예로는 SAML 엔터티가 ***attribute requester*** 역할을 하는 엔터티의 ID 속성 쿼리에 대한 응답으로 어설션을 생성하는 속성 권한(***attribute authority***) 역할이 있습니다.

대부분의 SAML 어설션의 핵심은 어설션이 이루어지는 ***subject***(특정 보안 도메인의 컨텍스트 내에서 인증될 수 있는 엔터티인 ***principal***)입니다. 주체는 사람일 수도 있지만 회사나 컴퓨터와 같은 다른 종류의 엔터티일 수도 있습니다. 이 문서에서는 ***subject*** 와 ***principal*** 이라는 용어를 같은 의미(주체)로 사용하는 경향이 있습니다.

identity provider의 일반적인 어설션은 "이 사용자는 신원 미상의 사용자이며 이메일 주소는 john.doe@example.com 이고 비밀번호 메커니즘을 사용하여 이 시스템에 인증되었습니다."와 같은 정보를 전달할 수 있습니다. 서비스 제공업체는 액세스 정책에 따라 이 정보를 사용하여 신원 미상의 사용자에게 로컬 리소스에 대한 웹 SSO 액세스 권한을 부여할 수 있습니다.

## 3.2. Web Single Sign-On Use Case

멀티도메인 웹 싱글 사인온은 SAML이 적용되는 가장 중요한 사용 사례입니다. 이 사용 사례에서 사용자는 웹 사이트(airline.example.com)에서 로그인 세션(즉, ***security context***)을 가지고 있으며 해당 사이트의 리소스에 액세스하고 있습니다. 어느 시점에서 명시적으로 또는 투명하게 파트너의 웹 사이트(cars.example.co.uk)로 이동합니다. 이 경우, 두 사이트 간의 비즈니스 계약에 따라 이전에 airline.example.com과 cars.example.co.uk 간에 사용자에 대한 연합 ID 가 설정되었다고 가정합니다. ID 공급자(IdP) 사이트(airline.example.com)는 서비스 공급자(SP) 사이트(cars.example.co.uk)에 사용자가 알려져 있고(연합 ID 로 사용자를 참조하여), 인증을 받았으며, 특정 ID 속성(예: "골드 멤버십" 보유)을 가지고 있음을 주장합니다. cars.example.co.uk는 airline.example.com을 신뢰하므로 사용자가 유효하고 제대로 인증되었음을 신뢰하여 해당 사용자에 대한 로컬 세션을 만듭니다. 이 사용 사례는 Figure 2에 표시되어 있으며, 사용자가 cars.example.co.uk 사이트로 이동할 때 다시 인증할 필요가 없다는 사실을 보여줍니다.


![Figure 2](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02_html_5f15833f.gif)

*Figure 2: General Single Sign-On Use case*


이 high-level 설명은 사용자가 SP에서 보호된 리소스에 액세스하기 전에 먼저 IdP에서 인증했음을 나타냅니다. 이 시나리오를 일반적으로 *IdP-initiated web SSO* 시나리오라고 합니다. IdP-initiated SSO는 특정 경우에 유용하지만, 더 일반적인 시나리오는 사용자가 브라우저 북마크를 통해 SP 사이트를 방문하여 특별한 인증이나 권한 부여가 필요하지 않은 리소스에 먼저 액세스하는 것으로 시작됩니다. 이후 사용자가 SP에서 보호된 리소스에 접속하려고 시도할 때 SAML이 활성화된 deployment에서는 사용자가 로그인하도록 하기 위해 SP가 인증 요청과 함께 사용자를 IdP로 보냅니다. 따라서 이 시나리오를 *SP-initiated web SSO* 라고 합니다. 로그인이 완료되면 IdP는 보호된 리소스에 대한 사용자의 액세스 권한의 유효성을 검사하는 데 사용할 수 있는 어설션을 생성할 수 있습니다. SAML V2.0은 IdP-initiated 및 SP-initiated 플로우를 모두 지원합니다.

SAML은 다양한 유형과 강도의 사용자 인증 방법, 연합 ID를 표현하기 위한 대체 형식, 프로토콜 메시지 전송을 위한 다양한 바인딩 사용, ID 특성 포함 등을 사용하기 위한 요구 사항을 다루는 이 두 가지 기본 흐름에 대한 다양한 변형을 지원합니다. 이러한 옵션 중 상당수는 이 문서의 뒷부분에서 더 자세히 다룹니다.

## 3.3. Identity Federation Use Cases

앞서 언급한 바와 같이, 사이트가 사용자를 참조할 식별자(identity) 및/또는 신원 속성(identity attributes) 집합에 대한 공급자 간의 합의가 있을 때 사용자의 신원은 일련의 공급자 간에 연합된 것으로 간주됩니다.

비즈니스 파트너가 연합 ID를 사용하여 사용자에 대한 보안 및 신원 정보를 공유하기로 결정할 때 고려해야 할 많은 질문이 있습니다. 예를 들면 다음과 같습니다.

- 사용자가 연합 ID를 통해 서로 연결해야 하는 사이트에 기존 로컬 ID를 가지고 있나요?
- 사용자에 대한 연합 ID의 설정 및 해지가 동적으로 이루어지나요, 아니면 사이트에서 미리 설정된 연합 ID를 사용하나요?
- 사용자가 연합 ID 설정에 명시적으로 동의해야 하나요?
- 사용자에 대한 신원 속성을 교환해야 하나요?
- ID 연합(identity federation)은 사용자 세션이 끝나면 파기되는 임시 식별자에 의존해야 하나요?
- 정보를 암호화해야 할 정도로 교환할 정보의 개인정보 보호에 대한 우려가 높은가요?

이전 버전의 SAML 표준은 파트너 간의 연합 ID를 나타내는 데 사용되는 식별자 유형에 대한 대역 외 합의에 의존했습니다(예: X.509 주체 이름 사용). 연합 ID의 사용을 지원했지만, SAML 메시지 교환을 사용하여 해당 ID의 식별자를 직접 설정할 수 있는 수단은 제공하지 않았습니다. SAML V2.0은 연합 ID 기능을 향상시키기 위해 두 가지 기능을 도입했습니다. 첫째, 연합된 이름 식별자(federated name identifiers)의 동적 설정 및 관리를 지원하기 위해 새로운 구조체와 메시지가 추가되었습니다. 둘째, 개인정보 보호 특성을 가진 두 가지 새로운 유형의 이름 식별자가 도입되었습니다.

경우에 따라 신원 관련 연합 정보(identity-related federation information) 교환은 SAML V2.0 메시지 교환 외부에서 이루어질 수 있습니다. 예를 들어, 공급자는 identity provider의 데이터 소스(예: 인사 데이터베이스)에 의해 구동되고 service provider에게 전파되는 일괄 또는 오프라인 'identity feeds'를 통해 등록된 사용자에 대한 정보를 공유하도록 선택할 수 있습니다. 그 후, 사용자의 연합된 ID는 SAML 어설션에 사용되어 공급업체 간에 전파되어 Single Sign-On을 구현하거나 사용자에 대한 ID 속성을 교환하기 위해 수 있습니다. 또는 identity provider가 특정 속성 이름과 값을 기반으로 사용자를 참조한다는 비즈니스 계약만으로 ID 연합을 달성할 수 있으며, 공급자 간에 사용자 정보를 유지 및 업데이트하는 데 추가적인 흐름이 필요하지 않습니다.

여기에 설명된 high-level의 ID 연합 사용 사례는 SAML이 새로운 기능을 사용하여 웹 SSO 교환 중에 사용자에 대한 연합 ID를 동적으로 설정하는 방법을 보여 줍니다. 대부분의 ID 관리 시스템은 사용자를 위해 로컬 ID를 유지합니다. 이러한 로컬 ID는 사용자의 로컬 로그인 계정 또는 기타 로컬에서 식별 가능한 사용자 프로필로 표시될 수 있습니다. 이러한 로컬 ID는 공급업체가 파트너와 상호 작용할 때 사용자를 나타내는 데 사용되는 연합 ID에 연결되어야 합니다. 연합 ID를 사용할 파트너(또는 파트너들)의 로컬 ID에 연합 ID를 연결하는 프로세스를 종종 계정 연결(*account linking*)이라고 합니다.

Figure 3에 표시된 이 사용 사례는 웹 SSO 중에 사이트가 계정 연결 프로세스에 사용되는 연합 이름 식별자를 동적으로 설정하는 방법을 보여줍니다. 이 예에서는 하나의 identity provider인 airline.example.com과 두 개의 service provider인 렌터카에 대한 cars.example.co.uk 및 호텔 예약에 대한 hotels.example.ca가 존재합니다. 이 예에서는 사용자가 세 제공업체 사이트 모두에 등록되어 있지만(즉, 기존 로컬 로그인 계정이 있는 경우) 로컬 계정은 모두 다른 계정 식별자를 가지고 있다고 가정합니다. 사용자 John은 airline.example.com에서는 **johndoe**로 등록되어 있고, cars.example.co.uk에서는 계정이 **jdoe**이며, hotels.example.ca에서는 **johnd**입니다. 이 사이트들은 사용자의 연합 이름 식별자에 대해 ***persistent*** SAML privacy-preserving pseudonyms(가명)을 사용하기로 계약을 맺었습니다. John은 이전에 이러한 사이트 간에 자신의 ID를 연합한 적이 없습니다.


![Figure 3](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02_html_m7bc7a2ed.gif)
   
*Figure 3: General Identity Federation Use Case*


처리 순서는 다음과 같습니다:

1. John은 자신의 **johndoe** 사용자 계정을 사용하여 airline.example.com에서 항공편을 예약합니다.

2. 그런 다음 John은 브라우저 북마크를 사용하거나 링크를 클릭하여 자동차를 예약하기 위해 cars.example.co.uk를 방문합니다. 이 사이트는 브라우저 사용자가 로컬로 로그인하지 않았지만 이전에 IdP 파트너 사이트인 airline.example.com을 방문한 적이 있음을 확인합니다(선택적으로 SAML V2.0의 새로운 IdP 검색 기능 사용). 따라서 cars.example.co.uk는 John에게 로컬 cars.example.co.uk ID를 airline.example.com과 연합하는 데 동의할지 묻습니다.

3. John이 연합에 동의하면 브라우저는 다시 airline.example.com으로 리디렉션되고, airline.example.com에서는 John이 cars.example.co.uk를 방문할 때 사용할 새 가명 **azqu3H7** 을 생성합니다. 이 가명은 John의 **johndoe** 계정에 연결됩니다. 두 공급업체는 후속 트랜잭션에서 John을 참조하기 위해 이 식별자를 사용하는 데 동의합니다.

4. 그런 다음 John은 연합 영구 식별자(federated persistent identifier) **azqu3H7** 로 표시되는 사용자가 IdP에 로그인했음을 나타내는 SAML 어설션과 함께 cars.example.co.uk로 다시 리디렉션됩니다. cars.example.co.uk가 이 식별자를 본 것은 이번이 처음이므로 이 식별자가 적용되는 로컬 사용자 계정을 알지 못합니다.

5. 따라서 John은 자신의 **jdoe** 계정을 사용하여 cars.example.co.uk에 로그인해야 합니다. 그런 다음 cars.example.co.uk는 나중에 IdP airline.example.com에서 사용할 수 있도록 로컬 **jdoe** 계정에 **azqu3H7** 식별자를 연결합니다. 이제 IdP와 이 SP의 사용자 계정이 연합 이름 식별자 **azqu3H7** 을 사용하여 연결됩니다.

6. 자동차를 예약한 후 John은 호텔 객실을 예약하기 위해 브라우저 북마크를 선택하거나 링크를 클릭하여 hotels.example.ca를 방문합니다.

7. IdP airline.example.com와의 연합 프로세스가 반복되어 IdP 사용자 **johndoe** 에 대한 새 가명 **f78q9C0** 이 생성되며, 이 가명은 hotels.example.ca를 방문할 때 사용됩니다.

8. John은 새 SAML 어설션을 사용하여 hotels.example.ca SP로 다시 리디렉션됩니다. SP는 John에게 로컬 **johnd** 사용자 계정에 로그인하도록 요구하고 나중에 IdP airline.example.com에서 사용할 수 있도록 가명을 페더레이션 이름 식별자로 추가합니다. 이제 IdP와 이 SP의 사용자 계정은 페더레이션 이름 식별자 **f78q9C0** 을 사용하여 연결됩니다.

앞으로 John은 항공편, 자동차 및 호텔을 예약해야 할 때마다 cars.example.co.uk 및 hotels.example.ca를 방문하기 전에 airline.example.com에 한 번만 로그인하면 됩니다. airline.example.com IdP는 John을 cars.example.co.uk에서는 **azqu3H7** 로, hotels.example.ca에서는 **f78q9C0** 으로 식별합니다. 각 SP는 연결된 영구 가명을 통해 John의 로컬 사용자 계정을 찾고 SSO 교환 후 John이 비즈니스를 수행할 수 있도록 허용합니다.
