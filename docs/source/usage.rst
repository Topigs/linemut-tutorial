Usage
=====

.. _installation:

Installation
------------

To use LineMut, first clone the public code repository:

.. code-block:: console

   $ git clone https://gitee.com/sunfengjie/linemut.git

Create a conda environment that contains the dependencies
required to run LineMut.

.. code-block:: console

   $ cd linemut/
   $ conda create -n linemut -f ./conda_env.txt 

LineMut also depends on GATK, so please ensure that GATK is 
already installed. You can verify the installation using the 
following command:

.. code-block:: console

   $ gatk --version 

Add the cloned local directory to the PATH (optional):

.. code-block:: console

   $ echo PATH=$(pwd):'${PATH}' >> ~/.bashrc 
   $ source ~/.bashrc
