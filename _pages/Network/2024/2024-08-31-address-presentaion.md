---
title: "🌐[Network] 주소의 표현"
tags:
    - Network
date: "2024-08-31"
thumbnail: "/assets/img/thumbnail/network.jpeg"
---

# 🌐[Network] 주소의 표현.

- 시스템을 설계할 때는 기능이나 목적과 함께 고유의 구분자(Identifier)를 부여하는 방법에 대해서도 우선하여 고려해야 합니다.
- 일반적으로 주소의 개념은 단순히 서로를 구분한다는 고유 목적을 넘어서 주소가 가리키는 대상의 특징을 표현할 수 있습니다.
- 사람들은 문자로 된 이름에 익숙하지만, 0과 1로 디지털화된 환경에서는 구분자를 숫자로 된 주소로 표현할 수밖에 없습니다.
- 디지털 환경에서 숫자로 된 주소 표현 방식은 일반 사용자에게 불편하므로 보통은 외우기 쉬운 문자 형식의 이름을 추가로 사용합니다.
- 주소와 이름은 일대일(1:1) 관계가 이루어지며, 이들은 연결하는 기능이 필요합니다.
- 인터넷에서 일반 사용자는 문자로 된 이름을 사용하고, 인터넷 내부는 숫자로 된 주소를 사용하므로 둘 사이의 변환 기능이 필요합니다.
- 대상을 유일하게 구별하는 구분자는 일반적으로 다음의 네 가지 특징이 있습니다.

## 1️⃣ 유일성

- 구분자의 가장 중요한 역할은 대상을 서로 구분하여 지칭하는 것입니다.
    - 따라서 서로 다른 대상이 같은 구분자를 갖지 않는 유일성을 보장해야 합니다.
        - 그러나 이론적으로 완전한 확장성을 전제로 하는 유일성을 보장하기는 불가능합니다.
            - 예를 들어, 주민 번호에서 앞쪽 여섯 글자인 생년월일은 100년 이내에 출생한 사람들만 구분할 수 있습니다.
                - 현재는 방편적으로 바로 뒤의 남녀 구분 자리(1900년대 출생자는 1,2를 사용하고, 2000년대 출생자는 3,4를 사용함)를 활용하여 제한적인 확장성을 확보하고 있을 뿐 입니다.

## 2️⃣ 확장성

- 시스템은 시간이 흐르면서 이용자가 증가하는 보편화 과정이 진행되므로 자연스럽게 규모가 확장됩니다.
    - 따라서 사용하는 구분자의 양도 증가합니다.
- 시스템의 최대 수용 규모를 예측하여 구분자의 최대 한계를 올바르게 설정하지 않으면, 표현할 수 있는 공간의 크기가 제한되어 시스템의 확장성도 제한받게 됩니다.
    - 합리적인 기준을 설정하여 확장의 정도를 예측하고, 또한 그 이후에 대한 고려도 함께 이루어져야 합니다.
- 처음 인터넷을 설계했을 때 지금과 같은 규모로 인터넷을 이용하리라고는 예측하지 못했습니다.
    - 그 결과 인터넷 구분자인 IP 주소의 고갈 문제에 직면해 있습니다.

## 3️⃣ 편리성

- 시스템 설계 과정에서 부여되는 구분자는 시스템의 내부 처리 구조를 효율적으로 운용할 수 있도록 해주어야 합니다.
    - 컴퓨터 시스템은 내부적으로 숫자에 기반해 처리되기 때문에 구분자의 체계도 숫자 위주입니다.
        - 또한 배치, 검색 등을 원활하게 수행하기 위해 보통 일반인이 의미를 이해할 수 없는 형식을 갖습니다.
            - 이처럼 시스템 내부 동작에 종속된 구분자의 주소 체계는 사용자가 쉽게 이해하기 어려우므로 문자로 된 이름을 추가로 부여합니다.
                - 따라서 숫자로 된 주소와 문자로 된 이름을 모두 가지므로 이를 매핑(Mapping)하는 기능이 필요합니다.

## 4️⃣ 정보의 함축

- 구분자는 응용 환경에 필요한 다양한 정보를 포함하는 경우가 많습니다.
    - 예를 들어, 주민 번호는 생년월일, 성별 등을 알 수 있는 숫자로 구성되어 있습니다.
        - 집 주소도 광역시부터 시작해 지역을 소규모로 분할하는 구조로 되어 있어 집의 지리적인 위치를 쉽게 가늠할 수 있습니다.
            - 이처럼 분자는 응용 환경에 적절히 대응할 수 있는 부가 정보를 포함해야 합니다.