# Issues in MAT
## 1
***
First I meet a problem: 
```
Setting up PyTorch plugin "bias_act_plugin"... D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py:287: UserWarning: Error checking compiler version for cl: 
'utf-8' codec can't decode byte 0xd3 in position 0: invalid continuation byte
  warnings.warn('Error checking compiler version for {}: {}'.format(compiler, error))
Failed!
D:\PythonProject\MAT\torch_utils\ops\bias_act.py:50: UserWarning: Failed to build CUDA kernels for bias_act. Falling back to slow reference implementation. Details:

Traceback (most recent call last):
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\setuptools\msvc.py", line 175, in _msvc14_get_vc_env
    stderr=subprocess.STDOUT,
  File "D:\PolyU\Anaconda3\envs\MAT\lib\subprocess.py", line 389, in check_output
    **kwargs).stdout
  File "D:\PolyU\Anaconda3\envs\MAT\lib\subprocess.py", line 481, in run
    output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command 'cmd /u /c "d:\Program Files (x86)\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsall.bat" x86_amd64 && set' returned non-ze
ro exit status 255.

The above exception was the direct cause of the following exception:
```
## 2
***
Note: The version of CUDA Toolkit's Visual Studio Integration must match the Visual Studio's version. e.g. Visual Studio Integration 11.0 CANNOT match Visual Studio 2022.
