---
layout: post
title: Matploylib Tricks
category: Python
tags: Python
keywords: Python  Matplotlib
published: true
---

# Matploylib Tricks

{% raw %}

## Gantt Chart

```python
taskDict = {
    'task1': [3, 7],
    'task2': [5, 9],
    'task3': [1, 5],
    'task4': [12, 16]
}

import matplotlib.pyplot as plt
plt.rcParams['ytick.labelsize'] = 8

fig, ax = plt.subplots()

for i, t in enumerate(taskDict.keys()):
    xPair = taskDict[t]
    yPair = [i, i]
    ax.plot(xPair, yPair, color=[1, 0, 0, 0.5], linestyle='-', linewidth=3)


import numpy as np
ax.set_xticks(np.arange(24))  # show each of 24 hours
ax.set_yticks(np.arange(4))  # show each of 4 tasks
ax.set_yticklabels(taskDict.keys())

# Show grid line
ax.xaxis.grid(True, which='minor', linestyle='-', linewidth=0.25)
ax.grid(True, which='major', axis='x' )

# Maximize window
plt.get_current_fig_manager().window.showMaximized()
plt.tight_layout()
```

## Histogram

```python

def getColorByDay(d):
    # colPair = ['#009E73', '#E69F00']  # colorblind friendly
    colDict = [
        '#007DC6',  # Monday
        '#F47321',  
        '#FFC220',
        '#F47321',
        '#FFC220',
        '#004C91',
        '#007DC6',
    ]
    print(type(d))
    x = datetool.parse(d) if isinstance(d, str) else d
    return colDict[x.weekday()]


def getLabel(d):
    day = datetool.parse(d) if isinstance(d, str) else d
    return "{} {}".format(day.strftime("%A")[0:3], d[-8:])


def drawHistogram(df):    
    from matplotlib import pyplot as plt
    from matplotlib.ticker import AutoMinorLocator
    figManager = plt.get_current_fig_manager()
    figManager.window.showMaximized()

    plt.rcParams['ytick.labelsize'] = 8
    plt.rcParams['font.family'] = "monospace"
    
    ax = df['Volume'].plot(
        kind='barh', 
        color=df['day'].map(getColorByDay),
    )
    plt.gca().invert_yaxis()

    # change tick lable color
    tb2day_map = df[['TimeBucket', 'day']].set_index('TimeBucket').T.to_dict('list')
    for i in ax.get_yticklabels():
        d = tb2day_map[datetool.parse(i.get_text())][0]
        i.set_color(getColorByDay(d))
        # i.set_color(colPair[int(i.get_text()[8:10])%2])
        # i.set_backgroundcolor(colPair[int(i.get_text()[8:10])%2])
   
    # change tick lable string    
    labels = [getLabel(item.get_text()) for item in ax.get_yticklabels()]
    ax.set_yticklabels(labels)
    
    
    # Add grid lines
    ax.xaxis.set_minor_locator(AutoMinorLocator(5))
    ax.xaxis.grid(True, which='minor', linestyle='-', linewidth=0.25)
    ax.grid(True, which='major', axis='x' )
    #ax.grid('on', which='minor', axis='x', linestyle='.')
    plt.tight_layout()
```

{% endraw %}