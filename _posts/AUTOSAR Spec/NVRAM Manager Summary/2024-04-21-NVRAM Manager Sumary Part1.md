---
layout: post
titile: "NVRAM Manager Sumary Part1"
author_profile: true
categories:
  [AUTOSAR-Spec, NVRAM Manager Summary]
tags: [AUTOSAR-Spec, NVRAM Manager Summary]
toc: true
toc_sticky: true
sidebar:
    nav: "sidebar-category"
---
After reading this article you will learn :

1. Basic working principle of NV data handling

2. Basic storage objects (RAM block, RAM Block, ROM Block and Administrative Block)

3. Block management types ( Native Data block, Redundant Data Block and Data set Block)

# **1. Basic Working Principle :**

Consider fig.1 shown below, in case of write data **(Blue arrow)**, SWC has data to write in NV memory, data is copied to RAM block then data is copied to NV Memory. Similar is the case while reading data **(Yellow arrow)**, first data is copied to RAM block  from NV Block then SWC read data from RAM block or RAM mirror.

아래 그림 1을 보면 데이터 write **(파란색 화살표)**의 경우 SWC에 NV 메모리에 쓸 데이터가 있고 데이터가 RAM 블록에 복사된 다음 데이터가 NV 메모리에 복사됩니다. 비슷한 경우가 데이터를 읽는 동안 **(노란색 화살표)**, 첫 번째 데이터가 NV 블록에서 RAM 블록으로 복사된 다음 SWC가 RAM 블록 또는 RAM 미러에서 데이터를 읽습니다.

---
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihaGuxZoKOxmHCb5-RKJV3GNWFm0YMxHipnXkW_mWmN_GMRHyNZtOHDoQLsULj-yLjN3Jubh6eicF9amVhwTLs1KBwIK7TTR9gY)
<center>**Fig.1 Basic Working Mechanism of MemStack**</center>

---

### **NV Block and Block IDs**

NV blocks are used to store NV data and present into NV memory. Each block is associated with block ID and this is maintained in NV Block descriptor table.

NV 블록은 NV 데이터를 저장하고 NV 메모리에 표시하는 데 사용됩니다. 각 블록은 블록 ID와 연관되며 이는 NV 블록 descriptor table에 유지됩니다.

e.g. if you have data related to vehicle speed e.g. max speed, min speed,  this data you can see on console of car, this data is stored in NV memory. There is a block dedicated to Vehicle Speed parameters and a block ID corresponding to a block. Similarly to store errors related to create a bock for vehicle error and assign a block ID using configuration tools.

예를 들어 차량 속도와 관련된 데이터(ex : 최대 속도, 최소 속도)가 있는 경우 이 데이터는 자동차 콘솔에서 볼 수 있으며 이 데이터는 NV 메모리에 저장됩니다. 차량 속도 parameter 전용 블록과 블록에 해당하는 블록 ID가 있습니다. 유사하게 차량 오류에 대한 블록 생성과 관련된 오류를 저장하고 구성 도구를 사용하여 블록 ID를 할당합니다.

# **2. Basic Storage Object and Block Management :**

To understand memory stack you should understand basic storage object used in memory stack and block management. Basic storage object is smallest entity of memory block. Basic storage objects are : **RAM block, ROM block, NV block and  Administrative block.**

메모리 스택을 이해하기 위해서는 메모리 스택과 블록 관리에서 사용되는 Basic storage object를 이해해야 합니다. Basic storage object는 메모리 블록의 가장 작은 엔터티입니다. **Basic storage object**는 **RAM 블록, ROM 블록, NV 블록 및 Administrative 블록입니다.**

**To store data into NV memory** and  to handle data to be stored into NV memory, a concept used called **memory blocks,** e.g. if you have a data related to Vehicle Speed  (Vspeed), this Vspeed is to be stored in NV memory. Size of Vspeed is 2 bytes, so to store Vspeed in NV memory **blocks of memory is reserved in NV memory called NV blocks.**

**데이터를 NV 메모리에 저장하고** NV 메모리에 저장할 데이터를 처리하기 위해 메모리 블록이라는 개념이 사용되었습니다. 차량 속도(Vspeed)와 관련된 데이터가 있는 경우 이 Vspeed는 NV 메모리에 저장됩니다. Vspeed의 크기는 2바이트이므로 Vspeed를 NV 메모리 블록에 저장하기 위해 **메모리 블록은 NV 블록이라는 NV 메모리에 예약되어 있습니다.**

Application (SWC) which uses VIN, uses RAM blocks to handle VIN. **Now what is use of RAM blocks if you already  have NV block?.** Application (SWCs) holds data into RAM block, at time of writing data in NV memory, data from RAM block is copied into NV block, at  time of data read, data from NV block is copied to RAM block. **RAM block is required as there is limitation of NV memory write cycle** (e.g. 10,000 write cycles), so data is held in RAM blocks and written to NV blocks when required, by doing so NV memory's life (write cycle) is maintained.

VIN을 사용하는 Application(SWC)은 RAM 블록을 사용하여 VIN을 처리합니다. **이제 NV 블록이 이미 있는 경우 RAM 블록을 사용하는 것은 무엇입니까?** Application(SWC)은 데이터를 RAM 블록에 보관하고, NV 메모리에 데이터를 쓸 때 RAM 블록의 데이터가 NV 블록으로 복사되고, 데이터를 읽을 때 NV 블록의 데이터가 RAM 블록으로 박사됩니다. **NV 메모리 write cycle**(ex : 10,000 write cycles)**의 제한이 있으므로 RAM 블록이 요구된다,** 그래서 데이터가 RAM 블록에 보관되고 필요할 때 NV 블록에 기록되므로 NV 메모리의 수명(write cycle)이 유지됩니다.

---
![Imgur](https://lh3.googleusercontent.com/u/0/drive-viewer/AKGpihbO4m1q4knQ6WLEAI0eL_jrIRYIh4fOzxZsd5IFBAb05F4XLVfQsrecN99drKhFn5iG1u8_LV9sbigitr44trQa5Kb8AU-MFA)
<center>**Fig. 2 Basic Storage Objects**</center>

---

## **2.1 Basic Storage Objects :**

Below are the basic storage objects

**NV Block :** NV block is basic storage object and resides in NV Memory. NV block structure is shown in fig.2. NV Block made up of optional CRC, optional Header and NV memory.

**NV Block :** NV 블록은 basic storage object이며 NV 메모리에 상주합니다. NV 블록 구조는 그림 2에 나와 있습니다. NV 블록은 optional CRC, optional 헤더 및 NV 메모리로 구성되었다.

**RAM Block :** RAM block basic storage object resides in RAM. RAM blocks can be permanent or temporary. Each RAM block corresponds to a particular NV Block. Structure of RAM block is as per  fig.2. RAM block has optional CRC (is available if corresponding NV block is having a CRC) and Header block and RAM memory.

**RAM Block :** RAM 블록은 basic storage object는 RAM에 상주합니다. RAM 블록은 영구적이거나 일시적일 수 있습니다. 각 RAM 블록은 특정 NV 블록에 해당합니다. RAM 블록의 구조는 그림 2와 같습니다. RAM 블록에는 optional CRC(해당 NV 블록에 CRC가 있는 경우 사용 가능)와 헤더 블록 및 RAM 메모리가 있습니다.

**ROM Block :** Rom block in ROM. It do not have CRC or Header refer fig.2. ROM block contains default data. This data is used in case of error in NV block (this will be explained in error recovery section)

**ROM Block :** ROM안에 ROM 블록입니다. 그것은 CRC 또는 헤더를 가지고 있지 않습니다. 그림2를 참조하십시오. ROM 블록에는 basic storage object가 포함되어 있습니다. NV 블록에서 오류가 발생한 경우에 사용됩니다. (이는 오류 복구 섹션에서 설명합니다)

**Administrative Block :** Administrative block resides in RAM and this block is mandatory. It is used to hold status of corresponding RAM block (discussed in Part -2 of NVRAM manager).

**Administrative Block :** Administrative 블록은 RAM에 상주하며 이 블록은 필수입니다. 해당 RAM 블록의 상태를 유지하는 데 사용됩니다.

## **2.2 Block Management Types :**

In above section we have discussed about basic storage object, in this section we will discuss about block management types. NVRAM block contains RAM block, ROM block, NV block and Administrative block. Combinations of these blocks can be configured to create different types of  NVRAM block. Below are the block management types supported by NvM :

위 섹션에서 우리는 basic storage object에 대해 논의했으며, 이 섹션에서는 블록 관리 타입에 대해 논의할 것입니다. NVRAM 블록에는 RAM 블록, ROM 블록, NV 블록 및 Administrative 블록이 포함됩니다. 이러한 블록의 조합을 구성하여 다양한 타입의 NVRAM 블록을 생성할 수 있습니다. 다음은 NvM에서 지원하는 블록 관리 타입입니다.

### **1. Native Block :**

The Native NVRAM block is the simplest block management type. It allows storage to/retrieval from NV memory with a minimum of overhead.

In case of native block, there will be 1 NV block created in NV memory. Then 1 RAM block will be created, as RAM block is required to provide data to application /run time. 1 ROM block to store default values and 1 Administrative block.

Native NVRAM 블록은 가장 간단한 블록 관리 타입입니다. 최소한의 오버헤드로 NV 메모리에 저장/회수할 수 있습니다.

Native 블록의 경우 NV 메모리에 1개의 NV 블록이 생성됩니다. 그러면 application/run time에 데이터를 제공하기 위해 RAM 블록이 필요하므로 1개의 RAM 블록이 생성됩니다. 기본값을 저장하기 위한 ROM 블록 1개와 Administrative 블록 1개

NV Block : 1

RAM Block : 1

ROM Block : 1 or 1

Administrative Block : 1

### **2. Redundant Block :**

In addition to the Native NVRAM block, the Redundant NVRAM block provides enhanced fault tolerance, reliability and availability. It increases resistance against data corruption.

So if one NV block fails, we have still have one more block, from which data can be retrieved. This will help to keep data secure in NV memory.

In case of Redundant block there will be 2 NV blocks but 1 RAM block as application will require only a copy if valid data, so valid data NV block will be copied to RAM. ROM block will be required as 1 and 1 Administrative block.

Native NVRAM 블록 외에도 Redundant 블록은 향상된 내결함성, 안정성 및 가용성을 제공합니다. 데이터 손상에 대한 저항력을 높입니다.

따라서 하나의 NV 블록이 실패하더라도 데이터를 검색할 수 있는 블록이 하나 더 있습니다. 이렇게 하면 NV 메모리에서 데이터를 안전하게 유지하는 데 도움이 됩니다.

Redundant 블록의 경우 2개의 NV 블록이 있지만 application이 유효한 데이터인 경우 복사만 필요하므로 1개의 RAM 블록이 있으므로 유효환 데이터 NV 블록이 RAM에 복사됩니다. ROM 블록은 0 or 1개가 필요하고 Administrative 블록은 1개가 필요합니다.

NV Block : 2

RAM Block : 1

ROM Block : 0 or 1

Administrative Block : 1

### **3. Data Set Block :**

The Dataset NVRAM block is an array of equally sized data blocks (NV/ROM). The application can at one time access exactly one of these elements. A specific dataset element is accessed by setting the corresponding index using the API NvM_SetDataIndex.

Dataset NVRAM 블록은 동일한 크기의 데이터 블록(NV/ROM)의 배열입니다. Application은 한 번에 이러한 요소 중 정확히 하나에 액세스할 수 있습니다. API NvM_SetDataIndex를 사용하여 해당 인덱스를 설정하여 특정 데이터 세트 요소에 액세스합니다.

NV Block : 1...n

RAM Block : 1

ROM Block : 0 or n

Administrative Block : 1