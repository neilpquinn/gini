# gini
##Overview
Calculate the Gini coefficient of a numpy array. Gini coefficients are often used to quantify income inequality, read more [here](http://www.statsdirect.com/help/default.htm#nonparametric_methods/gini.htm).

The function in ```gini.py``` is based on the third equation from [here](http://www.statsdirect.com/help/default.htm#nonparametric_methods/gini.htm), which defines the Gini coefficient as:

![G = \dfrac{ \sum_{i=1}^{n} (2i - n - 1) x_i}{n  \sum_{i=1}^{n} x_i}](https://github.com/oliviaguest/gini/raw/master/gini.png "Gini equation")


##Examples
For a very unequal sample, 999 zeros and a single one:
```
>>> from gini import *
>>> a = np.zeros((1000))
>>> a[0] = 1.0
```

The Gini coefficient is very close to 1.0:
```
>>> gini(a)
0.99890010998900103
```

For uniformly distributed random numbers, it will be low, around 0.33:
```
>>> s = np.random.uniform(-1,0,1000)
>>> gini(s)
0.3295183767105907
```

##Code
The code itself is very self-explanatory with respect to what it allows. The Gini calculation itself, requires non-zero positive sorted values within a 1d vector. This is dealt with within ```gini()```. So these four assumptions can be violated, as they are corrected for within the function:
```python
def gini(array):
    """Calculate the Gini coefficient of a numpy array."""
    # based on bottom eq: http://www.statsdirect.com/help/content/image/stat0206_wmf.gif
    # from: http://www.statsdirect.com/help/default.htm#nonparametric_methods/gini.htm
    array = array.flatten() #all values are treated equally, arrays must be 1d
    if np.amin(array) < 0:
        array -= np.amin(array) #values cannot be negative
    array += 0.0000001 #values cannot be 0
    array = np.sort(array) #values must be sorted
    index = np.arange(1,array.shape[0]+1) #index per array element
    n = array.shape[0]#number of array elements
    return ((np.sum((2 * index - n  - 1) * array)) / (n * np.sum(array))) #Gini coefficient
```

##Notes
It is faster than [pysal.inequality.gini](http://pysal.readthedocs.io/en/latest/_modules/pysal/inequality/gini.html) and answers are indistinguishable before approximately 6 decimal places (i.e., they are the same arithmetically for all intents and purposes).
Other Gini coefficient functions found online do not produce equivalent results, hence why I wrote this.
