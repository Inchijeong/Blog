---
layout: post
title: "VirtualBox 에 CentOS 설치 및 설정"
categories:
  - Linux
tags:
  - Linux
---

## VirtualBox & CentOS 파일 다운로드

1. 아래 사이트에 들어가

   <https://www.virtualbox.org/wiki/Downloads>

   현재 컴퓨터의 맞는 버전의 VirtualBox를 설치받습니다.

![2932](https://user-images.githubusercontent.com/31085727/54729916-5e0e0e80-4bc9-11e9-928d-826fe0383ca4.png)

2. CentOS 공식 홈에서 Download 탭의 Minimal ISO 를 선택하여 CentOS를 설치받습니다.

   ![2931](https://user-images.githubusercontent.com/31085727/54729917-60706880-4bc9-11e9-907c-7a43d4164e9d.png)

   > 아래와 같이 다양한 다운로드 방법이 존재하는데 큰 차이는 없는것 같습니다.

   ![2934](https://user-images.githubusercontent.com/31085727/54730075-20f64c00-4bca-11e9-95c3-4301a86f245e.png)



## VirtualBox 에 CentOS 설치

1. 맨처음 받은 VirtualBox를 실행하여 Yes 를 눌러 진행하고, 앱을 실행시켜 새로만들기를 누릅니다.

   ![2933](https://user-images.githubusercontent.com/31085727/54730129-864a3d00-4bca-11e9-83c7-6a6f68f3dd01.png)

2. 이름을 지정하고 Linux read Hat (64-bit) 를 선택합니다.

   ![2935](https://user-images.githubusercontent.com/31085727/54730169-c4476100-4bca-11e9-8362-f2e03d1316e7.png)

3. 메모리 크기를 할당합니다.

   ![2935](https://user-images.githubusercontent.com/31085727/54730169-c4476100-4bca-11e9-8362-f2e03d1316e7.png)

4. 지금 새 가상 하드 디스크 만들기를 선택하고 만들기를 누릅니다.

   ![2936](https://user-images.githubusercontent.com/31085727/54730215-01abee80-4bcb-11e9-8913-16132f531c7a.png)

5. VDI를 선택하고 다음을 누릅니다.

   ![2938](https://user-images.githubusercontent.com/31085727/54730257-56e80000-4bcb-11e9-94e1-37ec3238b439.png)

6. 동적 할당을 선택합니다.

   ![2939](https://user-images.githubusercontent.com/31085727/54730323-90207000-4bcb-11e9-98e0-0909b1266ebe.png)

7. CentOS 인트턴스가 생긴것을 확인합니다.

   

## VirtualBox 설정



설정에 들어갑니다.

![940](https://user-images.githubusercontent.com/31085727/54730337-a8908a80-4bcb-11e9-9aa3-c56898c6776c.png)



- 시스템 탭
  - 플로피 디스크 체크 해제
  - 다음 순서가 되도록 설정
    1. 광 디스크
    2. 하드 디스크

![2941](https://user-images.githubusercontent.com/31085727/54730459-73386c80-4bcc-11e9-8fa6-c697c9865511.png)



- 저장소탭

  1. 다음과 같은 아이콘 선택

  2. 디스크 선택하기

     ![2943](https://user-images.githubusercontent.com/31085727/54730522-c3173380-4bcc-11e9-85ad-8e5b2cdeb48f.png)

  3. 추가 버튼 클릭

     ![2942](https://user-images.githubusercontent.com/31085727/54730562-ff4a9400-4bcc-11e9-8371-8111b0aac1de.png)

  4. 아까 설치했던 centOS iso 파일 선택

     ![2947](https://user-images.githubusercontent.com/31085727/54730605-27d28e00-4bcd-11e9-907b-d6f64405d302.png)

  5. 이미지가 삽인된것을 확인하고 Choose 선택

     ![2945](https://user-images.githubusercontent.com/31085727/54730631-43d62f80-4bcd-11e9-8286-fc94507c8275.png)



- 네트워크탭

  1. 어댑터에 브리지 선택

     ![2944](https://user-images.githubusercontent.com/31085727/54730647-5cdee080-4bcd-11e9-890b-1eee6d9de9cf.png)

     

  ## CentOS 실행 및 초기 설정

  1. `I` 키를 눌러서 Install CentOS 7 선택 `Enter`

     - Ctrl 버튼을 누른채로 클릭하면 가상 OS 를 빠져나올 수 있습니다.
     - 우측 하단에 나와있는 키 참고

     ![2949](https://user-images.githubusercontent.com/31085727/54730707-b0512e80-4bcd-11e9-80a5-3808a50d7250.png)

  2. 한국어 찾아서 선택

     ![2948](https://user-images.githubusercontent.com/31085727/54730857-759bc600-4bce-11e9-95be-7d12c6bd436e.png)

  3. 네트워크 및 호스트명 선택

     ![2950](https://user-images.githubusercontent.com/31085727/54730868-8ba98680-4bce-11e9-9240-26d4958aa7e9.png)

  4. 켬 버튼 스위치

     ![2952](https://user-images.githubusercontent.com/31085727/54730880-9f54ed00-4bce-11e9-8ee8-abb83d0d4a22.png)

  5. 설치 대상 클릭

     ![2953](https://user-images.githubusercontent.com/31085727/54730895-b8f63480-4bce-11e9-9e8e-00b5f2d5d51d.png)

  6. 완료 누르기

     ![2951](https://user-images.githubusercontent.com/31085727/54730904-cad7d780-4bce-11e9-89d6-b99175d4bf4c.png)

  7. 설치 완료

     ![2954](https://user-images.githubusercontent.com/31085727/54730930-e216c500-4bce-11e9-86f9-b76b1c3df81e.png)

  8. ROOT 암호 클릭

     ![2955](https://user-images.githubusercontent.com/31085727/54730959-fa86df80-4bce-11e9-96c0-062393124b8b.png)

  9. 비밀번호 설정 이후 완료 -> 설정 완료

     ![2956](https://user-images.githubusercontent.com/31085727/54730987-18ecdb00-4bcf-11e9-9c91-45c7a6cfe30f.png)

  10. 재부팅 선택

      ![2958](https://user-images.githubusercontent.com/31085727/54731026-5fdad080-4bcf-11e9-9104-eefecfdd8882.png)

  11. CentOS Linux (3.10.....) 7 (Core) 선택

      ![2957](https://user-images.githubusercontent.com/31085727/54731043-74b76400-4bcf-11e9-98bf-25d37dfce65b.png)

  12. root 의 아까 설정했던 비밀번호 설정

      ![2959](https://user-images.githubusercontent.com/31085727/54731064-8993f780-4bcf-11e9-9b98-115b58586751.png)





