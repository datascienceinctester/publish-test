# Iris classifier

Example readme markdown file.

* Bullet 1
    * Indented bullet

* Bullet 2

Uses the [iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) data set. Classifier trained in `scikit-learn`.

The breakdown of classes is shown in the table below.

| setosa | virginica | versicolor |
| :---:  | :---:     | :---:      |
| 50     | 50        | 50         |

## Function

```python
def iris_predict(data):
    # return predictions, transform to list so jsonify works
    # numpy arrays cannot be jsonified
    return clf.predict(data).tolist()
```

From [Wikipedia](https://en.wikipedia.org/wiki/Iris_flower_data_set):

> The Iris flower data set or Fisher's Iris data set is a multivariate data set introduced by Ronald Fisher in his 1936 paper The use of multiple measurements in taxonomic problems as an example of linear discriminant analysis.
