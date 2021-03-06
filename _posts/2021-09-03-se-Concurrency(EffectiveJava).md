---
layout: post
title: 5.Concurrency (Effective Java) 
subtitle: 
categories: [SoftwareEngineering]
tags: [ComputerScience]
---

#### 66.공유하는 가변 데이터에 접근시 동기화 하자

많은 프로그래머들이 동기화를 상호 배타 (하나의 스레드가 객체를 변경하는 동안 다른 스레드에서 불안정 상태의 그 객체를 볼 수 없도록 하는) 수단으로만 생각한다. 이런 관점에서 볼때 객체는 안정상태에서 생성되고, 그 객체를 접근하는 메소드들에 의해 락이 걸린다. 이 메소드들은 그 객체의 상태를 보면서 필요시 상태 전이 (state transition - 객체가 안정상태에서 다른 안정상태로 변화)을 일으킨다.

동기화를 올바르게 사용하며 불안정한 상태의 갹체를 어떤 메소드에서도 접근할 수 없도록 보장한다.

동기화를 하지 않으면, 하나의 스레드에서 변경한 내용은 다른 스레드에서 못 볼 수 있다.

**따라서 동기화는 불안정 상태의 객체를 스레드가 볼 수 없도록 하는것은 물론, 동기화된 메소드는 블록에 진입하는 각 스레드가 앞에서의 모든 변경 (같은 락으로 보호되었던)이 반영된 결과를 볼 수 있게 해준다.**
- 동기화는 상호배타는 물론 스레드간의 신뢰성 있는 변수 값 전달에도 필요한 것이다.
- 공유하는 가변 데이터로의 접근을 동기화하는데 실패하면, 설사 그 데이터를 원자적으로 (atomically) 읽거나 쓸 수 있더라도 결과는 비참해질 수 있다.
   - 원자적 연산 : 더 이상 쪼갤 수 없는 가장 최소 단위의 연산으로 한번에 수행될 수 있는것을 원자성이라고 한다. 따라서 원자성 연산 중에는 다른것이 끼어들 수 없다.

메소드 동기화에는 값을 변경하는 메소드와 읽는 메소드 모두 동기화되어야 한다. 그래야 동기화 효과가 생긴다!

**Volatile**
상호배타를 수행하지는 않지만, 그 필드의 값을 읽는 스레드에서는 가장 최근에 변경된 값을 볼 수 있다.
하지만 정말 주의해서 사용해야하는데, 안전 실패 (safety failure - 잘못된 결과를 계산)를 유발할 수 있다.

즉, 여러 스레드가 가변 데이터를 공유할때, 그 데이터를 읽거나 쓰는 각 스레드에서는 반드시 동기화를 해야한다. 동기화를 하지 않으면, 한 스레드가 변경한 것을 다른 스레드가 볼 수 있다는 보장이 없다.

공유되는 가변 데이터를 동기화하는데 실패하면 liveness failure 혹은 safety failure가 생길 수 있다. 이는 간헐적으로 발생할 수 있고, 어떤 VM을 사용하는지에 따라서 동작이 달라질 수 있다.

만일 스레드간의 확실한 변수 값 전달만이 필요하고 상호배타 처리를 할 필요가 없다면 volatile 변경자를 사용하는것도 좋다. 하지만 올바르게 사용하려면 잘 알아야 한다.

#### 67.지나친 동기화는 피하자

상황에 따라 다르겠지만, 지나친 동기화는 성능을 저하시키고 교착상태를 유발시키며 심지어는 예기치 않던 행동을 초래할 수 있다.

liveness failure 혹은 safety failure가 생기지 않게 하려면 동기화된 메소드나 블록 안에서 절대로 클라이언트에게 제어권을 넘기면 안된다.
즉, 오버라이딩된 메소드를 호출하지 않아야하며, 함수 객체의 형태로 클라이언트가 제공하는 메소드도 호출하지 말아야한다.

동기화 영역을 갖는 클래스의 관점에서 그런 메소드들은 외계인 (alien) 처럼 이질적이다.

따라서 일반적으로 동기화된 블록 내부에서는 가능한 적은 양의 일을 처리해야 한다.


#### 70.일반적인 thread safe 수준 
￼
![1.1](/assets/images/se/5.1.png)

![1.1](/assets/images/se/5.2.png)￼












