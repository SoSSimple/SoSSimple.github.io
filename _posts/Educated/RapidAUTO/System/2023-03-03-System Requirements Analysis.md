---
layout: post
titile: "System"
author_profile: true
categories: [Educated, RapidAUTO, System Requirements Analysis]
toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
permalink: /educated/autosar/system/:title
---

## RapidAUTO Solution 생성
**RapidAUTO Tray** > **“Manage Solution…”** > **“New Solution”** 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYl-f0iZQpCWK3KlT6MgMF59LRc-dx72PQsG22AyWoXGqCu8UMQ1DbygqDM3XyW51uAotb9e7GGcN6LPWiRw2lcQwFKlpBvTC4)

- **New Solution Name: AirbagControlUnit**
- **Solution Path: C:\0_Test\Training\AirbagControlUnit <span style="color:red">(전체 경로에 한글 또는 공백이 포함되면 안됨)</span>**
- **“Set Active Solution” 클릭**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihakj7gpA8eXz8k_MTEoO5uEx0_3u68RPuNdKlQCjgIQ21VymzDAwaWvrYBEFELW2xoe2jItVNqacvg_K4sUdsLBiHapOxvco2U)

- **Active Model : System Model**
- **Active Role : “Role Based Development” 체크 해제**
- **Rhapsody Edition (Default) : Rhapsody Architect**
<img src="https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpiha_G883V2PRahVamO9S51fnzuXL9_7PIh6ANQcuCFtIzo-lAmOtOX_ZZlZfC1zuMkd4W-g3zQld_Zx8Rs4Ehc8zo38optSlwBw"
     alt="Imgur"
     style="float: right; margin-left: 0px;" />

- **"System Model" 오픈 (SysML 기반의 시스템 사양서)**
    - RapidAUTO Tray > "Open Active Solution"
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihb9zuW6Ol8TuH_29Ru1iqt0Gt3G04zDk33z4AVsHMWAH-ngoAqAW70Aa6E69P6l_FGAoSQprjnEC7ElmH16FpPH1J9z8aR_2uA)

## System 모델 구조화
- **System Model을 작성하기 위한 System Model Structure 생성**
    - Project Root > RapidAUTO Context Menu > Create Default Structure 실행<br>
    (RapidAUTO Context Menu : 마우스 가운데 버튼으로 생성되는 Popup Menu)
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihb9Swwbc_E3IwPxE-9HIGC81x8NJZUDlSrzJ7vjxt1LhUgsi4Qa57tGadH8Rl7s0vsH5nNdHVI_5NmQEVfioL51UzfMok2VLFc)

## System Requirements Analysis 
- **MS Office 문서(Word, Excel, PowerPoint), PDF 등으로 작성된 요구사항 문서를 “솔루션폴더 > REQ” 폴더에 저장한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaQ97PfBpbbLKe80w6mSZNQCwe6fo64VmWKHfB-cF7n0g3qOu_rP0gthEz9Y4co58eLvZSZbesTRcBCDeq-j18ytkWPP9z9f7U)
- **요구사항 문서 종류**
    - SYSRS : System Requirements Specification
    - SG : Safety Goal
    - FSR : Functional Safety Requirement
    - TSR : Technical Safety Requirement
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihY1wX1Ar5CuWRKwXqDaWUjBnbcGiP_pwJskfwsIRd3_r8QzL4Pp31Nl7KMEZk-Cb_clugFogJo5gvt97Hyz9W70ILsqpxWklwE)
- **System 모델과의 양방향 추적성 설정과 요구사항 변경에 따른 Impact Analysis를 위해, 요구사항을 RapidAUTO로 임포트.**
    - RapidAUTO Tray > “Gateway” > “Set Gateway for system requirements extraction”
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYcZpT5icrDzPX6GgE517gUjgDONgOb5D4hfqZOADwA3wl-j9TyUuPcOZYVa5qyeo-OgjKt0cRhzB2_ecxkcwlLd9RgMGa7uh4)
- **양방향 추적 도구인 Gateway가 열린다.**
    - “Management View” 탭 클릭 → “SYSRS” 아이콘 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihabEuDrGfJVNS5FUxwWsNRZZaCGEki5y-3vF75pI_Xu2Qe6avHpbYWU8O0vJnEw4u1SxXUbMLny74ym5cgBywumf7-iUXxscw)
- **Gateway가 인식한 요구사항 내용을 확인한다.**
    - “Coverage Analysis View” 탭 클릭 → SYSRS 하단 요구사항 클릭 → 요구사항 내용 확인
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihakJKrznvAq6aB3vCEB_5FWVONDG1dRmZrXzEICOTvHyHUeLctxWZd4OIc4KvxGOC7_Teeuw2v36_p4c_bWko9YLGjvK_MEKw)
- **SYSRS를 추적하는 RapidAUTO의 SYSUC 모델 클릭**
    - “Management View” 탭 클릭 → “SYSUC” 아이콘 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZMHbSKpg0w33R-DXy_yxz8QI9kxabmMctyfB0UeqTFqEJi9YN9p4mkQZByWVnaR3ZImO9L6jP2cAvCzYyP-I9DEMhA78robZo)
- **요구사항 임포트 명령 실행**
    - Tools > “Add high level requirements” 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZyfLZnGPjAjsePai15xkuowgsVY0W1bhA8fmU7zm1crOtpkKxw6Qzy_xwk3oXzWiW2UnCFnhziviS_oinCpUVFIi_SwyBVkg)
- **“Add high level requirements” 대화상자에서 요구사항이 임포트 될 “SystemRequirementsModel” Package 클릭**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYTKzbVavX5aW7jSdaYBtkvorThDKoXXbwG1ScnC_NWg8Qyfnp8C5iHZO6anpMTSwAqXrtN36IMfOOot6NZ3SA3Kqn9yO68RWs)
- **SYSRS를 “SystemRequirementsModel” Package에 할당**
    - SYSRS 클릭 -> 이동 버튼 클릭 -> SYSRS 이동 확인 후 OK 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZIezOpLxLKG_M9HNeWJv4JbaaoKDh8tBv98k0V0DzulebT5Fc1r2dGAtXOxs9pyha3sPKgbKwk2hyXMPK9UuW3UOU-Z54LRGQ)
- **이동 결과를 설명하는 대화상자 확인**
    - Gateway에서 Reload 또는 Re-Analysis를 묻는 대화상자가 나오면 항상 Yes를 클릭한다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpiha348gEt5yYvZm2N6XmNy76A9GNA20GtTyBxtssdkhEMqKKYxMLmGIqkKme239hcrVTHLD4F8HBx1Bul3kPXFe1SS_JVxDShrg)
- **“SystemRequirementsModel”로 임포트 된 Requirement 확인**
    - 임포트 된 Requirement는 System 모델과의 관계(추적성)를 설정하기 위한 용도로 사용된다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpiha-OPVVOV6hzO3VIVR3Ni2yvSHIs_S9QgeJ-wdo2mHtIQPlrIFomRJGMI_mfIFe4SdhxV7nvwcP6fFex4JdMMAvzlokDT_IZLg)

## Subsystem 생성
- **“DriverAirbag” Subsystem 생성**
    - “SystemUseCaseModel” > RapidAUTO Context Menu > “Add Subsystem” 클릭 
    - Subsystem명 입력 → 생성된 Subsystem 확인
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbk5M1qiCHtCRSvNMr2BkdPrzeBGqqRg9ncQXBbRK_fiJDq-O08GYY93Q7jr-vFgFyXGijBP2HFIucAAddySjKLUZTw3Jli3A)
- **임포트 된 요구사항을 Subsystem Package로 이동**
    - “SYSRS” > Requirement, Stereotype 선택 후 이동
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbGUAa-76T8pdWaaGMRDHqJuWUjPFaJJE3VwoZEU34P7y2sV2RtEhBnMnIV89tilaadsaQko62jgQ5pptQBtyx1po1vI2vWpL4)
- **“SYSRS” Package 삭제**
    - Gateway로부터 임포트 되면서 생성된 Package는 요구사항 이동 후 삭제한다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbkfer-GkdUmaSCJx2d_t-XKOA052qo5NalMIG7U3g1SOxhZSacmhSdYYcba9EEFa5t5ZZkTApuSOMNWp8_8-REdt72qu3OvA)

## Subsystem Level Use Case Modeling
- **“DriverAirbag” Subsystem의 Use Case Diagram을 작성한다.**
    - “DriverAirbag” Subsystem 하단의 Use Case Diagram을 Open한다.
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYUSBFMGhE2nAHFFnakAC8h4MXLmuEp37HoPVB84yHuNS10Mjv8fwLRubLuP23IgqsZifZWcObaatGy0zFoCoWpYbt0cPOsxjU)
- **Subsystem의 경계를 표현한다.**
    - Drawing Toolbar에서 Boundary Box를 선택하고, 이를 Diagram에 그린다. 
    - Boundary Box의 이름은 “Subsystem_DriverAirbag”으로 입력한다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbB_oXPccpkImKQwyCtHn12qB0gp5BK4QLuirCdTfaTPBS6KbzcfW_FTAzJIDOXct6cLwtfTW0uckGPP2GywpFVzGfKNSYPLg)
- **Subsystem과 상호작용하는 외부 Entity를 그린다.  (Impact, Driver)**
    - External Actor : Subsystem과 상호작용하는 외부 Entity
    - Internal Actor : Subsystem과 상호작용하는 다른 Subsystem 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaeeNJOM7ErV-nVyoH6U5rfsxWl2PxkEkvw1rzHZ466T8JIFqF-s3H3q7bEEPEbOHagCENWQO1o8MyBHqJCa2ynArkZCZtihOY)
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYG1ccCd5syBba53yXO2QqOtQhbCq7iGJ6jRm7s3-LPSAIeUNGZqvGNkJBdZr3eDlz1T9CNymP-5k0qr9A58PJ_pAQOb59sIQ)
- **External Actor를 그리고, 크기를 조정한다.**
    - External Actor를 선택하고, 마우스 가운데 버튼을 누르면 크기가 자동으로 조절된다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihayFAKwDmD2y7A1CBDEPTuSCJnLMGa0YmljxCwPIlzfd9PN4FRhoLL9AjuBXNgem1RnO6m42Xm9IfjaPMOqNpLBxwfjvSSIN4o)
- **Subsystem의 기능을 나타내는 Subsystem Use Case를 추가한다.**
    - “Inflate Driver Airbag” Subsystem Use Case는 충돌을 감지하면 Airbag을 전개하여 운전자를 보호하는 기능이다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbneeJwc--3I2D3FKSmO_jHnkZY5Xj43sOeAZotCfwKGIV9HkISctxnqpwSgxoooMh0mZoMtSf_-cZ7jZLbav_ZkCnMsmqV0ls)
- **Subsystem Use Case를 그리고, 크기를 조정한다.**
    - Subsystem Usage를 선택하고, 마우스 가운데 버튼을 누르면 크기가 자동으로 조절된다.
- **Drawing Toolbar를 통해 Subsystem Use Case와 External Actor간의 Association을 설정한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpiha3-zR-1NMS-o6BMyRp1VEEJv8azZdKgeu3F340BvaHQhFmpxDuS0CNiv7NCRjyBkDGpCouOcqk85sXdC90INHCZ9ljVpLa2w)
- **Subsystem Use Case Workflow Diagram(Sequence Diagram)을 Open한다.**
- **External Actor와 Subsystem Use Case를 Sequence Diagram으로 Drag & Drop한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYsz-ELev4hkYauX6l10bfo0V_R-sdZeqzzQcYvBTwn6FpkSgv14fbgGoTL0XVekubtVDS9Xx9dxeZEqlfzxJKSM10ONNPFPZ8)
- **Message를 그리고 Ctrl + Shift를 눌러 RapidAUTO Editor를 Open한다.**
- **Message를 입력하고 Enter 키를 입력하면 RapidAUTO Editor가 닫히면서 입력한 내용이 Message에 표시된다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZho8ig8MerYgG3uMPi91mx8cwUaIkrO98ft6Ji15RPNoPmFYVeX-EjAf_I_NijecVeYxHELaUuduJvASQKfVftjE2YCHlfB0g)
- **Interaction Operator를 사용하여 Branch(Decision)을 표현한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZESBgQ4__1E5P48yH-NW_XfKYKh9_7A1uC6kH7JfwOKgUqmipzzRj566pUeHDJ5JzgkoexIUzB4KJNtcV8zIKZVWSS61VLqEw)

- **완성된 Sequence Diagram은 다음과 같다. (왼쪽)**
- **완성된 Subsystem Use Case 모델은 다음과 같다. (오른쪽)**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYLbE6iOXu0H4IradFEr_7f4t08frIiKgfTqf2qzQPXNIHXuMxfACAzgz5WXotwbjX61rQXnUVqBDkel0xjJJPF0tbXC70-x-Y)

- **Subsystem Use Case와 System Requirement간에 추적성을 설정한다.**
    - 추적성 방향은 Subsystem Use Case → System Requirement로 설정한다. 
    - Subsystem Use Case를 선택한 후, RapidAUTO Tray > “Add Trace” > “Select Src” 클릭한다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihY_5tu6WPRfBMZljDhU88kzuu5PRLsWh92WjHyAjLYmVGVPiZbK0KZCmMskkdDgUesOeMJeFVqFTm1JZnNfOOFHrf-kFsYx5X4)

- **Subsystem Use Case와 System Requirement간에 추적성을 설정한다.**
    - Subsystem Requirement를 선택한 후, RapidAUTO Tray > “Add Trace” > “Select Dest” 클릭한다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaDkDxazirl-h756B7AlGPH6HHgu3lXro8wNkh97SMipy2CKLdTW-LcmVSFxMQUuEzlbZESXq6bphryw2LPT20Fljp3iqjjpg)

- **System Requirement를 Use Case Diagram으로 Drag & Drop하여, 추적성 관계를 표현한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYHqi5syI_9ff0xN0Vu3TePaItIQOJZbBGJ4nfHCRiIHP8q1Od173mxyGRTpov77Eo1uHtPDJw8oIs4FahLzQe1o0qvhvXCiw)

- **설정한 추적성 정보를 Gateway가 인식할 수 있는 형태로 변환한다.**
    - RapidAUTO Tray > “Create/Update Traceability Link” 클릭<br>
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYN7KRtI6mak6-vLhfzc0JkpggXDmBcYkNXAItibuhoYVEwd_y_3xsnJYvo3v2iEDO4k8IfnwvVHYbw4QTv_eeixRS0qH9VfVw)

- **System Model에 대한 추적성 모델(Gateway Project)을 생성한다.**
    - Tree Root > RapidAUTO Context Menu > “Create Traceability Model” 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYpy2k8EIA1AkJh_HCOJ8L3po1IvaRF-mAB-LOgt-kmOTqdh4eWNCa1j1TukfNxoQdenzGUmCU9kmym34ov7Puyq-478rZsJuk)

- **“SYSRS” ↔ “SYSUC”간 Coverage Ratio를 확인한다. (100%)**
    - “Management View” 탭 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYCPCvznyJZB6QRrd2NfWvdiOsdOul75I6gQuuWE5X3PAxfzB-T7WaBTRDlAJ6Ohd3k1ceWYTdRZyO7PVGsi14iJhrhgBZb1gc)

- **“SYSRS” ↔ “SYSUC”간 상세 추적 관계를 확인한다.**
    - “Graphical View” 탭 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYJaX991y4rs0osSFde6BdzpRlTjZbOnO0Qm3OomcXdffWv5eUUm71OqOLEcDBcZCqQJBr4WFcl0VRnfLddwIDNYwtLP-XGfw)

## Item Definition
- **“Item Definition” Package의 구조를 검토한다.**
    1. External Actor에 대응되는 ID Logical External Element 확인
    2. ID_Subsystem_DriverAirbag : Subsystem 내의 Block 설계
    3. ID_IF_DriverAirbag : Subsystem 내의 Interface 설계
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYhBDwjqxMcr5ARALe9qks7RCJfB3xoIJ_C5TkmFjwreqvxnrXWxrZuv42AVSuwAwCBrzfaVsSgHrPoAKNjQ00fwIPfMlz2fJ4)

- **“Item Definition” Package의 구조를 검토한다.**
    1. 전체 Subsystem을 통합하는 Composite Block 확인 
    2. Use Case에 대응되는 Static Design, Dynamic Design을 저장하는 Use Case Realization 확인 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihY5Ub3bJ0f3tjUoOZFzcPvT-CcH4XwnztHsWiIwKYfocxRt4he9Ln362c3M9rkgFJv6nh-bklOmwe8v46MEn8m2xa5qlQAWTIQ)

- **Use Case Realization 내의 VOPB Diagram을 Open 한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZXwaITV921nEN402lTQ-YPwTl448yeLoFc6MTpCiExy1IP-1QlzXEccqkR7oEdt-qMF-h3f3Yu0_6j2PRPhbGF57NMQNj3jO4)

- **실습을 통해 정의한 기능 부품(Logical Element)은 다음과 같다.**

| Logical (External) Element | Role                                                  |
|---------------------------|-------------------------------------------------------|
| Impact                    | 충돌을 추상화한 External Element                      |
| FrontImpactSensor         | 충돌을 감지하여, 충돌량의 크기를 판별하는 Logical Element |
| Algorithm                 | 충돌량 크기를 기반으로 충돌 여부를 판단하는 Logical Element |
| DriverAirbag              | 에어백 전개를 통해 충격량을 감쇄 시키는 Logical Element |
| Driver                    | 에어백 전개에 의해 충돌로부터 보호되는 운전자를 추상화한 External Element |

- **Logical Element를 생성한다. (FrontImpactSensor)**
    - ID_Subsystem Package > RapidAUTO Context Menu > “Add Logical Element” 클릭
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYmvEms7Lxh6Ble4_WmiJtT4SO_GAyYA7LmDIrXFYafESy0LsU-a2kkD0TbcajkLhKO-jA06vqhcd6_apJSx2kdWbq3Ah2e2Q)

- **Logical Element를 생성한다. (Algorithm, DriverAirbag)**
    - ID_Subsystem Package > RapidAUTO Context Menu > “Add Logical Element” 클릭<br>
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbiSWhqq5tVCJrIqvY6SOok3gP85r3whdErYbj9fvK7pZCwOw7YZoY_f4wqxkCm8P-Pd4cJCQUqppM6mIA-N2RUA6-0jqTKgXs)

- **IdSubsystem내에 생성된 Part를 Drag & Drop한 후, 마우스 가운데 버튼을 클릭하여 크기를 조정한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihb5efF3iYG7M2-2IhMTIMfDaL2W4zS0qNEhPV5YCbSKkJZ0mr34dqC9g8RSJQJ7G61vcsoz2527OoLRdvm3nl2-H7kE5-sTctU)

- **Logical External Element를 Drag & Drop 한 후, 마우스 가운데 버튼을 클릭하여 크기를 조정한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYrhG-6l06S3dvU-LcsLzw_Rc0x6PAwsk5HNmxBu5fSb4Z6PPNBzUU0TN9L3nqHc8TBKhKyTgrE5JtbMWXdlu3xV3Q5Bf2KoQ)

- **Impact와 Sensor간의 Flow Port와 Flow Specification를 정의한다.**
    - Impact는 충돌을 추상화한 External Element이다. 
    - Sensor는 충돌을 감지하여, 충돌량을 전류로 변환하는 Logical Element이다. 
    - 충돌은 Flow Port를 통해 전달되며, 충돌 에너지를 나타내기 위한 Flow Specification을 정의한다. 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaNzfXIWuhvWtrD3S-NhzyqlHhlnIYMd80kyzbxYXE5gXrienaw5bTWvS7quAC7g6SS7McSuHpk73dmG_0rup3R69yY78RqyH4)

- **“ICrashImpact” Flow Specification에 Flow Property를 정의한다.**
    - “crashImpact”라는 Flow Property를 정의한다. 
    - Flow Direction을 “In”으로 설정한다. (Require Port 기준) 
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaw5a6TAd3Y3e-7jj5TvgcJOlr-yZbZlBj_7fCIAUL6mhIpbqLepGDJhJA6HQkUmuutC3QDOYCP5WtuE039OxZU3ELknMNYIFw)

- **FrontImpactSensor에 “rCrashImpact” Flow Port를 정의한다.**
- **Flow Specification으로 Flow Port를 타입화 한다.**
    - Port를 선택한 후, 마우스 가운데 버튼을 클릭한다.  
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZ_4eqo36UV5th1n57zwBES_z6efAKFzS26lTR_hawacrGgeApycrN4wHjEXSO1iiepNJDOQatkszmJERkh7jZWRz0uVxt6olI)

- **앞에서와 동일한 방식으로 Flow Specification을 정의한다.**
    - Item Definition, FSC에서는 Value Type을 정의하지 않는다.

| Flow Specification   | Flow Properties  | Value Type | Flow Direction |
|----------------------|------------------|------------|----------------|
| Impact               | crashImpact      | 미정       | In             |
| FrontImpactSensor    | sensorInfo       | 미정       | In             |
| Algorithm            | deploymentInfo   | 미정       | In             |
| DriverAirbag         | cushionedImpact  | 미정       | In             |

![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbVsZBisGtPrmv049Ew01efGIpMo5TQNyGlcbIq_T65NB-42J65gJqZZA0zupO05KDW3-23IevPyS0_BC_oTCWw1NCf6RvhWA)

- **앞에서와 동일한 방식으로 Flow Port를 정의한다.**
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZVFqkMjCsUsxjw0MPxEGtreecjn7wuQvyM21Eei_WRyUcCspKZtzn81Am0_6p1HYcWToaguMaOwAzVLKXMTlm6ddnjJrL-iBM)
