Tutorial
========

.. _tutorial:

linemut call
------------

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


Running LineMut via a Bash Script
---------------------------------

To run LineMut, specify all required parameters and optionally include additional
arguments depending on your analysis needs. It is recommended to write the command
into a Bash script for reproducibility.

An example script is shown below:

.. code-block:: bash

   inemut='/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/linemut_rerun_generate_h5ad/linemut_call'

   raw_bam='/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/bam/callus_CNS0818518_10x/outs/possorted_genome_bam.bam'
   ref='/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/refgenome/tomato_spaceranger/fasta/genome.fa'
   output='/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/linemut_results/10x_cmb'
   barcode_celltype_mapping='/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/10x_cells_celltype.csv'
   cells_coor='/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/10x_cells_coor.csv'

   /usr/bin/time -v $linemut \
       -I $raw_bam -O $output -m $barcode_celltype_mapping \
       -c $cells_coor -R $ref -k 27 \
       --python /jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Tools/miniconda3/envs/multi-gatk/bin/python \
       --gatk /jdfsbjcas1/ST_BJ/PUB/User/sunfengjie/gatk-4.4.0.0/gatk \
       --samtools /jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Tools/miniconda3/envs/general-tools/bin/samtools


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

   2026-01-08 17:24:05 INFO: [linemut_call] bam=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/bam/callus_CNS0818518_10x/outs/possorted_genome_bam.bam
   2026-01-08 17:24:05 INFO: [linemut_call] output=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/linemut_results/10x_cmb
   2026-01-08 17:24:05 INFO: [linemut_call] barcode-celltype-mapping=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/10x_cells_celltype.csv
   2026-01-08 17:24:06 INFO: [linemut_call] cells-coordinate=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/data/tomato/10x_cells_coor.csv
   2026-01-08 17:24:06 INFO: [linemut_call] ref=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Projects/LineMut/refgenome/tomato_spaceranger/fasta/genome.fa
   2026-01-08 17:24:06 INFO: [linemut_call] k-mer=27
   2026-01-08 17:24:06 INFO: [linemut_call] python=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Tools/miniconda3/envs/multi-gatk/bin/python
   2026-01-08 17:24:06 INFO: [linemut_call] gatk=/jdfsbjcas1/ST_BJ/PUB/User/sunfengjie/gatk-4.4.0.0/gatk
   2026-01-08 17:24:06 INFO: [linemut_call] samtools=/jdfsbjcas1/ST_BJ/P21Z28400N0234/sunfengjie/Tools/miniconda3/envs/general-tools/bin/samtools
   2026-01-08 17:24:06 INFO: [linemut_call] Step 1: bam filtering and cell type-based demultiplexing
   ...
   2026-01-08 19:24:43 INFO: [linemut_call] Step 8: finished


Output Directory Structure
--------------------------

After the pipeline completes successfully, the output directory will contain a
structure similar to the following:

.. code-block:: text

   10x_cmb/
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
   ├── 5.integrated_vars
   ├── 6.gatk_result
   ├── 7.categorized_vars
   └── 8.h5ad_files