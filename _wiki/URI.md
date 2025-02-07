---
layout  : wiki
title   : URI
summary : Uniform Resource Identifier
updated : 2019-06-15 10:09:39 +0900
tag     : http
toc     : true
public  : true
parent  : what
latex   : false
---
* TOC
{:toc}

# 개요

* URI : **U**niform **R**esource **I**dentifier
    * URL : **U**niform **R**esource **L**ocator
    * URN : **U**niform **R**esource **N**ame

# RFC 3986

* [RFC3986](https://www.ietf.org/rfc/rfc3986.txt ) -  Uniform Resource Identifier (URI): Generic Syntax
    * 1.1.3.  URI, URL, and URN

>
A URI can be further classified as a locator, a name, or both.  The
term "Uniform Resource Locator" (URL) refers to the subset of URIs
that, in addition to identifying a resource, provide a means of
locating the resource by describing its primary access mechanism
(e.g., its network "location").  The term "Uniform Resource Name"
(URN) has been used historically to refer to both URIs under the
"urn" scheme [RFC2141], which are required to remain globally unique
and persistent even when the resource ceases to exist or becomes
unavailable, and to any other URI with the properties of a name.


# URI

* URI: 통합 자원 '식별자'
* 인터넷 상의 정보 리소스를 고유하게 식별할 수 있다.
* URI는 URL과 URN이라는 두 가지 형태가 존재한다.

다음은 [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt )의 3. Syntax Components에서 발췌한 URI의 syntax example이다.

```
The following are two example URIs and their component parts:

     foo://example.com:8042/over/there?name=ferret#nose
     \_/   \______________/\_________/ \_________/ \__/
      |           |            |            |        |
   scheme     authority       path        query   fragment
      |   _____________________|__
     / \ /                        \
     urn:example:animal:ferret:nose
```

위가 URL이고, 아래가 URN이다.

## URL

* 통합 자원 '지시자'
* 누구나 흔하게 접하는 그 URL이 맞다.

URL 포맷은 크게 세 부분으로 이루어져 있다.

* scheme: 리소스에 접근할 때 사용하는 프로토콜을 명시한다. `http:`, `ftp:`, `mailto:` 등이 이에 해당한다.
* 호스트: 서버의 인터넷 주소
* 리소스

예를 들어 `http://johngrib.github.io/wiki/URI/index.html`라는 URL이 있다면 다음과 같이 파악할 수 있다.

* scheme: `http://`
* 호스트: `johngrib.github.io`
* 리소스: `/wiki/URI/index.html`

### fragment

프래그먼트는 다음 예제와 같이 URL 마지막에 `#`과 함께 붙는다.

```
http://johngrib.github.io/wiki/URI.md#fragment
```

위의 예제에서 `#fragment`는 서버에 전달되지 않는다.

프래그먼트 처리는 브라우저가 서버로부터 리소스를 다운받은 다음에 이루어진다.

## URN

* 통합 자원 '이름'
* 리소스의 위치에 영향을 받지 않는 고유한 이름이다.
* 위치 독립적이기 때문에 리소스가 다른 곳으로 옮겨져도 작동해야 한다.

# 참고

## URI 파싱 정규식

다음은 RFC 3986의 "Appendix B.  Parsing a URI Reference with a Regular Expression"에
수록된 URI 파싱 정규식이다. (숫자는 캡처 그룹 넘버.)

```regex
^(([^:/?#]+):)?(//([^/?#]*))?([^?#]*)(\?([^#]*))?(#(.*))?
 12            3  4          5       6  7        8 9
```

이 정규식을 [regexper.com](https://regexper.com/#%5E((%5B%5E%3A%2F%3F%23%5D%2B)%3A)%3F(%5C%2F%5C%2F(%5B%5E%2F%3F%23%5D*))%3F(%5B%5E%3F%23%5D*)(%5C%3F(%5B%5E%23%5D*))%3F(%23(.*))%3F )에 넣고 돌리면 다음과 같은 기차 선로 다이어그램이 나온다(`//`를 `\/\/`로 수정해야 돌아간다).

![regex](/post-img/URI/URI.regex.svg)

만약 **http://www.ics.uci.edu/pub/ietf/uri/#Related**라는 URI를 이 정규식으로 파싱한다면 다음과 같은 캡처 그룹을 확보할 수 있다.

| 그룹 | 컴포넌트  | 캡처한 내용       |
|------|-----------|-------------------|
| $1   |           | http:             |
| $2   | scheme    | http              |
| $3   |           | //www.ics.uci.edu |
| $4   | authority | www.ics.uci.edu   |
| $5   | path      | /pub/ietf/uri/    |
| $6   |           | <undefined>       |
| $7   | query     | <undefined>       |
| $8   |           | #Related          |
| $9   | fragment  | Related           |

URI의 창시자가 직접 만든 정규식이니 유용하게 사용하자.

## 상대 URL을 절대 URL로 파싱하는 알고리즘

RFC 3986의 5.2.  Relative Resolution(29~33 쪽)에 나와 있다.

# 추가

## URL 길이 제한

[RFC 7231 - 6.5.12 414 URI Too Long](https://tools.ietf.org/html/rfc7231#section-6.5.12 )

URI가 너무 길때 반환되는 414 스테이터스 코드가 RFC 7231에서 추가되었다.

다음 글이 읽어볼 만 하다.

[WWW FAQs: What is the maximum length of a URL?](https://boutell.com/newfaq/misc/urllength.html )

링크된 글을 요약하자면 다음과 같다.

* HTTP 사양에서 URL 길이에 제한은 없다.
* 그러나 각 브라우저나 웹 서버는 길이에 제한을 두기도 한다.
* 매우 긴 URL은 대체로 바람직하지 않다. 의미를 잘 담을 수 있는 압축적인 URL을 궁리하자.
* 호환성을 위해 2000자 이상은 쓰지 않는 것이 좋겠다.

| Browser           | limit | tested  | link                                                                  |
|-------------------|-------|---------|-----------------------------------------------------------------------|
| Internet Explorer | 2,083 |         | [Maximum URL length is 2,083 characters in Internet Explorer][ie2083] |
| Firefox           | ?     | 10,000  |                                                                       |
| Safari            | ?     | 80,000  |                                                                       |
| Opera             | ?     | 190,000 |                                                                       |

[ie2083]: https://support.microsoft.com/en-us/help/208427/maximum-url-length-is-2-083-characters-in-internet-explorer 


# Links

* [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt ) - Uniform Resource Identifier (URI): Generic Syntax
* [RFC 3986(rfc-editor)](https://www.rfc-editor.org/info/rfc3986 )

# 참고문헌

* HTTP 완벽 가이드
    * David Gourley, Brian Totty, Marjorie Sayer, Sailu Reddy, Anshu Aggarwal 지음 (2002, O'Reilly Media)
    * 이응준, 정상일 옮김 (2016 초판 2쇄, Republic of Korea, Insight Press 2014(인사이트))
    * [교보문고](http://kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788966261208 )
    * [알라딘](http://www.aladin.co.kr/shop/wproduct.aspx?ItemId=49731592 )

