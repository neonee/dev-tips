# Bloom Filter

- Bloom 필터라고 해서 Bloom에 뭔가 의미가 있나 했는데 그냥 만든 사람의 이름에서 따온 것이었..
- 확률(론)적 자료구조(probabilistic data structure)라고 해서 굉장히 거창하고 어려운 줄 알았는데 다행이 아니었..
- 결론부터 말하면
  >- 어떤 값이 어떤 집합 안에
  >- 확실히 없거나
  >- 있을 수도 있다(없는 것을 있다고 잘못 판정(false positive)할 수 있다. 이 부분 때문에 probabilistic data structure라는 거창한 이름이 붙은 듯)를
  >- 굉장히 작은 메모리 공간만 사용하면서도
  >- 매우 빠르게 판정할 수 있는 자료 구조
- 이건 [설명보다 눈으로 보는 게 훨씬 낫다](https://llimllib.github.io/bloomfilter-tutorial/)
  - 문자열을 몇 개 입력하다보면 왜 블룸 필터가 없다고 판정한 것은 확실이 없고, 있다고 판장한 것은 없을 수도 있는지 금방 감이 올 것이다.
  - 결국 해시 충돌을 그냥 방치하기 때문.
  - 비트열(bit array)만큼의 메모리 공간만 사용하며 오차율(실제로는 집합 안에 없는데 있다고 판정(false positive)할 확률)이 블룸 필터에 넣고자 하는 입력값의 갯수, 해시함수의 갯수, 비트열의 크기에 의해 정해진다.
  - 해시 함수를 몇 개를 사용해야 하는지, 블룸 필터의 크기를 얼마로 정해야 하는지도 위 링크에 잘 나와있다.
- 해시 함수를 사용한다는 관점에서 비슷한 점이 있는 해시테이블과의 차이점
  - 해시테이블은 해시 충돌에 대한 처리를 하므로
    - 해시테이블이 있다고 판정하면 반드시 있고, 없다고 판정하면 반드시 없다.
    - 해시테이블에서 특정 데이터를 제거해도 판정 결과에 영향을 미치지 않는다.
    - 대신에 블룸 필터에 비해 메모리 공간을 훨씬 더 많이 차지하고, 메모리 공간이 충분하지 않아 해시 충돌 발생이 잦으면 최악의 경우 원소 갯수인 O(n)이지만 현실적으로 메모리 공간을 적절히 사용한다면 실질적으로 O(1).
  - 블룸 필터는 해시 충돌에 대한 처리를 하지 않으므로
    - 없다는 확실하지만 있다는 불확실하다(여러 번 반복해서 강조하는 데 없는 걸 있다고 판정할 수 있다).
    - 블룸 필터에서 특정 데이터를 제거(비트열에서 해당 데이터의 해시 결과에 해당하는 위치의 비트값을 1에서 0으로 변경)하면 판정 결과가 크게 달라지므로 실질적으로 데이터를 제거할 수 없다.
    - 대신에 해시테이블에 비해 메모리 공간을 훨씬 덜 사용하며, 해시 충돌 처리를 하지 않으므로 최악의 경우에도 해시 함수의 갯수(k)인 O(k). 현실적으로 원소 갯수 n에 비해 k가 현저히 작으므로 실질적으로 O(1)이라고 볼 수 있다.
