---
layout: post
title:  "Pandas : compare - Checking differences between DataFrames"
tag: DataAnalysis
---

When comparing DataFrames, [compare](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.compare.html) is here to help.

Imagine you have two different methods and they have the same output structure and you want to check the differences in results by comparing tables.

Lets create a dataframe 1:


```
import pandas as pd
import numpy as np

# Lets create two dataframes

df1 = pd.DataFrame(np.array([[101, 102, 103], 
	[201, 202, 203], 
	[301, 302, 303]]), columns=['Value1', 'Value2', 'Value3'], index=['A1',"A2","A3"])

df1

	Value1	Value2	Value3
A1	101	102	103
A2	201	202	203
A3	301	302	303

```

and dataframe 2:

```
df2 = pd.DataFrame(np.array([[101, 102.5, 103], 
	[201, 202, 203], 
	[301, 304, 303]]), columns=['Value1', 'Value2', 'Value3'],index=['A1',"A2","A3"])

df2

	Value1	Value2	Value3
A1	101.0	102.5	103.0
A2	201.0	202.0	203.0
A3	301.0	304.0	303.0


```

Now, lets use compare:

```
df1.compare(df2)

		Value2
	self	other
A1	102.0	102.5
A3	302.0	304.0
```

We can see that only in Value2, A1 and A3 are different, being **self** = df1 and **other** = df2.

It is also possible to make use of **keep_equal** and **keep_shape** arguments to change the way results are displayed:

```
keep_shape - "If true, all rows and columns are kept. Otherwise, only the ones with different values are kept."

keep_equal - "If true, the result keeps values that are equal. Otherwise, equal values are shown as NaNs."


df1.compare(df2,keep_equal=True,keep_shape=True)

     Value1	     Value2	    Value3
   self	other  self	other  self	other
A1	101	101.0	102	102.5	103	103.0
A2	201	201.0	202	202.0	203	203.0
A3	301	301.0	302	304.0	303	303.0


df1.compare(df2,keep_shape=True,keep_equal=False)

	 Value1	    Value2	     Value3
   self	other self	other	self other
A1	NaN	NaN	102.0	102.5	NaN	NaN
A2	NaN	NaN	NaN	NaN	NaN	NaN
A3	NaN	NaN	302.0	304.0	NaN	NaN

```


