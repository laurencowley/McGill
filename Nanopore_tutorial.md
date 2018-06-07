# Nanopore tutorial

We will be using the Galaxy server that you have been using this week so far so please open that up.

## Data we will use (Download from dropbox):

* ```EM_079517.fasta```
* ```illumina_assembly_graph.fastg```
* ```O55_combined_miniasm.gfa```
* ```sakai_prophages.fa```

## Programs we will use off galaxy:

* [Tablet](https://ics.hutton.ac.uk/tablet/) 
* [Bandage](http://rrwick.github.io/Bandage/)

#### First off, we are going to start by using some Ebola data to look at mapping nanopore reads to a reference. We are going to use BWA mem that has long read capability.

# Step 1: Fastq-dump
First we need to retrieve Ebola data from NCBI. 

* We will use the ```Get Data``` tab on the left hand side panel of Galaxy, under ```Tools```. 
* Click on ```Download and Extract Reads in FASTA/Q```
* Options will now be visible in the middle panel of galaxy
* The input type should be ```SRR accession```
* In Accession enter ```ERR1014225``` (This is the accession number for nanopore reads of an Ebola sample from the West African outbreak)
* Select ```gzip compressed fastq``` for the output format
* Click ```Execute```

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%209.52.33%20AM.png)

* The job will then appear in the ```History``` panel on the right hand side of the screen

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.07.13%20AM.png)

* The job will turn green when it has finished

# Step 2: BWA-MEM
Next we will map our nanopore Ebola reads to a reference.

* We will use the ```NGS:Mapping``` tab on the left hand side panel of Galaxy, under ```Tools```. 
* Click on ```Map with BWA-MEM```
* Options will now be visible in the middle panel of galaxy
* Select ```Use a genome from history and build index```
* The reference sequence will be preloaded into your galaxy history: ```EM_079517.fasta``` (This is a finished Ebola reference genome for strain Zaire)
* For Algorithm select ```Auto. Let BWA decide the best algorithm to use```
* Select ```Single``` for type of reads
* Select ```ERR1014225 (fastq-dump)``` for fastq dataset
* Select ```Do not set``` for read groups information
* Select ```3. Nanopore 2D-reads mode (-x ont2d)``` for analysis mode  
* Click ```Execute```

![Screenshot](screenshots/Screen%20Shot%202018-06-07%20at%2010.07.13%20AM.png)

### When it has finished running, we will download the mapped reads.

* Click on the finished job (Called ```Map with BWA-MEM```etc.) in the ```History``` panel on the right hand side panel 
* You will see a save icon under the details of the job
* Click on it and the ```Download dataset``` and ```Download bam_index``` options will appear
* We need both so click on one after the other 

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.32.52%20AM.png)

# Step 3: Viewing the mapping in Tablet

* We will load the downloaded data into Tablet by opening the Tablet program and clicking on ```Open Assembly``` in the top left hand side of the screen
* For ```Primary assembly file or URL:``` click on ```Browse...``` and find the downloaded Bam file (you need the file with the extension ```.bam``` not ```.bai```)
* For ```Reference/consensus file or URL:``` click on ```Browse...``` and find the downloaded (from dropbox) reference genome ```EM_079517.fasta```

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.42.02%20AM.png)

* Click ```Open``` and then click on the Contig ```EM_079517``` on the left hand panel of the screen and you will see your mapping results ready to browse
* Take a few minutes to explore the program and the results

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2010.44.03%20AM.png)

#### What can you tell from the alignment about the method of sequencing?

#### Does the data look different to Illumina data that you might have seen?

#### Can you find anything that looks like consistent errors?

#### Can you find anything that looks like a reliable SNP? 

# Step 4: Comparing Illumina and Nanopore assemblies

#### Next, we're going to use *Escherichia coli* to illustrate how much assemblies can be improved with long reads

## Pre-knowledge
 * Pathogenic *E. coli* tends to have a lot repeat sequences
 * This is due to large prophages integrating in the genome
 * These prophages are very similar to each other and cause confusion for assembly algorithms 
 * These prophages can be as large as ~50kb
 * No matter how deep you sequence, if you don't get reads longer than those repeats you will never finish the genome
 * See [this paper](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2013-14-9-r101) for more detail

## Bandage

	
	Nodes = contigs
	Edges = overlaps

* Open up Bandage from your desktop
* Click on ```File``` then ```Load graph```
* Find the ```illumina_assembly_graph.fastg``` location on your computer and click ```Open```

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2011.20.11%20AM.png)
 
 * Click ```Draw graph``` on the left hand panel of the screen
 * You will see that the Illumina assembly produces a whole mess of 649 contigs with a lot of short contigs connecting everything together
 * You can zoom in on the graph to look at it in more detail
 
![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.40.28%20PM.png)

 * We can colour the graph by depth by changing ```Graph display``` from ```Random colours``` to ```Colour by depth```
 * You'll see that the short contigs all have much higher coverage than the other contigs. *What do you think that means?*

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.47.48%20PM.png)

#### Now we are going to use [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) to look for the prophage regions in the assembly

* Click on ```Create/view BLAST search``` in the left hand panel of the screen
* ```Step 1``` click on ```Build BLAST database```
* ```Step 2``` click on ```Load from FASTA file``` and find the ```sakai_prophages.fa``` location on your computer
* ```Step 3``` click on ```Run BLAST search```
* click on ```Close```

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.52.56%20PM.png)

#### You will see the prophage regions highlighted in colour on the assembly graph now.

#### What affect are the prophage regions having on the assembly?

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.54.03%20PM.png)

#### Now, we will load the nanopore assembly into Bandage to see how long reads affect this

* Click on ```File``` then ```Load graph```
* Find the ```O55_combined_miniasm.gfa``` location on your computer and click ```Open```
* Click ```Draw graph``` on the left hand panel of the screen
* Change ```Graph display``` option from ```BLAST hits(solid)``` to ```Random colours```. *What is strikingly different about this assembly than the Illumina one?*

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.58.17%20PM.png)

### Now, lets look at where the prophage regions are in this assembly

* Click on ```Create/view BLAST search``` in the left hand panel of the screen
* ```Step 1``` click on ```Build BLAST database```
* ```Step 2``` click on ```Load from FASTA file``` and find the ```sakai_prophages.fa``` location on your computer
* ```Step 3``` click on ```Run BLAST search```
* click on ```Close```

![Alt text](screenshots/Screen%20Shot%202018-06-07%20at%2012.59.42%20PM.png)

#### How have the long reads improved this assembly?

#### Are there any regions that haven't been resolved by the nanopore experiment? Why would that be?