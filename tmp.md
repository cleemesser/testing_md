[//]: # ('cell_type':'markdown', "metadata":{'slideshow': {'slide_type': 'slide'}})
## Introduction to visualizing data in the eeghdf files

```python
# %load explore-eeghdf-files-basics.py
# Here is an example of how to do basic exploration of what is in the eeghdf file. I show how to discover the fields in the file and to plot them.
# 
# I have copied the stacklineplot from my python-edf/examples code to help with display. Maybe I will put this as a helper or put it out as a utility package to make it easier to install.

from __future__ import print_function, division, unicode_literals
# %matplotlib inline
%matplotlib notebook

import matplotlib
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import h5py
from pprint import pprint

import stacklineplot


# matplotlib.rcParams['figure.figsize'] = (18.0, 12.0)
matplotlib.rcParams['figure.figsize'] = (12.0, 8.0)
```

```python
hdf = h5py.File('./archive/YA2741G2_1-1+.eeghdf')
```

```python
pprint(list(hdf.items()))
pprint(list(hdf['patient'].attrs.items()))
```

```python
rec = hdf['record-0']
pprint(list(rec.items()))
pprint(list(rec.attrs.items()))
```

```python
signals = rec['signals']
labels = rec['signal_labels']
electrode_labels = [str(s,'ascii') for s in labels]
numbered_electrode_labels = ["%d:%s" % (ii, str(labels[ii], 'ascii')) for ii in range(len(labels))]
```

[//]: # ('cell_type':'markdown', "metadata":{'slideshow': {'slide_type': 'slide'}})
#### Simple visualization of EEG (brief absence seizure)

```python
# plot 10s epochs (multiples in DE)
ch0, ch1 = (0,19)
DE = 2 # how many 10s epochs to display
epoch = 147; ptepoch = 10*int(rec.attrs['sample_frequency'])
stacklineplot.stackplot(signals[ch0:ch1,epoch*ptepoch:(epoch+DE)*ptepoch],seconds=DE*10.0, ylabels=electrode_labels[ch0:ch1])
print("epoch:", epoch)

```

```python
annot = rec['edf_annotations']
#print(list(annot.items()))
#annot['texts'][:]
```

```python
antext = [s.decode('utf-8') for s in annot['texts'][:]]
starts100ns = [xx for xx in annot['starts_100ns'][:]]
len(starts100ns), len(antext)
```

```python
# age in years 10yo, #1 sz at 1476
```

```python
import pandas as pd
```

```python
df = pd.DataFrame(data=antext, columns=['text'])
df['starts100ns'] = starts100ns
df['starts_sec'] = df['starts100ns']/10**7
```

```python
df
```

```python
df[df.text.str.contains('sz',case=False)]
```

```python
list(annot.items())
```

```python

```
