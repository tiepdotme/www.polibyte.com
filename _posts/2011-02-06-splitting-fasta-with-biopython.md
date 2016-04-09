---
layout:     post
title:      Splitting fasta with biopython
---


A student in the lab associated with my employer asked me for advice on how to extract records from a FASTA file. The hitch was that he wanted a large number of records, on the order of thousands, and the FASTA file was even larger, containing tens of millions of records. The first approach that came to my mind was splitting the file into chunks that were small enough to fit into memory on
the nodes in our cluster. This would allow multiple CPUs to search for the records of interest while eliminating the greatest potential performance killer, lots of disk seeks. I'd never used Biopython before, so this request seemed like a good excuse to try it. It turned out to be remarkably easy to learn enough to accomplish what I wanted; going from idea to tested code took about an hour.

To split the data

	:::python
	import sys
	from Bio import SeqIO
	filename= sys.argv[1]
	maximum_length = int(sys.argv[2])
	input = open(filename, "rU")
	current_length = 0
	current_file = 0
	output = open("%s.%d" % (filename, current_file), "w")
	for record in SeqIO.parse(input, "fasta"):
	  if (current_length == maximum_length):
	    output.close()
	    current_length = 0
	    current_file = current_file + 1
	    output = open("%s.%d" % (filename, current_file), "w")
	  # SeqIO.write requires a list, so turn our record into one
	  SeqIO.write([record], output, "fasta")
	  current_length = current_length + 1
	input.close()
	output.close()


And then to find the records you care about

	:::python
	import sys
	from Bio import SeqIO
	fasta_filename = sys.argv[1]
	id_filename = sys.argv[2]
	output_filename = sys.argv[3]
	id_handle = open(id_filename, "rU")
	# read entire file into list, stripping newlines
	ids = [id.strip() for id in id_handle.readlines()]
	id_handle.close()
	fasta_handle = open(fasta_filename, "rU")
	records = SeqIO.to_dict(SeqIO.parse(fasta_handle, "fasta"))
	fasta_handle.close()
	output_handle = open(output_filename, "w")
	for id in ids:
	  if id in records:
	    SeqIO.write([records[id]], output_handle, "fasta")
	output_handle.close()


So assuming you have 40 million records in mydata.fasta, that file is 4 times too large to fit into memory, and the identifiers for
the records you care about are one per line in findthese.txt, you could run

split_data.py mydata.fasta 10000000

And then submit 4 jobs to the cluster

find_records.py mydata.fasta.0 findthese.txt results.fasta.0

find_records.py mydata.fasta.1 findthese.txt results.fasta.1

find_records.py mydata.fasta.2 findthese.txt results.fasta.2

find_records.py mydata.fasta.3 findthese.txt results.fasta.3





