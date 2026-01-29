LineMut Call
============

.. _tutorial:

Check Installation
------------------

You can verify whether **LineMut** has been successfully installed by running the
following command:

.. code-block:: bash

   which linemut_call

If LineMut is correctly installed, this command should return the full path to
the ``linemut_call`` executable.

After installation, you can inspect the available commands and options using:

.. code-block:: bash

   linemut_call --help


Command-Line Interface
----------------------

Below is the full help message provided by ``linemut_call``:

.. code-block:: text

   Usage:
       linemut_call [OPTIONS]


   Description:
       linemut_call is designed for detecting expressed single-nucleotide variants
       from single-cell RNA-sequencing or spatial transcriptomics data.


   Options:
       --bam, -I:
           raw BAM file.
       --ref, -R:
           The reference genome sequence file in FASTA format.
       --output, -O:
           Directory for saving the result.
       --barcode-celltype-mapping, -m:
           The CSV file containing cell barcodes and their corresponding cell types.

       --cells-coordinate, -c:
           (optional) The CSV file containing cell coordinate information. If this
           parameter is not provided, the default behavior is to use cell type as
           the unit for mutation detection without CMB partitioning.
       --known-variants-dir, -v:
           (optional) A directory of VCF-formatted known variant sites for the species.
       --k-mer, -k:
           (optional) The length of k-mer, default: 9.
       --cell-barcode-tag, -t:
           (optional) The tag name denoting the cell barcode in the BAM file
           defaults to 'CB'.
       --python:
           (optional) The pathname to the Python interpreter you want to use.
           By default, it uses the first 'python3' found in the PATH.
       --gatk:
           (optional) The pathname of the GATK executable file. By default,
           search for 'gatk' in the PATH.
       --samtools:
           (optional) The pathname of the samtools. By default, search for the
           'samtools' in the PATH.

       --help, -h:
           Print this message, exit and return a non-zero exit status.


Running LineMut Call via a Bash Script
--------------------------------------

To run LineMut, specify all required parameters and optionally include additional
arguments depending on your analysis needs. It is recommended to write the command
into a Bash script for reproducibility.

An example script is shown below:

.. code-block:: bash

   raw_bam='/path/to/bam'
   ref='/path/to/genome/fasta'
   output='/path/to/result/'
   barcode_celltype_mapping='/path/to/barcode/celltype/map/file'
   cells_coor='/path/to/cells/coordinate/file'
   k=kmer-length

   linemut_call \
       -I $raw_bam -O $output -m $barcode_celltype_mapping \
       -c $cells_coor -R $ref -k $k \
       --python /path/to/python \
       --gatk /python/to/gatk \
       --samtools /path/to/samtools


Parameter Notes
---------------

- ``-O`` specifies the output directory.  
  **This directory should not be created manually**; LineMut will generate it
  automatically during execution.

- ``-m`` specifies a CSV file mapping cell barcodes to cell types.
  Each line in the file should follow the format::

     <cell barcode>,<cell type>

- ``-k`` defines the k-mer length used for partitioning CellMetaBins (CMBs).


Runtime Logs
------------

After launching the script, you should see log messages similar to the following
printed to the console:

.. code-block:: text

   2026-01-08 17:24:05 INFO: [linemut_call] bam=/path/to/bam
   2026-01-08 17:24:05 INFO: [linemut_call] output=/path/to/store/result/files
   2026-01-08 17:24:05 INFO: [linemut_call] barcode-celltype-mapping=/path/to/barcode/celltype/map/file
   2026-01-08 17:24:06 INFO: [linemut_call] cells-coordinate=/path/to/cells/coordinate/file
   2026-01-08 17:24:06 INFO: [linemut_call] ref=/path/to/genome/fasta
   2026-01-08 17:24:06 INFO: [linemut_call] k-mer=kmer-length
   2026-01-08 17:24:06 INFO: [linemut_call] python=/path/to/python
   2026-01-08 17:24:06 INFO: [linemut_call] gatk=/path/to/gatk
   2026-01-08 17:24:06 INFO: [linemut_call] samtools=/path/to/samtools
   2026-01-08 17:24:06 INFO: [linemut_call] Step 1: bam filtering and cell type-based demultiplexing
   ...
   2026-01-08 19:24:43 INFO: [linemut_call] Step 8: finished


Output Directory Structure
--------------------------

After the pipeline completes successfully, the output directory will contain a
structure similar to the following:

.. code-block:: text

    /path/to/result/
    ├── 1.celltype_bams10x_cmb/
    ├── 1.celltype_bams
    │   ├── Epidermis.bam
    │   ├── Epidermis.bam.bai
    │   ├── Inner_callus.bam
    │   ├── Inner_callus.bam.bai
    │   ├── Outgrowth_shoot.bam
    │   ├── Outgrowth_shoot.bam.bai
    │   ├── Shoot_primordia.bam
    │   ├── Shoot_primordia.bam.bai
    │   ├── Vascular_tissue.bam
    │   └── Vascular_tissue.bam.bai
    ├── 2.cellmetabin_result
    │   ├── 27mer_cmbs
    │   └── cmb_bams
    ├── 3.freebayes_vcfs
    │   ├── Epidermis_cmb_0.vcf
    │   ├── Epidermis_cmb_1.vcf
    │   ├── Epidermis_cmb_2.vcf
    │   ├── Epidermis_cmb_3.vcf
    │   ├── Inner_callus_cmb_0.vcf
    │   ├── Inner_callus_cmb_1.vcf
    │   ├── Inner_callus_cmb_2.vcf
    │   ├── Inner_callus_cmb_3.vcf
    │   ├── Outgrowth_shoot_cmb_0.vcf
    │   ├── Outgrowth_shoot_cmb_1.vcf
    │   ├── Outgrowth_shoot_cmb_2.vcf
    │   ├── Outgrowth_shoot_cmb_3.vcf
    │   ├── Outgrowth_shoot_cmb_4.vcf
    │   ├── Shoot_primordia_cmb_0.vcf
    │   ├── Shoot_primordia_cmb_1.vcf
    │   ├── Shoot_primordia_cmb_2.vcf
    │   ├── Shoot_primordia_cmb_3.vcf
    │   ├── Shoot_primordia_cmb_4.vcf
    │   ├── Shoot_primordia_cmb_5.vcf
    │   ├── Vascular_tissue_cmb_0.vcf
    │   ├── Vascular_tissue_cmb_1.vcf
    │   ├── Vascular_tissue_cmb_2.vcf
    │   └── Vascular_tissue_cmb_3.vcf
    ├── 4.strelka_vcfs
    │   ├── Epidermis_cmb_0
    │   ├── Epidermis_cmb_1
    │   ├── Epidermis_cmb_2
    │   ├── Epidermis_cmb_3
    │   ├── Inner_callus_cmb_0
    │   ├── Inner_callus_cmb_1
    │   ├── Inner_callus_cmb_2
    │   ├── Inner_callus_cmb_3
    │   ├── Outgrowth_shoot_cmb_0
    │   ├── Outgrowth_shoot_cmb_1
    │   ├── Outgrowth_shoot_cmb_2
    │   ├── Outgrowth_shoot_cmb_3
    │   ├── Outgrowth_shoot_cmb_4
    │   ├── Shoot_primordia_cmb_0
    │   ├── Shoot_primordia_cmb_1
    │   ├── Shoot_primordia_cmb_2
    │   ├── Shoot_primordia_cmb_3
    │   ├── Shoot_primordia_cmb_4
    │   ├── Shoot_primordia_cmb_5
    │   ├── Vascular_tissue_cmb_0
    │   ├── Vascular_tissue_cmb_1
    │   ├── Vascular_tissue_cmb_2
    │   └── Vascular_tissue_cmb_3
    ├── 5.integrated_vars
    │   ├── trusted
    │   └── untrusted
    ├── 6.gatk_result
    │   ├── bqsr
    │   ├── filtered_vcfs
    │   ├── logs
    │   ├── raw_vcfs
    │   └── split_ncigar_bams
    ├── 7.categorized_vars
    │   ├── trusted
    │   └── untrusted
    └── 8.h5ad_files
        ├── sc_depth.h5ad
        ├── sc_ratio.h5ad
        ├── unit_depth.h5ad
        └── unit_ratio.h5ad