# chefboost

[![Downloads](https://pepy.tech/badge/chefboost)](https://pepy.tech/project/chefboost)

**Chefboost** is a lightweight [gradient boosting](https://sefiks.com/2018/10/04/a-step-by-step-gradient-boosting-decision-tree-example/), [random forest](https://sefiks.com/2017/11/19/how-random-forests-can-keep-you-from-decision-tree/) and [adaboost](https://sefiks.com/2018/11/02/a-step-by-step-adaboost-example/) enabled decision tree framework including regular [ID3](https://sefiks.com/2017/11/20/a-step-by-step-id3-decision-tree-example/), [C4.5](https://sefiks.com/2018/05/13/a-step-by-step-c4-5-decision-tree-example/), [CART](https://sefiks.com/2018/08/27/a-step-by-step-cart-decision-tree-example/), [CHAID](https://sefiks.com/2020/03/18/a-step-by-step-chaid-decision-tree-example/) and [regression tree](https://sefiks.com/2018/08/28/a-step-by-step-regression-decision-tree-example/) algorithms **with categorical features support**. The library is lightweight. It just depends on pandas and numpy. You just need to write **a few lines of code** to build decision trees with Chefboost.

**Installation** - [`Demo`](https://youtu.be/YYF993HTHf8)

The easiest way to install Chefboost framework is to download it from [from PyPI](https://pypi.org/project/chefboost).
 
```
pip install chefboost
```

**Usage** - [`Demo`](https://youtu.be/Z93qE5eb6eg)

Basically, you just need to pass the dataset as pandas data frame and tree configurations after importing Chefboost as illustrated below. You just need to put the target label to the right. Besides, chefboost handles both numeric and nominal features and target values in contrast to its alternatives.

```python
from chefboost import Chefboost as chef
import pandas as pd

df = pd.read_csv("dataset/golf.txt")

config = {'algorithm': 'ID3'}
model = chef.fit(df, config)
```

**Outcomes**

Built decision trees are stored as python if statements in the `tests/outputs/rules` directory. A sample of decision rules is demonstrated below.

```python
def findDecision(Outlook, Temperature, Humidity, Wind, Decision):
   if Outlook == 'Rain':
      if Wind == 'Weak':
         return 'Yes'
      elif Wind == 'Strong':
         return 'No'
      else:
         return 'No'
   elif Outlook == 'Sunny':
      if Humidity == 'High':
         return 'No'
      elif Humidity == 'Normal':
         return 'Yes'
      else:
         return 'Yes'
   elif Outlook == 'Overcast':
      return 'Yes'
   else:
      return 'Yes'
 ```

**Testing for custom instances**

Decision rules will be stored in `outputs/rules/` folder when you build decision trees. You can run the built decision tree for new instances as illustrated below.

```python
prediction = chef.predict(model, param = ['Sunny', 'Hot', 'High', 'Weak'])
```

You can consume built decision trees directly as well. In this way, you can restore already built decision trees and skip learning steps, or apply [transfer learning](https://youtu.be/9hX8ir7_ZtA). Loaded trees offer you findDecision method to test for new instances.

```python
moduleName = "outputs/rules/rules" #this will load outputs/rules/rules.py
tree = chef.restoreTree(moduleName)
prediction = tree.findDecision(['Sunny', 'Hot', 'High', 'Weak'])
```

tests/global-unit-test.py will guide you how to build a different decision trees and make predictions.

**Model save and restoration**

You can save your trained models. This makes your model ready for transfer learning.

```python
chef.save_model(model, "model.pkl")
```

In this way, you can use the same model later to just make predictions. This skips the training steps. Restoration requires to store .py and .pkl files under `outputs/rules`.

```python
model = chef.load_model("model.pkl")
prediction = chef.predict(model, ['Sunny',85,85,'Weak'])
```

### Sample configurations

Chefboost supports several decision tree, bagging and boosting algorithms. You just need to pass the configuration to use different algorithms.

**Regular Decision Trees**

```python
config = {'algorithm': 'C4.5'} #Set algorithm to ID3, C4.5, CART, CHAID or Regression
```

The following regular decision tree algorithms are wrapped in the library.

| Algorithm | Tutorial | Demo |
| ---       | ---      | ---  |
| ID3       | [`Tutorial`](https://sefiks.com/2017/11/20/a-step-by-step-id3-decision-tree-example/) | [`Demo`](https://youtu.be/Z93qE5eb6eg) |
| C4.5      | [`Tutorial`](https://sefiks.com/2018/05/13/a-step-by-step-c4-5-decision-tree-example/) | [`Demo`](https://youtu.be/kjhQHmtDaAA) |
| CART      | [`Tutorial`](https://sefiks.com/2018/08/27/a-step-by-step-cart-decision-tree-example/) | [`Demo`](https://youtu.be/CSApBetgukM) |
| CHAID     | [`Tutorial`](https://sefiks.com/2020/03/18/a-step-by-step-chaid-decision-tree-example/) | [`Demo`](https://youtu.be/dcnFuS4QILg) |
| Regression | [`Tutorial`](https://sefiks.com/2018/08/28/a-step-by-step-regression-decision-tree-example/) | [`Demo`](https://youtu.be/pCQ2RCa20Bg) |

**Gradient Boosting** [`Tutorial`](https://sefiks.com/2018/10/04/a-step-by-step-gradient-boosting-decision-tree-example/), [`Demo`](https://youtu.be/KFsnZKMKNAE)

```python
config = {'enableGBM': True, 'epochs': 7, 'learning_rate': 1, 'max_depth': 5}
```

**Random Forest** [`Tutorial`](https://sefiks.com/2017/11/19/how-random-forests-can-keep-you-from-decision-tree/), [`Demo`](https://youtu.be/J7hDtV261PQ)

```python
config = {'enableRandomForest': True, 'num_of_trees': 5}
```

**Adaboost** [`Tutorial`](https://sefiks.com/2018/11/02/a-step-by-step-adaboost-example/), [`Demo`](https://youtu.be/Obj208F6e7k)

```python
config = {'enableAdaboost': True, 'num_of_weak_classifier': 4}
```

### Paralellism

Chefboost offers parallelism to speed model building up. Branches of a decision tree will be created in parallel in this way. You should set enableParallelism argument to True in the configuration. Its default value is False. It allocates half of the total number of cores in your environment if parallelism is enabled.

```python
if __name__ == '__main__':
   config = {'algorithm': 'C4.5', 'enableParallelism': True, 'num_cores': 2}
   model = chef.fit(df, config)
```

Notice that you have to locate training step in an if block and it should check you are in main.

**Feature Importance** - [`Video`](https://youtu.be/NFLQT6Ta4-k)

Decision trees are naturally interpretable and explainable algorithms. Decisions are clear made by a single tree. Still we need some extra layers to understand the built models. Besides, its advanced version random forest and GBM are hard to explain. Herein, [feature importance](https://sefiks.com/2020/04/06/feature-importance-in-decision-trees/) is one of the most common way to see the big picture.

```python
if __name__ == '__main__':
   config = {'algorithm': 'C4.5', 'enableParallelism': True}
   model = chef.fit(df, config)
   fi = chef.feature_importance()
   print(fi)
```

Parallelism must be enabled to retrieve feature importance. This returns feature importance values in the pandas data frame format.

| feature     | final_importance |
| ---         | ---              |
| Wind        | 0.609868         |
| Humidity    | 0.265105         |
| Temperature | 0.197528         |
| Outlook     | -0.072501        |

### E-Learning

This [playlist](https://www.youtube.com/playlist?list=PLsS_1RYmYQQHp_xZObt76dpacY543GrJD) guides you how to use Chefboost step by step for different algorithms. 

You can also find the tutorials about these core algorithms [here](https://sefiks.com/tag/decision-tree/). 

Besides, you can enroll this online course - [**Decision Trees for Machine Learning From Scratch**](https://www.udemy.com/course/decision-trees-for-machine-learning/?referralCode=FDC9B836EC6DAA1A663A) and follow the curriculum if you wonder the theory of decision trees and how this framework is developed.

### Support

There are many ways to support a project - starring⭐️ the GitHub repos is just one.

### Citation

Please cite chefboost in your publications if it helps your research. Here is an example BibTeX entry:

```BibTeX
@misc{serengil2019chefboost,
  abstract = {Lightweight Decision Trees Framework supporting Gradient Boosting (GBDT, GBRT, GBM), Random Forest and Adaboost w/categorical features support for Python},
  author={Serengil, Sefik Ilkin},
  title={chefboost},
  url={https://github.com/serengil/chefboost}
  year={2019}
}
```

### Licence

Chefboost is licensed under the MIT License - see [`LICENSE`](https://github.com/serengil/chefboost/blob/master/LICENSE) for more details.
