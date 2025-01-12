# SnarlPy 
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/felixk1990/SnarlPy/HEAD)
##  Introduction
The module 'snarlpy' is a python packages encompassing a set of class and
method implementations for 'networkx' graphs, in order to calculate linking
numbers of spatially intertwined networks and optimal cuts to
topologically unlink them. Used and explained in the publication: **arXiv:2208.11662**
<br>

##  Installation
Via PyPi
```
pip install snarlpy
```
##  Usage
For theory and algorithm details see publication: **arXiv:2208.11662**
Let's create an intertwined, spatial network with the package's internal
generator (here two ladders) and calculate the linking number matrices 'lk_mat'
(cycle space) and 'p'(edge space). When we have done so compute the the priority
matrices and plot the matrices diagnonal values onto the respective graphs'
edges (visualising the important edges for intertwinedness).
```
import numpy as np
import snarlpy.edgePriority as spe
import snarlpy.tangledGenerators as tg

def update_priorityPlot(D, p):

    colors = []
    priority_mat = [np.dot(p,p.T), np.dot(p.T,p)]

    for i, k in enumerate(D.layer):
        k.edges['priority'] = np.round( np.diagonal(priority_mat[i]),10)
        colors.append(k.edges['priority'])
    kwargs = {
        'color_nodes': ['#030512','#030512'],
        'color_edges': colors,
        'colormap': ['BuGn','RdPu'],
        'axis': True
    }
    fig = D.plot_circuit('priority', **kwargs)
    fig.show()

    return fig

num_periods = 2
D = tg.createLabelCatenation(num_periods)
graph_sets = [k.G for k in D.layer]
p, lk_mat = spe.getEdgeLinkageOperator(graph_sets)
fig = update_priorityPlot(D, p)
fig.show()
```
![network](https://raw.githubusercontent.com/felixk1990/network-linkage//main/gallery/main/dualLadderShift_0.png)
![prio1](https://raw.githubusercontent.com/felixk1990/network-linkage/main/gallery/main/lambdaSQ_10.png)
![prio2](https://raw.githubusercontent.com/felixk1990/network-linkage/main//gallery/main/lambdaSQ_20.png)

The package also allows you to directly compute cut sets, to find the best way
to topologically disentangle the networks. Calling this bit of code will
repeatedly compute the priority matrices for both networks and remove edges of
highest priority one by one (as displayed on the carton below).
```
import spe.edgePriority as spe
import tangledGenerators as tg

num_periods = 2
D = tg.createLabelHexagonHopfed_V4()
graph_sets = [k.G for k in D.layer]
init_cut_sets = [graph_sets[:], graph_sets[::-1]]
cut_lists = []
for ics in init_cut_sets:
    cut_lists.append(spe.cuttingEdgeAlgorithm(*ics))
```
![cuts](https://raw.githubusercontent.com/felixk1990/network-linkage//main/gallery/main/cuttingEdgeAlgorithm.png)
Further examples on the usage and percularities of edge priorities and network linkage can be found in the notebook example included.
##  Requirements
```
kirchhoff==0.2.7
matplotlib==3.7.1
networkx==2.8.4
numpy==1.23.5
pandas==1.5.3
plotly==5.9.0
scipy==1.10.1
```

## Acknowledgement
```snarlpy``` written by Felix Kramer
