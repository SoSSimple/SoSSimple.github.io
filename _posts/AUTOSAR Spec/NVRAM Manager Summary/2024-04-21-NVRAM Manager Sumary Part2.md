---
layout: post
titile: "NVRAM Manager Sumary Part2"
author_profile: true
categories:
  [AUTOSAR-Spec, NVRAM Manager Summary]
tags: [AUTOSAR-Spec, NVRAM Manager Summary]
toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---
[In Part -1 of NVRAM manager (NvM)]({{site.url}}/posts/NVRAM-Manager-Sumary-Part1/) you have understood working principle of NVRAM manager  Basic storage object. After reading this article you will understand :

# Overview

1. Services provided by NvM module.

2  Read block and Write block mechanism.

3. Read all and Write all  functionality.

4. Error Detection mechanism.

5. Error recovery (Implicit and Explicit ) methods.

7. Write Protection mechanism.

8. Resistant to changed software.

9. RAM block and NV block synchronization (Implicit and Explicit) methods.

10. Service ports provided by NvM module.11. Accessing NVM using RTE

12. Important Configuration Parameter of NvM module in AUTOSAR

# **1. Introduction :**

**NvM 모듈은 AUTOSAR의 서비스 계층에 있으며 NV(Non-Volatile) 데이터 스토리지(ex : 읽기 및 쓰기 서비스) 및 유지 관리와 관련하여 SWC에 서비스를 제공합니다.** NvM 모듈은 EEPROM 및 FLASH EEPROM 모듈의 NV 데이터를 관리합니다. NV RAM 매니저는 NV 메모리에 액세스할 수 있는 유일한 모듈입니다.

메모리 스택을 구성하려면 NV RAM 매니저의 기능을 이해해야 합니다.

**NV RAM이 하는 일 :**

1. 메모리 읽기/쓰기를 위해 SWC 및 BSW 모듈에 서비스를 제공합니다.
2. NV 메모리의 오류 감지 및 오류 복구(implicit or explicit)를 수행합니다.
3. RAM 블록 데이터의 유효성 검사.
4. 멀티 블록 읽기 및 쓰기 요청을 제공합니다.
5. Write verification을 수행합니다.

아래 섹션에서는  NVRAM 매니저의 기능에 대해 설명합니다.

# **2. Functionalities of NvM Module :**

이 섹션에서는 NVRAM 모듈의 중요한 기능에 대해 설명합니다.

## **2.1 RAM Block States :**

다른 API 호출에 따라 RAM 블록의 상태가 변경됩니다. NVRAM 매니저는 RAM 블록의 상태를 변경하며 이러한 상태는 읽기 또는 쓰기 중에 사용됩니다. **각 RAM 블록의 상태는 Administrative 블록에 저장됩니다.** RAM 블록의 세 가지 상태는 다음과 같습니다.

1.Valid Unchanged  2.Valid Changed  3. Invalid Unchanged.

### **1. Valid Unchanged :**

이 상태는 RAM 블록의 내용이 변경되지 않고 NV 메모리와 동일함을 나타냅니다. 이 상태로 들어가려면 싱글 블록 또는 멀티 블록 요청에 대한 읽기/쓰기가 성공해야 합니다.
(즉, **NvM_ReadBlock() / NvMReadAll()** 또는 **NvM_WriteBlock() / NvM_WriteAll()** 상태가 Valid Unchanged로 변경됨)

### **2. Valid Changed :**

이 상태는 RAM 블록의 내용이 변경되어음을 나타냅니다.

- NvM_SetRamBlockStatus가 블록에 대해 TRUE로 호출된 경우 또는
- write all 또는 write 블록 요청이 진행중인 경우 또는
- 블록에 대해 호출된 NvM_RestoreBlockDefaults가 성공적으로 완료되면 RAM 블록의 상태가 Valid Changed로 변경됩니다.
- read all 또는 read 블록은 default 데이터 제공

### **3. Invalid Unchanged :**

이는 RAM 블록의 상태가 유효하지 않음을 의미합니다. RAM 블록의 초기화 후 상태는 Invalid Unchanged입니다. INVALID / UNCHANGED 상태가 되려면 다음 중 하나 이상이 발생해햐 합니다.

- NvM_SetRamBlockStatus가 블록에 대해 FALSE로 호출된 경우
- read 블록 / read all / write 블록 / write all 작업(operation)은 실패
- NvM_EraseNvBlock이 블록에 대해 성공적으로 완료됨
- NvM_InvalidateNvBlock이 블록에 대해 성공적으로 완료됨

## **2.2 Read Block and Read All :**

read 블록 및 read all 요청은 비동기식 요청입니다. read 블록 기능은 싱글 블록(즉, 특정 NV 블록)을 읽습니다. NV 블록에서 데이터를 읽고 임시 RAM 블록을 업데이트합니다. **NvM_ReadBlock()** 함수는 이 용도로 사용되며 NvM_ReadBlock() 함수는 NV 메모리에서 **Temporary RAM 블록**으로 데이터를 복사합니다.

**NvM_ReadPRAMBlock()** 기능은 NV 블록에서 **Permanent RAM 블록**으로 데이터를 복사합니다.

**Read all** 기능은 한 번에 모든 NV 블록을 읽습니다. **NvM_ReadAll()**은 NV 블록에서 데이터를 읽고 RAM 블록으로 로드합니다. 블록을 읽는 동안 오류가 있는 경우 **즉, RAM 블록의 콘텐츠가 NV 블록과 일치하지 않는 경우 즉, CRC 오류 및 NV에서 읽기 실패(Invalid Unchanged), 기본 데이터가 로드됩니다. RAM 블록에 저장하고 블록이 Redundant 타입인 경우 Redundant 블록에서 읽기가 수행되고 이것이 실패하면 기본값이 RAM 블록에 로드됩니다.** NvM_ReadAll() 함수는 ECU 초기화 시 사용되며 ECU 상태 매니저에 의해 호출됩니다.

read 블록 또는 read all request 후 요청이 성공하면 RAM 블록의 상태가 Valid Unchanged로 변경됩니다.

### **2.2.2 Static ID check :**

NV 블록이 NV 메모리에 기록되면 ID, 즉 static 블록 ID도 NV 메모리에 기록됩니다. static ID는 블록 헤더에 저장됩니다. 이 ID는 요청된 블록 ID 대해 확인됩니다. 예를 들어 메모리에 ID 10의 블록을 작성하고 ID 10의 read 블록을 요청한 경우 요청된 ID와 저장된 블록 ID를 비교하여 오류를 감지합니다. 잘못된 블록 ID 오류가 감지되면 NVM_E_WRONG_BLOCK_ID 오류가 DEM에 보고됩니다.

static ID check는 configuration parameter를 사용하여 활성화할 수 있습니다. RAM 블록에 대한 static ID check에 실패한 경우 RAM 블록의 상태가 **Invalid Unchanged**로 변경되었습니다.

### **2.2.3 Read Retries :**

Read Retries는 NV 데이터 읽기 중 오류가 발생했을 때 수행됩니다. 즉, NVRAM 매니저는 NV 데이터 읽기를 다시 시도하고, NVRAM 매니저는 configuration parameter(NVM_MAX_NUM_OF_READ_RETRIES)의 값에 따라 읽기 재시도를 수행합니다. 예를 들어 Read Retries 값이 2이면 첫 번째 read 블록에 오류가 있으면 NvM이 두 번째 블록을 읽습니다.

여전히 오류가 있는 경우 Read Retries 후 NVRAM 매니저는 ROM 블록을 읽거나 Redundant NV 블록(구성된경우)을 읽습니다.

1. NVRAM 매니저가 CRC 오류를 감지한 경우
2. NVRAM 매니저가 static 블록 ID check를 감지한 경우

## **2.3 Write Block and Write All:**

write 블록 및 write all 요청은 비동기식 요청입니다. write 블록 기능은 싱글 블록을 Temporary ****메모리에 write합니다. 블록에 write하기 위해 **NvM_WriteBlock()** 함수가 사용됩니다. 이 기능은 Temporary ****RAM 블록에서 ROM 블록으로 데이터를 복사합니다. **NvM_WritePRAMBlock()은 Permanent RAM 블록에서 NV 블록으로 데이터를 write합니다.**

write all은 한 번에 모든 블록을 write합니다. **NvM_WriteAll()** 함수는 write 시 CRC를 계산합니다. CRC가 일치하고 RAM 블록 상태가 유효하면 해당 블록이 NV 메모리에 기록되지 않습니다. Permanent RAM 블록의 경우 : 상태는 API(NvM_SetRamBlockStatus())에 의해 VALID_CHANGED로 변경될 수 있으며 NvM_WriteAll() 시에 기록될 수 있습니다.

write 블록 및 NvM_WriteAll이 성공하면 RAM 블록 상태가 Valid Unchanged로 변경되고 write 블록 및 NvM_WriteAll 요청 처리 중에 RAM 블록 상태가 Valid Changed로 변경됩니다.

### **2.3.1 Write Protection :**

write protection이 활성화되면 RAM 블록은 수정할 수 있지만 NV 블록은 수정할 수 없습니다. write protection은 **NvMBlockWriteProt = TRUE** 매개변수를 구성하여 수행됩니다.

**NvMWriteBlockOnce가 FALSE인 경우** (NvMBlockWriteProt 값에 관계 없이) **NvM_SetBlockProtection() 함수를 사용하여 write protection을 활성화/비활성화할 수 있습니다(explicit 메커니즘).**

**NvMWriteBlockOnce가 True**이면 RAM 블록의 NvM_SetBlockProtection() 함수 Valid verification을 사용하여 활성화/비활성화할 수 없습니다.

### **2.3.2 Write Verification :**

RAM 블록이 NV 메모리에 기록되면 NV 블록을 즉시 다시 읽어 RAM 블록의 원래 내용과 비교해야 합니다. Write verification 은 **NvMWriteVerification** 매개변수를 사용하여 구성할 수 있습니다. RAM 블록의 원본 콘텐츠가 read back과 동일하지 않은 경우 production code error(NVM_C_VERIFY_FAILED) 가 DEM에 보고됩니다. write retries가 구성되고 Write verification이 실패하면 NVRAM 매니저가 데이터 write를 다시 시도합니다. write retries는 NvMMaxNumOfWriteRetries 매개변수로 구성할 수 있습니다. 최대 재시도 횟수에 도달하면 NVM_E_REQ_FAILED 오류가 발생합니다.

## **2.4 Error Detection NVRAM Block Data Consistency check :**

**오류는 CRC 비교 메커니즘에 의해 감지됩니다.** NvM 모듈은 구성 가능한 옵션으로 NVRAM 블록에 대한 CRC를 확인하고 생성하기 위해 내부적으로 CRC 생성 루틴(8/16/32비트)을 사용합니다. RAM 블록의 CRC와 ROM 블록의 CRC가 일치하지 않으면 에러가 발생합니다. CRC 계산 메커니즘을 사용하여 데이터 일관성을 확인합니다. implicit 방법은 configuration parameter**(NvMBlockUseCrc 및 NvMCalcRamBlockCrc)**를 사용 설정하는 것입니다.

1. NvM_SetRamBlockStatus()가 호출되고 NvMCalcRamBlockCrc == TRUE인 경우 CRC가 계산됩니다.
2. CRC는 NvM_Main() 함수에 의해 백그라운드에서 계산됩니다.
3. CRC는 NvM_ReadBlock()이 처리되고 NvMCalcRamBlockCrc == TRUE 후에 계산됩니다.
4. NvM_WriteBlock() 및 NvM_WriteAll()이 NV 블록에 쓰기 전에 CRC 계산을 요청할 때 CRC가 계산됩니다.
5. CRC는 NvM_RestoreBlockDefaults() 이후 계산되며, RAM 블록의 crc가 계산됩니다.
6. CRC는 NvM_ReadAll() 동안 계산됩니다.

CRC 불일치가 발생하면 CRC 불일치 후 **Integrity fail** 오류가 발생합니다.

## **2.5 Error Recovery :**

Read NV 데이터 또는 Write NV 데이터를 위해 Error Recovery가 제공됩니다. 데이터를 Read 중 Error가 발생하면 기본값이 ROM 블록에서 RAM 블록으로 로드됩니다.

**write retries는 데이터를 쓰는 동안 오류가 발생하는 경우(ex : RAM 블록의 내용이 쓰여진 것과 동일하지 않은 경우) 오류 복구 타입입니다.**

**ROM 데이터로 RAM 데이터 복구(implicit) :** 

시작 시(NvM_ReadAll() 시점) 조건에서 implicit 복구 제공

1. ROM 블록이 구성되었다 그리고
2. RAM 블록과 ROM 블록의 CRC 불일치 그리고
3. NV에서 Read retries가 실패합니다.

**ROM 데이터로 RAM 데이터 복구(explicit) :** 

explicit 복구는 API(NvM_RestoreBlockDefaults() 또는 NvM_RestorePRAMBlockDefaults())를 호출하여 수행됩니다. application이 런타임에 오류를 감지하면 Application SWC에서 이러한 API를 호출합니다.

## **2.6 Initialization and Shutdown of NVRAM Module :**

NvM은 NvM_Init() 함수를 호출하여 ECU 상태 매니저에 의해 초기화되지만 이 함수에는 RAM 블록의 초기화가 포함되어 있지 않습니다. RAM 블록은 NvM_ReadAll() 함수에 의해 초기화됩니다. NvMReadAll()은 NV 블록에서 RAM 블록으로 데이터를 복사하고 초기화합니다. shutdown은 NvM_WriteAll()을 호출하여 수행되며 이 함수는 RAM 블록에서 NV 블록으로 데이터를 write합니다.

## **2.7 Resistance to SW Change :**

이 섹션에서는 NvMDynamicConfiguration 및 NvMResistantToChangedSw의 두 가지 configuration parameter에 대해 설명합니다.

이미 4개의 NV 블록을 구성했다고 생각한다면 데이터가 write됩니다. 이제 블록 중 하나의 블록 크기를 변경했거나 새 블록이 추가되어 다른 블록의 ID가 변경되어 모든 read 프로세스에서 잘못된 데이터 read가 발생합니다.

이러한 경우 NV 블록 데이터로 RAM 블록을 초기화하지 않는 옵션이 있을 수 있습니다. 이는 **NvMDynamicConfiguration = TRUE** 매개변수를 설정하여 수행할 수 있으며 configuration 변경은 configuration parameter**(NvMCompiledConfigID)**로 알려야 합니다.

NvmCompiledConfigID가 무엇인가요? 이것은 별도의 NV 블록에 저장되는 configuration parameter이며 시작 시 **NvmCompiledConfigID의 값이 NV 블록에 저장된 값과 비교됩니다. 비교된 값이 다른 경우 기본값이 RAM 블록에 로드되고 write 시 모든 새 값이 NV 블록에 write됩니다. 따라서 다음 시작 cycle에서 NvMResistantToChangedSw** 매개변수가 **TRUE**로 설정되고 **NvMResistantToChangedSw** 매개변수가 **false**로 설정된 경우 RAM 블록에 새 값을 사용할 수 있습니다. 그러면 RAM 블록의 유효성에 관계없이 기본값이 RAM 블록에 로드됩니다.

**In short :**

1. NV 블록에 오래된 데이터가 포함되어 있고 **NvMDynamicConfiguration=TRUE**라고 가정합니다.
2. configuration이 변경됩니다.
3. 읽을 때 모든 **NvmCompiledConfigID** 비교는 구성이 변경되었음을 보여줍니다.
4. **NvMResistantTochangedSw** 매개변수가 **TRUE**로 설정된 경우 NV 블록의 콘텐츠가 RAM 블록에 로드되어 불일치가 발생합니다(이전 블록의 데이터가 새 블록에 기록됨).
5. **NvMResistantToChangedSw** 매개변수가 **FALSE**로 설정된 경우 read all 시 기본값이 RAM 블록에 로드됩니다.

<aside>
💡 따라서 configuration을 더 잘 변경하려면 NvMResistantToChangedSw 매개변수를 **FALSE**로 설정하세요.

</aside>

## **2.8 Job End and Job Error Notification :**

NVM에서 데이터 읽기가 완료되거나 NVM에서 데이터 쓰기가 상태 ok로 완료되면 기본 메모리 추상화가 오류 없이 작업 종료를 알립니다. NvM_JobEndNotification()이 호출되고 있습니다. 마찬가지로 NVM에서 data read가 오류로 중단되거나 NVM에서 data write가 오류로 중단되면 NvM_JobErrorNotification()이 호출됩니다.

## **2.9 Immediate Data :**

데이터를 즉시 저장하도록 블록을 구성할 수 있습니다. 즉, NvM_Write가 요청되면 데이터가 NV 메모리에 즉시 write되어야 하며, 그런 다음 configuration parameter(immediate write = On)을 활성화할 수 있습니다.

일반적으로 immediate data가 비활성화되어 있고 NV 데이터 write는 write가 모두 처리 중일 때 (즉) 종료 시간에 수행됩니다. 그때까지 데이터가 변경되면 RAM 블록에 저장되며, Wrtie all은 싱글 블록 write를 호출하고 데이터가 변경되면 NV 메모리에 데이터를 write합니다.

# **3. NV data accessing :**

NVRAM 기본 작동 원리 및 basic storage objects (ex : RAM 블록, ROM 블록 및 NV 블록) 및 NV 데이터는 ROM 블록에서 RAM 블록으로 읽고 데이터를 쓰는 동안 RAM 블록에서 NV 블록으로 복사됩니다. **Application SWC는 NV 데이터를 읽고 NV 데이터를 쓰기 위해 각각 RAM 블록을 읽거나 씁니다.**

또한 **RAM 블록과  RAM 미러**가 있으며 이 섹션에서 이들 간의 차이점이 명확해집니다. RAM 블록을 사용하여 NV 블록과 Application SWC 간에 데이터 전송이 있으며 이 데이터 전송은 **explicit**이거나 **implicit**입니다.

**Application SWC가 데이터 Read 또는 Write를 요청하면 RAM 블록의 implicit 데이터가 NV 블록으로 복사되고 NV 블록에서 RAM 블록으로 복사됩니다. explicit 통신 NV 모듈이 RAM 블록에서 데이터를 읽고 쓰기 위한 API를 사용해야 하는 경우 Application  SWC가 데이터 Read 또는 Write를 요청**

## **3.1 Implicit data transfer between NV block and RAM block :**

implicit 통신에서는 RAM 블록이 사용됩니다. Application SWC와 NV 블록은 RAM 블록을 사용하여 데이터를 교환합니다. RAM 블록은 영구적이거나 일시적일 수 있습니다. **영구적인 RAM 블록은 하나의 Application SWC와 연결되며 영구적은 RAM 블록은 각 SWC에서만 액세스할 수 있습니다. 런타임 시 임시 램 블록이 생성됩니다. 즉, RAM 센션이 사용됩니다.**

---
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihYquVhQVY2HkpjYlnvOYnGPWFKLjprfz1PxdjUqvoJB2jRasx7CMKCUfaFHv6LWgHlZgaTVIBEuvNNk6uF4PG6OE9Ztk8xVLg)
<center>**Fig.4 Implicit Synchronization**</center>
---

### **3.1.1 Write Block Request :**

Write Operation :  Application SWC가 RAM 블록을 채우고 RAM 블록의 내용이 NV 블록으로 복사됩니다. Application SWC는 write 요청 시 아래의 규칙을 따라야 합니다.

1. Application은 API **NvM_WriteBlock()/NvM_WritePRAMBlock()**을 사용하여 RAM 블록을 채웁니다.
2. 이제 요청이 NvM  모듈로 전송되고 그때까지 **Application SWC는 데이터 일관성을 유지하기 위해 RAM 블록을 수정해서는 안 됩니다.**
3. 폴링 방식 또는 콜백 알림(작업 종료 알림) Application SWC는 요청 상태(Write Operation 성공 또는 실패)를 얻은 다음 Application 이후에 다시 RAM 블록을 사용할 수 있습니다.

### **3.1.2 Read Block Request :**

Read Operation : NV 블록은 RAM 블록을 업데이트하고 Application SWC는 RAM 블록을 읽습니다. Application SWC는 read 요청 시 아래의 규칙을 따라야 합니다.

1. NV 데이터를 읽으려면 Application SWC에서 RAM 블록을 제공해야 합니다.
2. API **NvM_ReadBlock() 또는 NvM_ReadPRAMBlock()**을 사용하여 NV에서 데이터를 읽습니다.
3. Read 요청 상태는 폴링 메커니즘 또는 콜백 알림을 통해 읽을 수 있으며 그때까지 Application SWC는 RAM 블록을 읽거나 쓰지 않아야 합니다.
4. 요청 성공 후 Application SWC는 RAM 블록 데이터를 사용할 수 있습니다.(RAM 블록에는 NV 블록의 데이터가 포함됨).

## **3.2 Explicit data transfer between NV block and RAM block :(이해 잘 안감)**

Explicit 동기화 RAM 미러는 RAM 블록과 NV 블록 간에 데이터를 전송하는 데 사용됩니다.
Application은 NV 블록을 사용하여 RAM 미러에 데이터를 씁니다. Application은 RAM 미러를 읽고 데이터를 NV 블록에 복사합니다.
Application은 NV 블록을 사용하여 RAM 미러에서 데이터를 읽고 Application은 RAM 미러를 쓰고 NV 블록에서 데이터를 복사합니다.

NV 블록을 사용하여 RAM 미러에 Application write 데이터가 **RAM 미러**를 읽고 NV 블록에 데이터를 복사합니다.
NV 블록을 사용하여 RAM 미러에 Application read 데이터는 **RAM 미러**를 쓰고 NV 블록에서 데이터를 복사합니다.

Explicit synchronization **RAM mirror** is used to transfer data between RAM block and NV Block.  Application writes/reads data to /from RAM Mirror using  NV Block reads/writes RAM mirror and copies data to/from NV Block.

**NvMBlockUseSyncMechanism 매개변수로 블록별로 Explicit 메커니즘을 구성할 수 있습니다.**

**NvMBlockUseSyncMechanism == TRUE.**

이 방법의 단점은 최대 RAM 블록 크기만큼의 추가 RAM 미러가 필요하다는 것입니다. 장점은 RAM 블록을 효율적으로 제어할 수 있고 데이터 일관성이 더 나은 방식으로 유지된다는 것입니다. RAM 블록 수에 관계없이 1개의 RAM 미러만 존재합니다( 1 RAM에서 구성해야 함).

---
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpiha0EgHqRnidv3PjSEAkoHuaKMCeGMPx0xyBRSKFb1TDnAIdUyNYwcMBq2DFRMP69ReBXD1007_fxLDt1vp7SeMEnexW0FH2NJY)
<center>**Fig.5 Explicit Synchronization**</center>
---

### **3.2.1 Write Data**

Application SWC는 데이터를 RAM 블록에 쓴 다음 데이터를 RAM 미러에 복사하고 NV 볼록은 RAM 미러에서 데이터를 읽습니다.  Write 요청 시 Application은 다음 사항을 따라야 합니다.

1. Application SWC는 API **NvM_WriteBlock() 또는 NvM_WritePRAMBlock()**을 사용하여 RAM 블록을 데이터로 채웁니다.
2. **NvM은 NvMWriteRamBlockToNvM()** 루틴을 호출하여 RAM 블록에서 데이터를 읽습니다. 이 루틴이 Application SWC 라고 불리며 일관성 있게 제공해야 합니다.
3. Application SWC는 데이터가 RAM 미러에 복사됨에 따라 RAM 블록을 읽고 쓸 수 있습니다.
4. Write Operation의 상태는 폴링 메커니즘 또는 콜백 기능으로 읽을 수 있습니다.

### **3.2.2 Read Data**

Application SWC는 RAM 미러에서  NV 데이터를 읽고(즉, RAM 미러에서 RAM 블록으로 데이터 복사) RAM 미러는 NV 블록에 의해 업데이트됩니다. 요청을 읽는 동안 Application은 아래 규칙을 따라야 합니다.

1. Application SWC는 **API NvM_ReadBlock() 또는 NvM_ReadPRAMBlock()**을 사용하여 RAM 블록에서 데이터를 읽습니다.
2. Application SWC는 데이터로 채워질 RAM 블록을 제공합니다.
3. NvM 모듈은 **NvMReadRamBlockFromNvM()** 함수를 호출하고 Application SWC는 데이터를 RAM 미러에서 RAM 블록으로 복사할 수 있습니다.
4. Read Operation 상태는 폴링 메커니즘 또는 콜백 기능으로 읽을 수 있습니다.

# **4. NvM Accessing by RTE:**

NvM 인터페이스에 액세스하기 위해 NVRAM 매니저는 Client-Server 또는 NvDataInterface에서 제공합니다. 일반 SWC는 Client-Server 인터페이스를 사용할 수 있으며 NvData 인터페이스는 NvBlock SWC와 함께 사용됩니다. 아래 이미지를 참고해주세요.

---
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihZLwVJxADx7jN73oRn6IH-VHT9ltatG_3bkJ9sVJCLN8xuBWbzjvk9_v7wKovcQ-BiwE3aGGRBh9fBz9TLtjqtXBSdqT9kMeg)
<center>**Fig.6 Accessing NVRAM Manager**</center>
---

그림 6은 Application SWC가 Client-Server 인터페이스를 사용하여 NVRAM 매니저에 액세스하는 것을 보여줍니다. 즉, NVRAM 매니저가 제공하는 서비스 포트를 사용합니다. 또한 Sender-Receiver를 사용하여 여러 SWC에서 액세스할 수 있는 NvBlock SWC 타입이 하나 더 있으며 NvBlock SWC는 NVRAM 매니저가 제공하는 서비스를 사용하기 위해 NVservice 포트를 사용합니다. SWC는 permanent RAM 블록 또는  temporary RAM 블록을 사용할 수 있으며 동기화 메커니즘을 기반으로 RAM 미러가 사용되거나 사용되지 않습니다.

따라서 SWC를 사용하여 NNRAM에 액세스하는 방법은 다음과 같습니다.

1. Application SWC는 RAM 미러가 있거나 없는 temporary RAM 블록을 사용합니다.
2. Application SWC는 RAM 미러가 있거나 없는 permanent RAM 블록을 사용합니다.
3. RAM 미러만 있는 NV SWC

## **4.1 Client-server interface :**

Client-Server 인터페이스는 Client가 Server에서 호출할 수 있으며 여러 Operation을 제공합니다. NvM이 Application에 제공하는 서비스의 경우 NvM은 서버 역할을 하고 Application은 클라이언트 역할을 합니다. 또한 NvM에서 Application에 제공할 알림도 동일하게 구현할 수 있습니다. 이 경우 Client-Server의 역할이 교환됩니다.

## **4.2 NvDataInterface :**

비휘발성 데이터 인터페이스는 비휘발성 블록 component와 atomic SWC 간에 교환할 다수의 VarialbeDataPrototype들을 정의합니다. 이러한 VariableDataPrototypes는 완전한 RAM 블록 또는 비휘발성 블록 component 내부에 구현된 RAM 블록의 요소에 매핑될 수 있습니다.

## **4.3 Service Ports :**

다음은 NVRAM 매니저가 제공하는 일부 서비스 포트(operation/service)입니다. SWC는 그 중 하나를 사용하여 서비스별로 제공되는 서비스를 포트로 접근할 수 있습니다.

**1 NvM_Admin :**

NvM_Admin은 NV 블록에 대한 write protection을 설정/재설정하는 서비스를 제공합니다. write protection application SWC와 관련된 작업을 수행하려면  SetBlockProtection() API를 사용할 수 있습니다. NvM_Admin 서비스 포트에서 제공하는 SetBlockProtection() Opeation입니다.

**2 NvM_Mirror :**

RAM 미러가 구성된 경우 Applictaion은 RAM 미러에서/로 데이터를 복사해야 합니다. 해당 콜백 기능이 사용(WriteRamBlockToNvM 및 ReadRamBlockFromNvM)되고 이러한 기능은 NvM_Mirror 서비스 포트에서 제공됩니다.

**3 NvM_NotifyInitBlock :**

기본 데이터를 RAM 블록에서 복원해야 할 때 NvM 모듈에서 콜백을 호출합니다. InitBlock()은 기본 데이터를 RAM 블록에 로드한 후 RAM 블록의 초기화가 완료될 때 (즉) NvM 모듈에서 호출하는 콜백입니다. 기본 값을 로드할 ROM 블록이 없으므로 InitBlock()이 호출됩니다.

**4 NvM_NotifyJobfinished :**

이 서비스 포트는 NvM 작업이 완료되면(즉, read 블록 또는 write 블록이 완료된 후) 호출되는 콜백을 제공합니다.

**5 NvM_Service :**

이것은 중요한 서비스 포트입니다. 이 서비스 포트는 Application SWC에 많은 작업을 제공합니다.

이러한 Operation/APIs은 다음과 같습니다.

1. EraseBlock()

2. ReadBlock()

3. WriteBlock()

4. RestoreBlockDefaults()

5. GetDataIndex()

6. GetErrorStatus()

7. InvalidateNVBlock()

8. ReadPRAMBlock()

9. WritePRAMBlock()

# **5. Important Configuration Parameters:**
## 5.1 **Paramters**
NVRAM 매니저를 구성하려면 다음과 같이 중요한 매개변수를 잊어서는 안 됩니다.

1. Block Id.

2. Block Length.

3. Is block (Is block should be processed at Read all or write all time).

4. **RAM block** (Temporary / Permanent).

5. **CRC calculation.**

6. **Write Protection.**

7. Resistance to change SW.

8. Job end notification.

9. Enable **Synchronization** ( i.e explicit synchronization)

10. If enabled synchronization then get and set ram mirror callbacks.

11. To provide default values, use of **ROM block.**

12. **Block type ( Native or redundant or Data set ).**