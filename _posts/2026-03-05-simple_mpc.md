---
layout: single
title: "Simple MPC"
categories: data-science
tags: [model predictive control]
use_math: true
published: true
author_profile: false
toc: true
---

MPC에 대해 글을 써볼까.

# 0.

회사에서 제어를 위한 툴로서 강화학습과 MPC를 사용한다.
둘 모두 실시간 제어 즉 온라인 방식을 구현해내는 것은 쉬운 일이 아니지만 이것들을 제한적으로 사용하거나 오프라인 방식으로 활용하는 방안은 충분히 생각해볼 법하다.

작년 말에, 나의 실친이자 직장 선배인 K 책임님이 MPC 기능을 구현해놓았다.
나는 MPC가 무엇인지 공부할 엄두가 나지 않아 계속 미루고 있다가 최근에야 클로드와의 대화를 통해 MPC를 이해하게 되었다.
지극히 간단한 예제로 MPC를 코드로 구현해놓았으니 나는 이걸 simple MPC라고 부르겠다.
목적함수를 만들 때에도 군더더기 없이 MPC의 핵심 term만 남겨서 서술하려고 한다.
이것을 MPC라고 불러도 되는지는 잘 모르겠으나 어쨌든 MPC의 원리를 이해하기 위한 입문용으로는 괜찮지 않을까 하는 생각으로 글을 쓴다.

# 1. 모터 환경

모터에 전압을 가해 차의 속력을 변화시키는 문제를 생각하자.
$s_t$를 $t$시점의 속력이라고 하고(speed) $v_t$를 $t$시점에서의 전압이라고 하자(voltage).
$s_t$는 상태변수이자 목표변수의 역할을 하고 $v_t$는 제어변수가 된다.
$t$는 이산적으로, 등간격의 시간만을 생각하자.
다시 말해, 일종의 decision process를 생각하는 것이다.
시스템 동역학은

$$s_{t+1}=0.8s_t+30v_t$$

로 주어진다고 하자.
즉, 속력은 이전 속력에 의한 관성의 영향을 받고 거기에 모터에 걸려진 전압에 비례하게 속도가 상승한다.
이것은 선형적인 동역학이다.

이 모터 환경은 다음과 같은 `Motor` 클래스로 구현된다.

```
class Motor:
    """간단한 모터 시뮬레이터"""
    def __init__(self):
        self.speed = 0.0
        
    def apply_voltage(self, voltage, noise_std=1.0):
        """전압을 주면 속도가 변함"""
        # 간단한 식: 새속도 = 현재속도 * 0.8 + 전압 * 30 + noise
        noise = np.random.normal(0, noise_std)
        self.speed = self.speed + voltage * 30 - self.speed * 0.2 + noise
        return self.speed
```

# 2. 데이터 생성

이 시스템 동역학은 알려져 있지 않다고 가정하자.
대신 다음과 같은 데이터가 주어져있다고 하자.

$$
\mathcal D = \{(s_t,v_t,s_{t+1}):0\le t\lt200\}
$$

실제로는 $0\le t\lt 200$인 $t$에 대하여 전압 $v_t$를 0과 10 사이의 임의의 실수로 부여하는 식으로 가상의 데이터를 생성했다 ($s_0=0$)

```
motor = Motor()
data = []

for i in range(200):
    # 현재 속도
    current_speed = motor.speed
    
    # 랜덤 전압 (0~10V)
    voltage = np.random.uniform(0, 10)
    # 모터에 전압 주기
    next_speed = motor.apply_voltage(voltage)
    
    # 기록
    data.append({
        'current_speed': current_speed,
        'voltage': voltage,
        'next_speed': next_speed
    })
df = pd.DataFrame(data).round(3)
df
```

![2_df]({{site.url}}\images\2026-03-05-simple_mpc\2_df.png){: .img-50-center}


위 코드에서 데이터 $\mathcal D$는 `df`로 표현되었다.