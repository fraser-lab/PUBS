###README - How to run hamming scripts?

1. The first script you want to run is the "fastq_index_parser_v4.py". This will take the fastq files from the different sequence runs as input - each at a time - (day and lane - in our case 3: Oct16-lane1, Oct16-lane2 and Oct17-lane3). More details about the options are explained in the first lines of this script. Here is an example of how to start the script: "python /data/ubcelss/scripts/fastq_index_parser_v4.py /data/ClassData-2014/data-oct16/lane2_Undetermined_L002_R1_001.fastq --indices /data/ubcelss/input_files/barcodes_e.txt -o /data/ubcelss/output_files/indices_oct16_lane2-all.pkl --index_cutoff 2 --const_cutoff 2". 
The output (pkl) will be as follows (for each day and lane): {TS1:{barcode:quality-score, ...}, TS2:{barcode:quality-score, ...}, ..., TS30:{barcode:quality-score, ...}}
2. For the next step you should run the "pkl_barcode_parser.py". This will take the pkl files  - each at a time - from the "fastq_index_parser_v4.py" script as input. More details about the options are explained in the first lines of this script. Here is an example of how to start the script: "python /data/ubcelss/scripts/pkl_barcode_parser.py /data/ubcelss/output_files/indices_oct16_lane2-all.pkl --out_pickle /data/ubcelss/output_files/indices_oct16_lane2-all-barcodes.pkl --allele_pickle /data/ubcelss/input_files/allele_dic_with_WT.pkl --fuzzy_cutoff 2".
The output (pkl) will be as follows (for each day and lane): {TS1:{barcode:number-of-reads, ...}, TS2:{barcode:number-of-reads, ...}, ..., TS30:{barcode:number-of-reads, ...}}
3. And finally you want to convert the previous dictionaries - all at once - (from "pkl_barcode_parser.py") into the requested order. This will result in one dictionary for each TS with barcodes sequence as key and the number of reads as values. The script is called "pickleread.py". More details about the options are explained in the first lines of this script. Here is an example of how to start the script: "python /data/ubcelss/scripts/pickleread.py /data/ubcelss/output_files/indices_oct16_lane1-all-barcodes.pkl /data/ubcelss/output_files/indices_oct16_lane2-all-barcodes.pkl /data/ubcelss/output_files/indices_oct17_lane1-all-barcodes.pkl --out_dir /data/ubcelss/output_files/ --pkl_basename merged-all-seq-days-TS_ --allele_pickle /data/ubcelss/input_files/allele_dic_with_WT.pkl". 
The output files (pkl) will be as follows (30 pkl files): TS1:{barcode:number-of-reads, ...}, TS2:{barcode:number-of-reads, ...}, ..., TS30:{barcode:number-of-reads, ...}

FYI: The seqmatch includes functions which will be called during the first two scripts.