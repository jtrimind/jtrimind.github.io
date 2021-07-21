---

title:  "pandas.factorize"
---

## pandas.factorize
object를 enumerated type이나 categorical variable로 변환한다.
참고 : [pandas.factorize](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.factorize.html)

## 예시

```python
>>> codes, uniques = pd.factorize(['b', 'b', 'a', 'c', 'b'])
>>> codes
# output : array([0, 0, 1, 2, 0])
>>> uniques
# output : array(['b', 'a', 'c'], dtype=object)
```
['b', 'b', 'a', 'c', 'b']를 factorize하면 [0, 0, 1, 2, 0]과 ['b', 'a', 'c']가 반환된다.

```python
# before
# |'Sex'   | 'Embarked' |
# |--------|------------|
# | male   | S          |
# | female | C          |
for f in ['Sex', 'Embarked']:
    train[f] = (train[f].factorize())[0]
# after
# |'Sex'   | 'Embarked' |
# |--------|------------|
# | 0      | 0          |
# | 1      | 1          |
```
'Sex', 'Embarked' column을 categorical variable로 변환한다.
