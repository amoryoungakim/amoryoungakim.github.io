BPR: Bayesian Personalized Ranking from Implicit Feedback

특정 User에게 보여줄 Item의 Ranking을 정해주는 알고리즘

그 User가 아직 듣지 않은 노래를 어떻게 생각할지 모르지만, 한 가지 확실한 것은 내가 ITZY의 노래를 들었고 임영웅의 노래를 한 번도 안 들었다면,
나는 임영웅보다는 ITZY를 선호하는 것이 확실하다고 가정한다. 다른 알고리즘들은 interaction 있었냐 없었냐를 주로 보는데 BPR은 User, known item(i), unknown item(j)
이렇게 한 쌍이다. 이 개념을 수식으로 표현하면

user u의 item i에 대한 score(x̂ui)가 item j에 대한 score(x̂uj) 보다 항상 높다. 빼면 0보다 크다.

posterior probability(사후확률)가 사전확률 X likelyhood에 비례한다는 것을 이해해야 함.

