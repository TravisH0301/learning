# Pandas Parallelism via Modin

## Table of Contents
- [Modin](#modin)
- [Dependancy](#dependancy)
- [Installation](#installation)
- [Import](#import)
- [Trouble shooting](#trouble-shooting)
- [Source](#source)

## Modin
Modin provides parallelism for Pandas operations. And it provides easy integration with no configuration required. Additionally, it follows Pandas syntax.

Modin uses matrix systems for its dataframe architecture for flexibility and scalability.

<img src="https://modin.readthedocs.io/en/latest/_images/block_partitions_diagram.png" width="400">

## Dependancy
Modin uses either Ray or Dask engine to operate. In this instruction, Dask engine will be used.<br>
Dask engine requires msgpack < 1.0.

## Installation
Modin with Dask engine can be installed as:<br>
`$ pip install modin[dask]`

msgpack can be installed as:<br>
`$ pip install msgpack==0.6`

## Import 
Modin on Dask engine can be imported as:<br>
`import os`<br>
`os.environ["MODIN_ENGINE"] = "dask"`<br>
`import modin.pandas as pd`

Warning module can be used to hide warnings.<br>
`import warnings`<br>
`warnings.filterwarnings("ignore")`

## Trouble shooting
New version of Modin may clash with dependencies such as Pandas. <br>
If so, it is recommended to downgrade Modin and install dependencies with the respective versions. <br>
ex. modin\[dask]==0.10.0 & pandas==1.2.4

## Source
[Modin Documentation](https://modin.readthedocs.io/en/latest/index.html)
