# \[Appendix 7] Introduction on How to Use Conda

Anaconda is distribution software that provides a collection of packages written in Python and R programming languages in the scientific computing field (e.g., data science, machine-learning application programs, large-scale data processing, and predictive analytics).

The Anaconda distribution has over 12 million users and includes over 1,400 popular data science packages suitable for Windows, Linux, and MacOS.

To install Anaconda, you can download and install the distribution suitable for your OS from the website: [https://www.anaconda.com](https://www.anaconda.com/)

(Example) Windows, MacOS, Linux

Currently, Anaconda has a version based on Python 3.7 and one based on Python 2.7.

Conda is an application provided to manage package versions in Anaconda.

By using conda, the dependency problem with which Python users have the most difficulty when installing packages can be solved easily.

This document introduces the method for using the conda package in the KISTI system for Python users.

The "/home01/userID" directory on the introduction page is the home directory of the test account. Therefore, the directory should be changed to the path that is appropriate for a user.

## **1. Use of Conda**

\- For Miniconda, you can download a version suitable for each OS from the website: https://docs.conda.io/en/latest/miniconda.html

For Anaconda, you can download a version suitable for each OS from the website: https://www.anaconda.com/distribution/#download-section

|  Command  |                                                                                 Description                                                                                |
| :-------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|   clean   |                                                                     Remove unused packages and caches.                                                                     |
|   config  | <p>Modify configuration values in .condarc. This is modeled after the git config command.</p><p>Writes to the user .condarc file (/home01/userID/.condarc) by default.</p> |
|   create  |                                                      Create a new conda environment from a list of specified packages.                                                     |
|    help   |                                                     Display a list of available conda commands and their help strings.                                                     |
|    info   |                                                           Display information about the current conda installed.                                                           |
|    init   |                                                           Initialize conda for shell interaction. \[Experimental]                                                          |
|  install  |                                                       Install a list of packages into a specified conda environment.                                                       |
|    list   |                                                                List linked packages in a conda environment.                                                                |
|  package  |                                                               Low-level conda package utility. (EXPERIMENTAL)                                                              |
|   remove  |                                                        Remove a list of packages from a specified conda environment.                                                       |
| uninstall |                                                                           Alias for conda remove.                                                                          |
|    run    |                                                          Run an executable in a conda environment. \[Experimental]                                                         |
|   search  |     <p>Search for packages and display associated information.<br>The input is a MatchSpec, which is a query language for conda packages.</p><p>See examples below.</p>    |
|   update  |                                                          Updates conda packages to the latest compatible version.                                                          |
|  upgrade  |                                                                           Alias for conda update.                                                                          |

## **2. Create Conda Environment**

\- The conda environment creates an independent virtual execution environment for Python, so different versions of packages can be easily managed.

\- You can create a conda environment using "conda create -n \[ENVIRONMENT]."

\- The conda environment is created with the environment name specified in the path under envs of the conda path by default.

\- If the "--use-local" option is used, the conda environment is created in the user’s home directory (**${HOME}/.conda/envs/\[environment\_name]**).

\- Example -

```
$ module load python/3.7
 $ conda create -n scikit-learn_0.21 --use-local
 Collecting package metadata: done
 Solving environment: done
 
 ## Package Plan ##
 
   environment location: /home01/userID/.conda/envs/scikit-learn_0.21
 
 Proceed ([y]/n)? y
 
 Preparing transaction: done
 Verifying transaction: done
 Executing transaction: done
 #
 # To activate this environment, use:
 # > conda activate scikit-learn_0.21
 #
 # To deactivate an active environment, use:
 # > conda deactivate
 #
 
 $ source activate scikit-learn_0.21
 (scikit-learn_0.21) $  
```

## **3. Install and Check Packages in Conda Environment**

\- Packages can be installed using conda install \[package name].

\- Packages in the conda channel can be installed using "conda install -c \[channel name] \[package name]".

\- Packages are installed under the conda environment path created in Section "2" above.

\- Example -

```
$ module load python/3.7
 $ source activate scikit-learn_0.21
 (scikit-learn_0.21) $ conda install scikit-learn
 Collecting package metadata: done
 Solving environment: done
 
 ## Package Plan ##
 
   environment location: /home01/userID/.conda/envs/scikit-learn_0.21
   added / updated specs:
     - scikit-learn
 
 The following packages will be downloaded:
     package                    |            build
     ---------------------------|-----------------
     ca-certificates-2019.1.23  |                0         126 KB
     ...
       wheel-0.33.1               |           py37_0          39 KB
     ------------------------------------------------------------
                                            Total:       277.6 MB
 
 The following NEW packages will be INSTALLED:
 
   blas               pkgs/main/linux-64::blas-1.0-mkl
  ...
    zlib               pkgs/main/linux-64::zlib-1.2.11-h7b6447c_3
 
 Proceed ([y]/n)? y
 
 Downloading and extracting packages
 setuptools-40.8.0    | 643 KB    | ##################################### | 100% 
 ...
 openssl-1.1.1b       | 4.0 MB    | ##################################### | 100% 
 Preparing transaction: done
 Verifying transaction: done
 Executing transaction: done
 (scikit-learn_0.21) $ python -c "import sklearn"
 (scikit-learn_0.21) $
```

## **4. Check Conda Environment List**

\- You can check the conda environment list using "conda-env list" or "conda env list."

\-Example-

```
(scikit-learn_0.21) $ conda env list
 # conda environments:
 #
 base                     /apps/applications/PYTHON/3.7
 scikit-learn_0.21     *  /home01/userID/.conda/envs/scikit-learn_0.21
 (scikit-learn_0.21) $ conda deactivate
 $
```

## **5. Remove Conda Environment**

\- You can remove a conda environment using "conda-env remove -n \[ENVIRONMENT]" or "conda env remove -n \[ENVIRONMENT]."

\-Example-

```
$ module load python/3.7
$ conda env remove -n scikit-learn_0.21
Remove all packages in environment /home01/userID/.conda/envs/scikit-learn_0.21:
$
```



## **6. Export Conda Environment**

\- The conda-pack package is required before exporting a conda environment.

(Reference) [https://conda.github.io/conda-pack](https://conda.github.io/conda-pack)

\- You can use the conda environment in another system by using "conda pack -n \[ENVIRONMENT] -o \[file name]."

(Example) When the external Internet is not connected, when the same conda environment is used in another system

\-Example-

```
$ module load python/3.7
 $ source activate tensorflow_1.12
 (tensorflow_1.12) $ conda install -c conda-forge -n tensorflow_1.12 conda-pack
 (tensorflow_1.12) $ conda pack -n tensorflow_1.12 -o conda_tensorflow_1.12.tar.gz
 Collecting packages...
 Packing environment at '/home01/userID/.conda/envs/tensorflow_1.12' to 'conda_tensorflow_1.12.tar.gz'
 [########################################] | 100% Completed |  4min 18.8s
 (tensorflow_1.12) $ ls -l conda_tensorflow_1.12.tar.gz
 -rw-------. 1 userID in0162 1459826406 Mar 28 15:03 conda_tensorflow_1.12.tar.gz
 (tensorflow_1.12) $
```

## **7. Import Conda Environment**

\- You can import the conda environment that was created using conda pack, as shown in the example below, and use it after setting the environment.

\-Example-

```
$ module load python/3.7
 $ mkdir -p $HOME/.conda/envs/tensorflow_1.12
 $ tar xvzf conda_tensorflow_1.12.tar.gz -C $HOME/.conda/envs/tensorflow_1.12
 $ source activate tensorflow_1.12
 (tensorflow_1.12) $ conda-unpack
 (tensorflow_1.12) $ conda deactivate
 $
```

{% hint style="info" %}
2022년 2월 15일에 마지막으로 업데이트되었습니다.
{% endhint %}
