

```python
from ipython_tools import handler
handler.style()
```


    ---------------------------------------------------------------------------

    ImportError                               Traceback (most recent call last)

    <ipython-input-1-4dbf13e8fcc2> in <module>()
    ----> 1 from ipython_tools import handler
          2 handler.style()


    /cvmfs/belle.cern.ch/ubuntu1604/releases/release-01-02-09/lib/Linux_x86_64/opt/ipython_tools/__init__.py in <module>()
    ----> 1 from hep_ipython_tools.ipython_handler_basf2 import handler
    

    /cvmfs/belle.cern.ch/ubuntu1604/releases/release-01-02-09/lib/Linux_x86_64/opt/hep_ipython_tools/ipython_handler_basf2/__init__.py in <module>()
          3 
          4 
    ----> 5 from hep_ipython_tools.ipython_handler_basf2.ipython_handler import Basf2IPythonHandler
          6 
          7 #: Create a single instance


    /cvmfs/belle.cern.ch/ubuntu1604/releases/release-01-02-09/lib/Linux_x86_64/opt/hep_ipython_tools/ipython_handler_basf2/ipython_handler.py in <module>()
    ----> 1 from hep_ipython_tools.ipython_handler_basf2.calculation import Basf2Calculation
          2 from hep_ipython_tools.ipython_handler_basf2.viewer import StylingWidget
          3 from hep_ipython_tools.ipython_handler import IPythonHandler
          4 from hep_ipython_tools.ipython_handler_basf2.information import Basf2ModulesInformation, Basf2EnvironmentInformation
          5 


    /cvmfs/belle.cern.ch/ubuntu1604/releases/release-01-02-09/lib/Linux_x86_64/opt/hep_ipython_tools/ipython_handler_basf2/calculation.py in <module>()
          1 from hep_ipython_tools.calculation import Calculation
          2 
    ----> 3 from hep_ipython_tools.ipython_handler_basf2.calculation_process import Basf2CalculationProcess
          4 from hep_ipython_tools.ipython_handler_basf2 import viewer
          5 


    /cvmfs/belle.cern.ch/ubuntu1604/releases/release-01-02-09/lib/Linux_x86_64/opt/hep_ipython_tools/ipython_handler_basf2/calculation_process.py in <module>()
    ----> 1 import basf2
          2 
          3 from hep_ipython_tools.ipython_handler_basf2 import python_modules
          4 from hep_ipython_tools.calculation_process import CalculationProcess
          5 from hep_ipython_tools.ipython_handler_basf2.entities import Basf2CalculationQueueStatistics


    /cvmfs/belle.cern.ch/ubuntu1604/releases/release-01-02-09/lib/Linux_x86_64/opt/basf2.py in <module>()
         19 import os
         20 import signal
    ---> 21 from pybasf2 import *
         22 # inspect is also used by LogPythonInterface. Do not remove!
         23 import inspect


    ImportError: /cvmfs/belle.cern.ch/ubuntu1604/externals/v01-05-02/Linux_x86_64/common/lib64/libcurl.so.4: symbol SSLv3_client_method version OPENSSL_1.0.0 not defined in file libssl.so.1.0.0 with link time reference


BASF2 Basics
========

If you run things at the command line (for example at KEKCC or NAF) you will need to set up your environment and whatever release.

```bash
. /cvmfs/belle.cern.ch/sl6/tools/setup_belle2
setuprel release-01-00-00
```

You **don't** need to do that in this ipython/jupyter environment (we've set it all up for you)


```python
import os
os.system('basf2 --info')
```




    32512



BASF2 modules
-------------

To find information about a basf2 module, run
```bash
basf2 -m
basf2 -m ParticleCombiner
```


```python
os.system('basf2 -m ParticleCombiner')
```




    32512



BASF2 variables
-------------

To check the list of basf2 variables known to the VariableManager, run
```bash
basf2 variables.py
```


```python
os.system('basf2 variables.py')
```




    32512



BASF2 modularAnalysis functions
-----------------------------

To check the list of functions you can call from modularAnalysis, run:
```bash
basf2 modularAnalysis.py
```


```python
os.system('basf2 modularAnalysis.py')
```




    32512



BASF2 other useful features
-------------------------

If you just execute `basf2` without any arguments, you will start an IPython session with many basf2 funcions imported.

In a terminal window, try:
```bash
basf2 
$ basf2
Python 3.6.2 (default, Oct 27 2017, 10:52:50)
Type "copyright", "credits" or "license" for more information.
IPython 5.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.
Welcome to BASF2 (Belle Analysis Software Framework 2)
basf2 in [1]: import pdg
basf2 in [2]: pdg.get(11)
basf2 out[2]: <ROOT.Belle2::EvtGenParticlePDG object ("e-") at 0x2327340>
basf2 in [3]: pdg.get(-11)
basf2 out[3]: <ROOT.Belle2::EvtGenParticlePDG object ("e+") at 
```


```python
import pdg
whatisthis = pdg.get(11)
print(whatisthis.GetName(), whatisthis.Mass())
```

You should also make use of IPython's built-in documentation features. As this jupyter session also runs with IPython we can showcase them here.


```python
import modularAnalysis 
modularAnalysis.inputMdst?
# the question mark brings up the function documentation
```


```python
print(dir(modularAnalysis)) # the python dir() function will also show you all functions' names
```
