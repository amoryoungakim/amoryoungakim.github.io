딕셔너리 만들기
from collections import defaultdict
counter = defaultdict(int)
counter = defaultdict(lambda: 0)

counter.setdefault(letter, 0)

[key for m in [max(stats.values())] for key,val in stats.iteritems() if val == m]
['b', 'd']

import operator

dict = {"abcde" : 7, "fzowe" : 5, "fko" : 5}
sortedArr = sorted(dict.items(), key=operator.itemgetter(0))
key=operator.itemgetter(0) 는, 정렬의 키값을 0번째 인덱스 기준으로 하겠다는 것이다.

string_list = ['A','B','C']
dictionary = {string : 0 for string in string_list}
print(dictionary)

string_list = ['A','B','C']
dictionary = dict.fromkeys(string_list,0)
print(dictionary)

string_list = ['A','B','C']
int_list = [1, 2, 3]
dictionary = dict(zip(string_list, int_list))
print(dictionary)
