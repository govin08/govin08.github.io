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

회사에서 제어를 위한 툴로서 강화학습과 MPC를 사용한다.
둘 모두 실시간 제어, 즉 온라인 방식을 구현해내는 것은 쉬운 일이 아니지만 이것들을 제한적으로 사용하거나 오프라인 방식으로 활용하는 방안은 충분히 생각해볼 법하다.

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
        
    def apply_voltage(self, voltage, noise_std=10.0):
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

![2-1_df]({{site.url}}\images\2026-03-05-simple_mpc\2-1_df.png){: .img-30-center}

위 코드에서 데이터 $\mathcal D$는 `df`로 표현되었다.
시간에 따른 속력과 전압을 나타내면 다음과 같이 나타난다.
전압이 높아지면 다음 순간에 속력이 높아짐을 확인할 수 있다.
(시인성을 위해 전압은 40을 곱하여 시각화했다.)

```
plt.figure(figsize=(15,3))
plt.plot(df['current_speed'], label='current_speed')
plt.plot(df['voltage']*40, label='voltage')
plt.legend()
plt.show()
```
![2-2_voltage-speed]({{site.url}}\images\2026-03-05-simple_mpc\2-2_voltage-speed.png){: .img-100-center}

# 3. 시스템 동역학 학습 (System Identification)

MPC를 위해 먼저 하는 것은 시스템 동역학을 학습하는 것이다.
즉

$$s_{t+1}=f(s_t,v_t)$$

인 함수 $f$를 모델링하는 것이다.

만약 $f$가 일차함수면 이 MPC를 linear MPC라고 부르고 $f$가 신경망처럼 비선형함수면 nonlinear MPC라고 부르게 된다.
지금 이 포스트에서는 linear한 경우를 다루도록 하자.
아까 생성한 데이터를 통해 선형식으로 모델링하면

```
def train_model(data):
    """데이터로 모델 학습"""
    print("모델 학습 중...")
    
    # X: [현재속도, 전압]
    X = data[['current_speed', 'voltage']].values
    # y: 다음속도
    y = data['next_speed'].values
    
    # 선형회귀로 학습
    model = LinearRegression()
    model.fit(X, y)
    
    print("✅ 학습 완료!")
    return model

model = train_model(df)

print(model.coef_.round(8), round(model.intercept_,8))
```

`[ 0.79428807 29.74423484] 5.89475244`

당연하게도 처음 가정한 시스템 동역학 식과 거의 일치하는 식이 도출된다.

$$
s_{t+1}=f(s_t,v_t)=0.79428807 s_t + 29.74423484 v_t+5.89475244
$$

이 모델은 데이터로부터 우리가 추론해낸 시스템 동역학이다.

# 4. MPC 제어 (one step)

이제 MPC의 핵심부분에 대해 알아보자.

목표변수의 목표값을 1500으로, horizon을 10으로 두어 one step에 대하여만 MPC 제어를 시행한다고 해보자.
이것은, 향후 10번의 timestep동안 목표변수의 예측값 $\hat s_1$, $\cdots$, $\hat s_{10}$이 최대한 1500에 가깝도록하려면 10번동안의 제어변수값 $\hat v_0$, $\cdots$, $\hat v_9$이 얼마여야 하는지 결정하는 것을 말한다.
목표변수의 예측값들은 벡터 $\hat s=\left(\hat s_1,\cdots,\hat s_{10}\right)$로 표현하고 제어변수값들도 벡터 $v=\left(v_0,\cdots,v_9\right)$로 표현하자.

목표변수값(속력)들은 동역학식 $f$을 순차적으로 적용하여 예측된 속도이다 ; $\hat s_0=s_0$, $\hat s_{t+1}=f(\hat s_t,v_t)$ $(t=1,\cdots,9)$.
이것은 함수 `predict_future`로 구현된다.

이렇게 얻어진 속도들을 사용하여 목표함수를 다음과 같이 정할 수 있으며

$$
\begin{align*}
J(v)
&=\sum_{t=1}^{10}\left(\hat s_t-1500\right)^2\\
&=\sum_{t=1}^{10}\left(f(\hat s_{t-1},v_{t-1})-1500\right)^2\\
\end{align*}
$$

이것은 함수 `cost`로 구현된다.
이 두 함수를 바탕으로 $J(v)$를 최소로 하는 $v$를 얻어낸다.
여기서 $J(v)$는 복잡할지언정 2차함수(quadratic function)에 속하므로 이 최적화 문제는 볼록 최적화(convex optimization)에 해당한다.
따라서 비교적 쉽게 풀리는 최적화 문제이다.
범용적이나 계산량이 많은 SLSQP로 이 문제를 풀 수도 있지만 여기서는 `scipy.optimize.minimize`의 기본 솔버인 L-BFGS-B를 사용하였다.

```
motor = Motor()

target_speed = 1500.0
current_speed = motor.speed

def predict_future(voltage_plan):
    """미래 10스텝 예측"""
    speed = current_speed
    speeds = []
    
    for v in voltage_plan:
        # 모델로 다음 속도 예측
        speed = model.predict([[speed, v]])[0]
        speeds.append(speed)
    
    return speeds

def cost(voltage_plan):
    """비용 함수: 목표와 얼마나 차이나는지"""
    future_speeds = predict_future(voltage_plan)
    error = sum((s - target_speed)**2 for s in future_speeds)
    return error

# 최적화: 10스텝 전압 계획
initial_plan = [5.0] * 10  # 초기값: 5V로 10번
bounds = [(0, 30)] * 10    # 전압 범위: 0~30V

result = minimize(cost, initial_plan, bounds=bounds)

print(result.x)
```

`
[30.         26.24554965 10.17598267 10.17576185 10.17595909 10.175793
 10.17593034 10.17558639 10.17605522 10.17605394]
`

그 결과로 얻어지는 $v^\ast=\text{argmin}_vJ(v)=(v^\ast_0,\cdots,v^\ast_9)$를 관찰하면

$$
\begin{align*}
v^\ast_0&\approx30\\
v^\ast_1&\approx26\\
v^\ast_2&\approx10\\
v^\ast_3&\approx10\\
&\vdots\\
v^\ast_9&\approx10\\
\end{align*}
$$

와 같다.
즉 처음에는 전압을 최대(30V)로 올려서 속도를 상승시킨 후 특정시점(t=2)부터는 전압을 10V로 주어 목표변수를 목표값에 수렴시킨다.

$v^\ast_2=v^\ast_3=\cdots=v^\ast_9\approx10$인 것은 동역학식

$$s_{t+1}=0.8s_t+30v_t$$

의 양변에 $\lim_{t\to\infty}$를 씌워서 정상상태의 $s$, $v$ 사이의 관계식을 다음과 같이 구했을 때

$$
\begin{align*}
\lim_{t\to\infty}s_{t+1}&=0.8\lim_{t\to\infty}s_t+30\lim_{t\to\infty}v_t\\
s&=0.8s+30v\\
0.2s&=30v\\
\end{align*}
$$

$s=1500$을 대입해 얻어지는 값 $v=10$과 정확히 일치한다.

이렇게, 향후 10번의 목표값을 맞추기 위한 전압값을 10개 추천했다.
하지만 horizon을 10으로 설정한 것은 단지 멀리 보기 위함이고 더 중요한 것은 당장 다음 스텝에서 얼마의 전압값(제어변수값)을 쓸 것이냐 하는 것이다 : $v^\ast_0=30$.

```
best_voltage = result.x[0]
print(best_voltage)
```

`30.0`

# 5. MPC 적용

## 5.1 Online MPC (50 step)

각 스텝 $t$마다 $s_t$가 주어졌을 때, $J(v)$를 최소화시키는 $v^\ast=(v^\ast_0,\cdots,v^\ast_9)$를 구한 후 그 중 첫번째 값 $v^\ast_0$만을 취하여 $v_t$로 둔다.
이후 모터의 동역학식을 적용하여 $s_{t+1}$을 구한다.
그런 식으로 50번의 스텝을 시행한 결과는 다음과 같다. 목표변수는 목표값에 수렴해가는 모습을 보인다.

```
motor = Motor()
results = []
target = 1500.0

for t in range(50):
    # 현재 속도
    speed = motor.speed

    # MPC로 전압 계산
    voltage = mpc_control(model, speed, target)

    # 모터에 적용
    new_speed = motor.apply_voltage(voltage)

    # 기록
    results.append({
        'time': t,
        'speed': new_speed,
        'target': target,
        'voltage': voltage
    })
results_df = pd.DataFrame(results)
results_df.to_csv('results.csv', index=False)

plt.figure(figsize=(10, 6))
plt.plot(results_df['time'], results_df['speed'], label='실제 속도', linewidth=2)
plt.plot(results_df['time'], results_df['target'], 'r--', label='목표 속도', linewidth=2)
plt.xlabel('시간 (초)')
plt.ylabel('속도 (RPM)')
plt.title('MPC 제어 결과 (50 step, receding horizon)')
plt.legend()
plt.grid(True, alpha=0.3)
```

![5-1_online_result]({{site.url}}\images\2026-03-05-simple_mpc\5-1_online_result.png){: .img-100-center}

위 코드에서 `apply_voltage`는 단순히 동역학 식을 넣어서 다음 스텝을 도출하는 구조였다.
만약 정말 실시간 제어를 한다면 `apply_voltage`는 '실제로' 전압을 가하고 다음 스텝의 속도를 '관찰'하는 형태가 되어야 할 것이다.

![5-1_diagram]({{site.url}}\images\2026-03-05-simple_mpc\5-1_diagram.png){: .img-50-center}

## 5.2 Offline MPC

위의 MPC 알고리즘은 $s_0$가 주어졌을 때 $s_1$, $s_2$, $\cdots$를 순차적으로 구해나가는 방식 (receding horizon)이다. 
하지만 실시간으로 제어하고 그 다음 스텝의 상태를 계측하는 것은 많은 경우에 기술적으로 가능하지 않을 수 있다.

실제로는 데이터셋만이 전체 시간에 대하여 주어져있는 경우가 많다.
실시간으로 제어하는 대신, 주어진 데이터 셋에 대하여 각 시점에 어떤 제어값을 취했어야 하는지를 사후적으로 추천하는 방식을 택할 수 있다.
이것을 나는 offline방식의 MPC라고 부르겠다.


```
df = df_duplicated.copy()
L = len(df)

# 기존 df에 컬럼 추가
df['recommended_voltage'] = 0.0
df['predicted_speed'] = 0.0

for i in range(L):
    current_speed = df.loc[i, 'current_speed']
    
    # MPC로 추천 voltage
    recommended_voltage = mpc_control(model, current_speed, target_speed)
    # print(i, recommended_voltage)
    
    # 모델 예측
    predicted_speed = model.predict([[current_speed, recommended_voltage]])[0]
    
    df.loc[i, 'recommended_voltage'] = recommended_voltage
    df.loc[i+1, 'predicted_speed'] = predicted_speed

# 플롯
plt.figure(figsize=(12, 5))

t = 100

plt.subplot(2, 1, 1)
plt.title('voltage')
plt.plot(df['voltage'], label='voltage')
plt.scatter(t, df.loc[t, 'voltage'], color='blue', s=30, zorder=5)
plt.plot(df['recommended_voltage'], label='recommended_voltage')
plt.scatter(t, df.loc[t, 'recommended_voltage'], color='orange', s=30, zorder=5)
plt.axvline(x=t, color='gray', linestyle='--', linewidth=1, alpha=0.7)  # ← 수직선

plt.legend()
plt.subplots_adjust(hspace=0.3)

plt.subplot(2, 1, 2)
plt.title('speed')
plt.plot(df['current_speed'], label='current_speed')
plt.scatter(t, df.loc[t, 'current_speed'], color='blue', s=30, zorder=5)
plt.plot(df['predicted_speed'], label='predicted_speed')
plt.scatter(t+1, df.loc[t + 1, 'predicted_speed'], color='orange', s=30, zorder=5)
plt.axvline(x=t, color='gray', linestyle='--', linewidth=1, alpha=0.7)  # ← 수직선
plt.legend()

plt.show()
```

![5-2_offline_result]({{site.url}}\images\2026-03-05-simple_mpc\5-2_offline_result.png){: .img-100-center}

전압범위는 0V에서 30V 사이로 해놨었다.
대부분의 범위에서 현재 속도(current speed)는 목표값(1500)보다 한참 미달하므로 추천 전압값은 최댓값인 30을 추천하는 경우가 잦다.
그리고 그렇게 추천한 결과 다음 시점의 속도(predicted speed)는 목표값과 유사한 값을 가지게 된다.
즉, '이 시점에 이 전압값을 가했다면 속도가 이만큼 목표값에 근접했을 것이다'라는 것을 표현하는 것이 위 그래프가 되겠다.

위 그래프에서 특정 시점 $t=100$에서 어떤 추천을 했는지도 보이고 있다.
현재 시점의 전압과 속도에 대하여 (파란 점 두 개), 현재시점의 전압을 얼마로 하면 (위의 노란 점) 다음 시점의 속도가 얼마가 될 것이다 (아래의 노란 점) 하는 것을 잘 나타내고 있다.
이것을 다이어그램으로 나타내면 다음과 같다.

![5-2_diagram]({{site.url}}\images\2026-03-05-simple_mpc\5-2_diagram.png){: .img-50-center}

이상의 두 MPC 적용방식을 표로 나타내면 아래와 같다.

![5_summary]({{site.url}}\images\2026-03-05-simple_mpc\5_summary.png){: .img-100-center}