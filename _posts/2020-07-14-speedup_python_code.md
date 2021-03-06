---
title: Ways to speed up a python program
layout: post
use_toc: true
excerpt: Ways to speed up a python program
---
## 1. Before starting any optimizations identify the most problematic parts of Code 

### Python debugging performance
- [scikit-learn recommendations](https://scikit-learn.org/stable/developers/performance.html)
- [Blog which collects useful tools](https://pythonspeed.com/articles/beyond-cprofile/)
- [PyInstrument](https://github.com/joerick/pyinstrument/)      

  ```python
  from pyinstrument import Profiler

  profiler = Profiler()
  profiler.start()

  tm = TMI()
  tm.fit(tree_100_0, tree_100_1)
  profiler.stop()

  print(profiler.output_text(unicode=True, color=True))
  ```    

- [cProfiler](https://ipython-books.github.io/42-profiling-your-code-easily-with-cprofile-and-ipython/)     

  ```
  %%prun -s tottime -q -l 20 -T prun0

  print(open('prun0', 'r').read())
  ```    
  
The %prun and %%prun magic commands accept multiple optional options (type %prun? for more details). 
In the example, 
-s allows us to sort the report by a particular column, -q to suppress (quell) the pager output (which is useful when we want to integrate the output in a notebook), 
-l to limit the number of lines displayed or to filter the results by function name (which is useful when we are interested in a particular function), 
and -T to save the report in a text file. In addition, we can choose to save (dump) 
the binary report in a file with -D, or to return it in IPython with -r. 
This database-like object contains all information about the profiling and can be analyzed through Python's pstats module.

## 2. Cython
- [Online Course from oreilly](https://learning.oreilly.com/videos/learning-cython/)
- [Summary of hot to use Cython](https://byumcl.bitbucket.io/bootcamp2014/_downloads/AllWeek7.pdf)
- Sample code of usage from [scikit-learn ](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/metrics/cluster/_expected_mutual_info_fast.pyx)
- Before starting make sure that `sudo apt-get install g++`
- Use magic method in jupyter nobooks for fast prototyping. Flag -a means to give analysis which shows parts which may be optimized `%%cython -a`
- In order to compile the file and later on import it in your code use: `python setup.py build_ext --inplace`
- [OpenMP Cython Jupyter notebook example](https://homes.cs.washington.edu/~jmschr/lectures/Parallel_Processing_in_Python.html)


## 3. Multithreading in Python
- [Use shared recources instead of copying](https://research.wmz.ninja/articles/2018/03/on-sharing-large-arrays-when-using-pythons-multiprocessing.html)
- There are can be several `Threads` inside one `Process`. `Threads` share resources between each other, while `Process` can be executed in `parallel`: without any sharing.    
![image](https://user-images.githubusercontent.com/13698885/87418526-1b4c2300-c5d2-11ea-8b6b-8940428d4ff2.png)
- In the multiprocessing module, the `Pool` class is mainly used to implement a pool of processes, each of which will carry out tasks submitted to a Pool object. Generally, the Pool class is more convenient than the `Process` class, especially if the results returned from your concurrent application should be ordered.
`Pool` class: the `Pool.map()` and `Pool.apply()` methods follow the convention of Python's traditional `map()` and `apply()` methods, ensuring that the returned values are ordered in the same way that the input is. These methods, however, block the main program until a process has finished processing. The Pool class, therefore, also has the `map_async()` and `apply_async()` functions to better assist concurrency and parallelism.
- Common issues [1](https://stackoverflow.com/questions/13068760/parallelise-python-loop-with-numpy-arrays-and-shared-memory),[2](https://stackoverflow.com/questions/11368486/openmp-and-python)


