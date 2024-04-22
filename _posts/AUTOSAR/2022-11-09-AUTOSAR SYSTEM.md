---
layout: post
titile: "AUTOSAR SYSTEM"
author_profile: true
categories:
  [AUTOSAR, Classic AUTOSAR]
tags: [AUTOSAR, Classic AUTOSAR]
toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---
`{`% assign posts = site.categories.human %`}`
`{`% for post in posts %`}` `{`% include archive-single.html type=page.entries_layout %`}` `{`% endfor %`}`

# AUTOSAR SYSTEM
이 글에서는 이타스 툴 ISOLAR-A를 이용해 NvM, Fee, Fls Configuration을 하고 RTA-BSW을 통해 Configuration한 것에 대한 결과물(Code)을 생성한다. 앞의 결과물을 이용해서 AUTOSAR Spec에 나온 내용에 기반한 Memory Stack API를 작성한다. 최종적으로 이타스 툴인 ISOLAR-EVE를 이용해 작성한 API와 Application SWC의 연동이 정상적으로 작동하는지 Test하는 과정까지 소개하는 것을 목표로 한다.

## **AUTOSAR란**
최근 자동차에는 사용자의 편리와 안전을 위해 ECU와 새로운 기능들이 추가되고 있다. 이로 인해 SW 코드의 복잡성이 증가하고 재사용성이 떨어지는 문제점이 발생한다. 이를 해결하기 위해 ECU의 효율성과 SW 재사용성을 높이는 AUTOSAR 플랫폼이 등장했다.기존에 자동차 임베디드 소프트웨어는 그림 1 Yesterday에 나온 것과 같이 하드웨어 위에 소프트웨어를 올리는 구조로 둘 사이에 Dependency가 강했다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZ-2wKUlCvFQo_xc_DNbFdWHnpnZ8BBa_zuZffLNeOsnxDm54J9Z6PR1Ob7chiMjBgZXL5GE_-pEfQj-E8Yzq_i8psZXkkPKA)
따라서 개발자가 하드웨어의 세밀한 부분까지 신경 써서 프로그래밍을 해야 하고 하드웨어가 바뀌면 그 위에 있는 소프트웨어도 상당 부분 바꿔야 하는 단점이 있었다. 사람들은 Conventional에 나온 것과 같이 ASW(Application Software)와 BSW(Basic Software) 개념을 도입함으로써 소프트웨어를 개발하는데 있어 하드웨어와 독립적인 개발을 용이하게 했다. 동시에 앞서 말한 단점도 해결했다.

하지만 회사마다 ASW와 BSW에서 사용하는 모듈이나 하드웨어가 다르기 때문에 서로 호환하는데 어렵다는 단점이 있었다. 이에 각 자동차 회사의 주요기술을 모아서 표준화하자는 의견이 나왔고 그림 1 AUTOSAR에 나온 것과 같이 RTE를 도입하고 ASW와 BSW를 표준화 시킨것이 AUTOSAR다. “Software의 재사용성을 높이자”는 것이 AUTOSAR의 목표이자 가장 큰 장점이다.

AUTOSAR를 공부해야 할지를 고민하는 사람들을 위해 한 가지 덧붙이자면, AUTOSAR는 주로 독일의 자동차 OEM과 부품사들이 모여서 만든 진입장벽이 높은 플랫폼이다. AUTOSAR를 바라보는 시각은 사람마다 다르다. “AUTOSAR는 독일에서 진입장벽을 만들기 위해 독일 내 각 회사의 기술을 섞어 만든 플랫폼이므로 확산되기는 어렵다”고 주장하는 사람이 있는가하면, “AUTOSAR는 임베디드 소프트웨어에서 앞서가는 최신 기술로 소프트웨어 엔지니어라면 이러한 트렌드는 알아야 한다”라는 의견도 있다.

실제로 작년까지만 해도 독일이나 미국과 같은 경우는 AUTOSAR에 대한 도입이 활발히 일어나고 있었지만 국내에서는 그렇지 않았다. 국내에서는 AUTOSAR에 대한 개념도 모르는 자동차 관련 회사도 많았을 뿐만 아니라 AUTOSAR를 활용하는 회사도 별로 없었다. 그럼에도 불구하고 AUTOSAR를 해야 하는 이유는 AUTOSAR가 여러 자동차 회사들의 핵심기술들을 담고 있을 뿐만 아니라 임베디드 소프트웨어 분야에서 앞서가는 기술이기 때문이다. 이를 배워두면 혹여나 다른 소프트웨어 플랫폼이 확산되더라도 기본적인 뼈대는 비슷할 것이기 때문에 많은 도움이 될 것이다.

AUTOSAR의 개념만 듣고는 가슴에 와 닿지 않을 것이다. 직접 Software Modeling을 해보고 BSW에 대한 API를 짜본다면 그 때 비로소 하드웨어가 독립적이라는 것이 무엇인지 깨달을 수 있을 것이다. 본격적으로 그 과정에 대해 소개하도록 하겠다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaoFETBVCxTXY_MXy4N3DZByoNcnq4kbvtu6BaGrTH0nOxD4espqA1RcPWBQUQMo02eahJNoE14S3fahbWU2Acu4AgfbCovcw)
### **Layered Software Architecture**
AUTOSAR는 그림 2와 같이 Application Layer, RTE, Basic Software Layer로 구분된다.Application Layer는 실제 ECU를 Modeling하는 단계를 말한다. 이 단계에서는 ECU에 필요한 소프트웨어 기능이 있다면 그 기능을 Component로 만든다. Component끼리 데이터 교환이 필요하다면 각 Component에 Port를 만들고 Interface를 부여함으로써 가능하게 해준다.

RTE는 Component들의 Port를 가지고 있어 각 Application SWC 및 Application SWC와 BSW 사이의 정보 교환을 위한 연결 매개체 역할을 한다.BSW Layer는 크게 Microcontroller Abstraction Layer(MCAL), ECU Abstraction Layer, Services Layer, Complex Drivers로 나눠진다.
### **Microcontroller Abstraction Layer**
Microcontroller Abstraction Layer(MCAL)는 Basic Software의 가장 낮은 Software layer로 하드웨어에 직접적으로 접근할 수 있는 소프트웨어 Driver를 포함하고 있다. Microcontroller로부터 상위 계층을 독립적으로 만들어 주기 위해 Microcontroller를 추상화시키는 역할을 담당한다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbWj4YgXm9jAo6jbX0ctXOIqSe4Gtbo7J4wHle4KBt_xuLgwG_zroBBBFshsh0GGLrPCldEz7eQUcdq-Vqs1u-WDTWpOq0jvjw)
프로젝트는 Non-volatile Memory중에 Flash Memory를 대상으로 한 Stack API를 구현했기 때문에 BSW Module도 Flash memory에 관련된 모듈만 사용했다. 따라서 Microcontroller Abstraction Layer의 Module로는 Memory Drivers의 Flash Driver(Fls)만을 구현했다(그림 2-1).
### **ECU Abstraction Layer**
ECU Abstraction Layer는 MCAL의 driver로 접속시키는 역할을 한다. 또한 이것은 주변 장치나 device로 접근하기 위한 API를 제공한다. BSW에 속해 있는 대부분의 정형화된 인터페이스는 이 계층에 속해 있으며 Microcontroller에는 독립적이지만 ECU Hardware에는 종속적으로 구현돼야 한다.프로젝트는 ECU Abstraction Layer의 Module로는 Memory Hardware Abstraction의 Flash EEPROM Emulation(FEE)만을 구현했다(그림 2-2).
### **Services Layer**
Services Layer는 Application과 Basic Software module에 대한 서비스를 자신의 상위 계층에 제공하는 역할을 담당한다. 제공하는 서비스의 종류로는 System Service, Memory Service, Communication Service가 있다.프로젝트는 Services Layer의 Module로 Memory Services의 NVMRAM Manager(NVM)만을 구현했다(그림 2-3).
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYNvwni69ErRZFJxS-3s5YYZsJX7dPRhgtGxtioeKF2RDFLB11jvIN5U_l2du17jz08Ygob4l_nCyKqyFB0aETFdi6NndrxwA)
### **NVRAM Manager**
NvM이란 NVRAM Manager의 약자로 메모리 서비스를 관리하는 모듈을 말한다. Application level의 Software Component에서 Flash Memory로부터 데이터를 읽고 쓰고자 할 때 NvM 모듈을 거쳐서 일이 진행된다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZE6hfwag5fRzA8hhVfxQFivawf_1reo7ukXN2ia3GwpuiGqCGSm_Ihbfk5fvG3USNEClTn1eEXv0ibFXYQ9-1TxMGRY84A3w)
**그림 3**은 AUTOSAR Memory의 전체흐름도를 나타낸다. 메모리의 비휘발성 데이터를 쓰려고 할 때도 단순히 직접적으로 접근해서 사용하는 것이 아닌 여러 단계를 거쳐야만 한다. 하드웨어에는 EEPROM과 Flash Memory가 존재하는데 본 문서에서는 Flash Memory에 대해서만 다루도록 하겠다.

NvM에 대해서 다시 정리하면 AUTOSAR의 계층화된 소프트웨어 아키텍처는 Application SW와 하드웨어로 부터 분리시킴으로써 재사용과 이식을 용이하게 해준다. 이때 하드웨어에 직접적으로 접근할 수 있는 모듈을 담고 있는 것으로 하드웨어와 소프트웨어를 독립적으로 해주는 것이 MCAL이다. NvM은 MCAL의 모듈 중 Flash Memory에 직접적으로 접근할 수 있는 모듈인 Flash Driver에 접근하기 위한 상위계층인 BSW 모듈이라고 보면 된다(그림 3).
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYBHVDOSvcBJ36YPV2k3O6eo-L7CYWca28WWtDsAusRSJ1__K-UPuQczWTJsHiOLz7AJTcyX5Is_9rAy4tuLIZkfby1mnmyOA)
**그림 3-1**과 같이 NvM은 AUTOSAR Spec에 나온 Header file structure에 맞춰서 구현해야 한다. Header file structure에 나온 것 중에 ~cfg.c나 ~cfg.h와 같은 헤더파일의 경우는 ETAS 툴인 ISOLAR-A의 BCT Add-On을 이용해 Configuration을 하고 RTA-BSW Tool에 의해 자동 generate되는 파일이다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpiha267Thtc_unBtkvq5MZnQ1lvVsR6OuD7ivzpH9aikUaHVHd2n1PnmPnPdBD41LZnr7XwxbwINxRF3WHPJf6TZ8HpVPrFY57g)
NvM에 대한 Configuration은 **그림3-2**와 같이 ISOLAR-A의 BCT(ECU Conf + Code Generation) Add-On을 이용해서 한다. AUTOSAR Spec에 나온 항목에 대한 설명을 토대로 자신이 만드는 ECU의 Application SWC의 특성을 고려해 설정해주면 된다.

모든 설정을 완료하면 RTA-BSW를 이용해 Code Generate한다. 이번 프로젝트에서는 Configuration은 직접 해봤지만 RTA-BSW는 회사에서 제공할 수 없는 상황이기에 Generate해보지 못했다. 그래서 AUTOSAR 표준에 따라 작성된 ~Cfg,h와 ~Cfg,c 같은 Configuration File은 기존에 작성된 파일을 활용했다. 이외에 Header Structure에 나온 파일들은 AUTOSAR Spec을 보고 직접 작성했다. 모든 작성을 완료하면 ISOLAR-EVE에서 Header Structure의 구성에 맞게 Header File끼리 Include해줬다.

여기서 ISOLAR-EVE를 사용하는 이유는 ISOLAR-A에서 Software Component를 Modeling한 것과 NvM, Fee, Fls에 대해 Configuration한 것을 연동하고 추가적으로 Memory Stack API 코드를 작성해 Window 상에서 Compile/Debug를 함으로써 만들어진 결과물들이 제대로 작동하는지 Test하기 위해서다.Flash EEPROM Emulation Module과 Flash Driver도 위와 같은 내용으로 Configuration 하고 Header File Structure를 구성했다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYZZB1MoXF_1RpkMKHCj7NqnAPOP_PymM2UpqUcN6sqdWJ5MRcd5GFxacRzbH8uCZ-O0seCOlP8vn6B2LoBRWT0qdwcJsKf-A)
### **Memory Abstraction Interface**
Memif는 Memory Abstraction Interface의 약자로 NVRAM manager가 FEE 모듈이나 EA 모듈에 접근하는 것을 허용하는 역할을 한다. 즉 NVRAM manager와 FEE 모듈, EA 모듈 간의 인터페이스를 의미한다. FEE 모듈은 Fls memory에 접근하기 위한 모듈이고 EA 모듈은 EEPROM에 접근하기 위한 모듈이다. 따라서 Memif에서 어떤 메모리에 접근할지를 정하고 어떤 블록에 데이터를 읽고 쓸 것인지 정한다(그림 4).
### **Flash EEPROM Emulation Module**
Fee는 Flash EEPROM Emulation의 약자로 device의 구체적인 주소 지정기법을 추상화할 뿐만 아니라 상위 Layer에게 가상의 주소지정 기법을 제공한다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYShMX_k0qXwA8Wtlf7gQ5BCgDt3t2NcvtDu6YpKD_rCMrhGiYnTvB1WffUm2yok5Qd1OB5dOF6o25QmHnetQ6lmSaOYcD2puI)
쉽게 말해 Flash memory에 데이터를 읽고 쓰기 위한 memory address에 관련된 사전작업을 한다. 예를 들어 Flash memory가 64 byte라고 하면 저장공간은 32 byte씩 2개의 Blcok으로 나눠진다. 데이터를 쓸 때는 한 개의 Block에 쓰고 이 Block에 더 이상 데이터를 쓸 공간이 없으면 다른 Block에 쓰는 구조로 돼 있다. 이때 현재 Flash memory의 offset을 구해주고 Block에 데이터를 쓸 수 있는 공간이 얼마나 남았는지 확인해 주는 역할을 하는게 FEE 모듈이다.

Fee 개념뿐만 아니라 FeeConfiguration을 할 때 중요 개념인 page에 대해서 알아야 한다.나중에 설명하겠지만 Fee Configuration Setting parameter로 FeeVirtualPageSize라는 요소가 있다.이 사이즈가 의미하는 바는 데이터의 크기가 얼마이든지 상관없이 메모리를 FeeVirtualPageSize만큼 읽고 쓰겠다는 것을 의미한다. Page Size에 대한 얘기는 AUTOSAR Spec에서 언급되는 개념이니 자세한 내용은 spec(AUTOSAR_ SWS_FlashEEPROMEmulation.pdf)을 참고하기 바란다.
### **Flash Driver**
FLS는 flash driver의 약자로 flash memory에 데이터를 쓰고 읽고 지우는 서비스를 제공한다.MCU 내부에 존재하는 Internal flash memory에 접근하는 Driver는 MCAL에 존재하고 External flash memory에 접근하는 Driver는 ECU Abstraction Layer에 존재한다. 이 글에서는 Internal flash memory에 대해서만 다루기 때문에 Fls가 MCAL에 존재하는 것으로 하겠다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZI3AIdRa4IBuHMwbZguDbnh4l5biUERCuu6waE_LyhPM4xIo63XRLD6StmEIHi_CJgzLAgu7zOQ5BYn4HivX0EO2SKBwg-9Tw)
### **Fls , Fee , NvM Stack 구현**
앞선 과정을 통해 NvM Stack을 구현하는데 있어서 필요한 헤더파일은 모두 구성한 것이다.RTE가 붙은 파일은 ISOLAR-A에서 Configuration을 통해 자동 Generate된 파일들이고 ~Cfg.c와 ~Cf.g.h 파일은 앞에서 언급했듯이 ISOLAR-A의 BCT Add-On를 이용해서 Configuration한 것을 Generate한 파일들이다. 그 외에 파일들은 Fls, Fee, NvM Stack을 구현하기 위해 AUTOSAR Spec에 나온 Sequence diagram을 따라서 만든 API를 담고 있는 파일들이다(그림 6).
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaWSpKUNfNY8wzeCrJVd9r99NwsZmsSy_6X0E3utCGPNvs_-BH6uhoiwlJWuoVPcDziiisEbkwUmgXiWye4s9K6nxjjjy3ihg)
**그림 7**은 AUTOSAR Spec에 나온 Sequence Diagram을 종합해서 간단하게 만든 모식도다. 그림에 나온 것과 같이 Application Software Component에서 NvM에 대한 실행명령을 Call하면 NvRAM Manager에서 다음의 명령들을 받아들여 Queue에 저장한다. NvM에서는 하나의 명령이 실행되는 도중에는 더 이상 Dequeue를 하지 않는다. Flash Memory까지의 작업을 모두 완료하면 각각의 모듈은 Job이 완료됐다는 메시지를 상위 레벨의 모듈로 전달한다. 최종적으로 NvM의 Job이 끝나면 다른 작업을 진행할 준비가 됐다고 판단해 Queue에 저장된 다음 명령을 Dequeue해준다.

기본적으로 NvM, Fee, Fls 모듈들은 모두 Main Function을 가지고 있어 polling 방식으로 자기 상태를 체크한다. 주기적으로 자기의 상태를 계속해서 체크함으로써 명령을 실행시킬 준비가 되면 그때서야 모듈 내에 있는 API들이 동작한다.
### **ISOLAR-EVE Simulation**
앞에서 만든 것들이 제대로 동작하나 확인하기 위해 ETAS Tool ISOLAR-EVE를 사용했다. 직접 Flash Memory에 데이터를 읽고 쓰는 것을 검증하는 것은 현실적으로 제약이 있어 파일 입출력을 통해 데이터를 제대로 읽고 쓰는지 확인했다.

검증 방법으로 ISOLAR-EVE의 VRTA Monitor를 이용해 파일에 있는 값을 제대로 읽어 오는지, 입력된 값을 파일에 제대로 쓰는지, Sequence Diagram의 흐름에 따라 제대로 동작하는지 확인할 수 있다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZOjOpGmq9mfaqywU8cEYKo6zezonqTdEsfxHlpLIlRmV17DeGF7And9sUYYaAEeUhxgSBn26H0KEzmzOJ0enFgX0o3W4iuEVo)
**그림 8**을 보면 처음에 파일에는 ‘0’의 값이 저장돼 있어서 최종적으로 읽는 값은 ‘0’이다. Gyroscope Sensor에 ‘352’의 값을 입력하자 Flash Memory에 ‘352’라는 데이터 값을 입력했다. 이후에 파일에서 읽어오는 최종적인 값은 ‘352’ 임을 확인할 수 있다.

결론부터 말하자면 이번 AUTOSAR Basic Software에 대한 Modeling과 Configuration 경험을 통해 AUTOSAR 표준이 보장하는 재사용성의 의미를 이해할 수 있었다. 또한 AUTOSAR Spec에 맞춘 대부분의 개발이 BSW에 있다는 것도 느낄 수 있었다.

Application Software Level에서의 Modeling은 ECU의 기능을 구현하는 과정으로 AUTOSAR Spec의 표준을 따르는게 그다지 많지는 않다. 이때까지만 해도 “재사용성이 어떻게 이뤄진다는 것인가?” “AUTOSAR Spec에 기술한 것들이 표준화와 관련이 있는가?”라는 의문에 대한 답을 얻지 못했다. 답은 BSW에서 찾을 수 있었다. BSW의 모듈들은 하나부터 열까지 모두 AUTOSAR Spec에 기반한다. BSW 모듈의 Configuration하는 과정뿐만 아니라 모듈에 쓰이는 API, Header Structure 등 모두 AUTOSAR Spec에 나오는 내용을 바탕으로해 개발하는 과정을 통해 재사용성에 대한 의미를 재조명할 수 있다.