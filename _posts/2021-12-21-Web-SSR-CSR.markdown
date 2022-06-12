---
layout: post
title: "[Web] 서버 사이드 렌더링(SSR)과 클라이언트 사이드 렌더링(CSR) 개념"
tags: [Web]
categories: [Web]
banner:
  image: /assets/images/banners/ssr.png
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 3.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
---

요즘 기업이 SSR을 선택하는 이유가 있을까? 

우리가 많이 알고 있는 네이버 블로그 모바일 서비스도 2019년 5월 29일 "내 동영상 페이지"를 시작으로 Node.js 기반의 SSR 아키텍처가 네이버 모바일 블로그에 적용되었다고 합니다.

또한, 월마트도 SEO 최적화와 좋은 퍼포먼스를 위해 SSR 아키텍처를 선택했다고 합니다.

왜!? Why!?

밑의 자료를 통해 저의 궁굼증을 해결했습니다.

참고자료 :[어서 와, SSR은 처음이지? - 도입 편](https://d2.naver.com/helloworld/7804182)

<br>

## **SSR(server-side rendering)이란 무엇인가?**
{: id="a" } 

***

서버 사이드 렌더링은 서버에서 클라이언트(브라우저)에게 보여줄 페이지를 모두 구성하여 클라이언트(브라우저)에게 페이지를 보여주는 방식입니다. JSP/Servlet의 아키텍처에서 이 방식을 사용했습니다.

서버 사이드 렌더링을 사용하면 모든 데이터가 매핑된 서비스 페이지를 클라이언트(브라우저)에게 바로 보여줄 수 있습니다. 서버 사이드 렌더링은 서버에서 페이지를 구성하기 때문에 클라이언트 사이드 렌더링보다 페이지를 구성하는 속도는 늦지만 전체적으로 사용자에게 보여주는 콘텐츠 구성이 완료되는 시점은 빨라진다는 장점이 있습니다. 

<br>

## **SSR(server-side rendering)의 장점**
{: id="b" } 

***

서버 사이드 렌더링을 사용하는 목적은 크게 2가지 입니다.  

1. `SEO(search engine optimization)` 
2. `빠른 페이지 렌더링`

`SEO`, 검색 엔진 최적화는 구글이나 네이버와 같은 검색사이트에서 검색 결과가 사용자에게 최대한 많이 노출될 수 있도록 최적화 하는 기법입니다. 
서버 사이드 렌더링은 빈 HTML 페이지를 받아 브라우저에서 그리는 클라이언트 사이드 렌더링과 다르게 서버에서 모두 구성되어 브라우저로 보내주기 때문에 페이지를 그리는 시간이 빠릅니다. 

<br>

## **SSR(server-side rendering) VS CSR(client-side rendering)**
{: id="c" } 

***


![image](/assets/images/banners/s_rendering.png) 


![image](/assets/images/banners/c_rendering.png)  

출처자료: [The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)
{: style="text-align: center;"}

<br>

`서버 사이드 렌더링`은 브라우저의 요청에 대해 렌더링할 준비가 된 페이지의 HTML을 응답하는 반면, 클라이언트 사이드 렌더링은 브라우저가 모든 자바스크립트의 링크가 포함된 빈 HTML 문서를 가져옵니다.
다시 말해, 서버 사이드 렌더링은 모든 자바스크립트 파일이 다운로드 및 실행될 때까지 기다릴 필요 없이 브라우저가 서버에서 HTML 렌더링을 시작합니다. 

반면 `클라이언트 사이드 렌더링`은 서버 사이드 렌더링보다 초기 전송되는 페이지 속도는 빠르지만 서비스에서 필요한 데이터를 클라이언트(브라우저)에서 추가로 요청하여 재구성해야 하기 때문에 전체적인 페이지 완료 시점은 서버 사이드 렌더링 보다 느립니다.

<br>

## **Node.js의 기반의 SSR**
{: id="d" } 

***

기본적으로 `SPA` 기반의 어플리케이션을 개발하기 위해서는 `프론트엔드`와 `백엔드` 영역의 분리가 선행되어야 합니다.

아래 그림과 같이 기존의 `JSP/Servlert` 페이지를 `CSR`, `SSR` 영역 `API`로 분리함으로써 프론트엔드와 `백엔드` 영역에서 담당하는 페이지의 역할을 나누어야 합니다.

![image](/assets/images/banners/spa.png)  
출처자료 : [어서 와, SSR은 처음이지? - 도입 편](https://d2.naver.com/helloworld/7804182)
{: style="text-align: center;"}

<br>

저 그림만 보면 무슨 차이인지 모를 수 있습니다.

`SPA`는 Single Page Application의 약자로 하나의 `HTML` 파일을 기반으로 자바스크립트를 이용해 동적으로 화면의 컨텐츠를 바꾸는 웹 어플리케이션을 말합니다. 요즘 프론트엔드 프레임워크 중 `React`, `Vue`, `Angular`를 `SPA` 프레임워크라고도 부르는데
`CSR`를 이용한다고 생각하면 됩니다. 

<br>

### **너무나 빠른 프론트엔드의 생태계**  
2015년 ES6 표준이 개정되면서 Javascript의 많은 변화가 일어났습니다. 또한, 이전의 자바스크립트의 단점을 보완하기 위해 나온 `Jquery`가 `react`, `vue`, `angular` 같은 강력한 프레임워크가 나오자 더이상 필요가 없어지는 상황까지 오고야 말았습니다.

또한 ES6 이후로 프론트엔드 환경 최적화에 필요한 웹팩, 바벨, 린트 같은 기술들이 출시되면서 프론트엔드의 생태계가 빠르게 변화되고 있다는 걸 알 수 있습니다.

이런 상황에서, 프론트엔드 개발자들은 개발영역 확장에 대한 도전이 필요하게 되었습니다.

![image](https://user-images.githubusercontent.com/52439201/147050643-6ad3a6ec-2f6d-482c-b393-2eb4773a2773.png)  
**[jqeury 추이 그래프]**
{: style="text-align: center;"}

<br>

### **Full Stack Developer의 한계** 

2004-현재까지 `Full Stack Developer` 트렌드를 보면 지속적으로 증가하는 것을 확인할 수 있습니다. 많은 기업에서도 `Full Stack Developer`를 육성하거나 선호하고 있는데 빠르게 변화되는 프론트 엔드 생태계에서 양쪽 모두 잘하는 개발자를 만들어 내기에는 한계가 있습니다.

이에 따라, 프론트엔드 개발 영역의 확대가 커지고 있습니다. `Node.js`의 등장으로 백엔드 역할까지 모두 할 수 있으며 `SSR`, `CSR` 페이지를 프론트엔드 영역에서 모두 수행할 수 있게 되었습니다.

![image](https://user-images.githubusercontent.com/52439201/147045749-82d7c9de-ecc1-4b07-adbd-19378e9ca646.png)  
**[Full Stack Developer 트렌드]**
{: style="text-align: center;"}

<br>

### **Node.js의 기반의 SSR의 이점**

`JavaScript를 최대한 사용할 수 있습니다.`

이게 가능한 이유는 밑의 포스트를 한번 보시기 바랍니다.  

[[Node.js] Node.js - 개념 이해하기](/posts/Node.js-01/)

<br>

`프론트엔드와 백엔드 영역의 완전한 분리로 생산성 향상`

기존의 CSR 페이지는 프론트엔드에서 SSR 페이지는 백엔드에서 개발했다면, SSR 환경을 구축하면 페이지의 소유권이 온전히 프론트엔드 영역에 존재하게 되므로 페이지가 변경될 때마다 불필요한 요청/응답을 할 필요가 없습니다.

이렇게되면 백엔드 영역에서는 API 영역과 데이터 활용에 더 집중할 수 있어 서비스 품질을 높이는 데 기여할 수 있습니다.

![image](/assets/images/banners/node_ssr.png)  

<br>

`여러 가지 대안 활용`

필요에 따라서 CSR로만 구성할 수도 있고 CSR과 prependering을 함께 사용하도록 개발할 수도 있습니다.  

![image](/assets/images/banners/infographic.png)  