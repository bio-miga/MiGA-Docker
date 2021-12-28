# Create A Genome Project

Genome projects are collections of genomes that do not have to be closely related. They are created by submitting reference genomes. They can serve as references against which to compare query genomes.

For this exercise I suggest that you download several genomes of the same genus from NCBI or use the _Pseudomonas_ genomes available at[ https//github.com/jfq3/data\_sets](https://github.com/jfq3/data_sets). The genomes comprising a genome project do not have to be from the same genus; the reason for this requirement in this exercise is that we will use this genome project as a reference for classifying a query genome in the **Classify a Genome** exercise.

Open MiGA-Web in your browser and log in if necessary. Click on **Create new project**.

On the **New project** page, enter a short name for your project in the **Path** box. Then select **Genomes** as the **Type** of project you want to run.

Under the section **Identity Engines**, the recommended settings are already filled in. You may change these to make your project run more quickly, but with some loss of sensitivity.

If you wish, you may add a name, description, and comments for your project in the **Optional information** section.

Scroll down to the bottom of the page and click on the blue **Create new MiGA project** button.

After creating the project, you are taken to a page for uploading datasets. Click on **Upload reference datasets**. This will take you to another page for uploading genome sequences.

The **Type of dataset** is **Genome**, and the **Type of input** is **Assembly in FastA format**. Click on **Choose Files** and select all but one of the genomes you are using. The un-selected genome will serve as the query genome in the **Classify a Genome** exercise. Scroll down to the bottom of the page and click on the blue button **Upload new data set**.

To start processing, you must turn on the daemon. Click on the green website icon in the upper right, select **Admin console**, **Control and review daemons**, and turn on the daemon for your project.

To monitor the progress of your project, click on **Projects** in the title bar, and then **Public.** Click on the name of your project. The menu in the middle of the screen gives the results for your project. When you click on **Project Progress** a progress bar will appear at the top of the page indicating how many genomes have been processed. If you click on **\[Active\] Last server update** just below the progress bar, you can also read several lines from the progress log. Expect it to take approximately 10 minutes to process each genome.

As the processing for each genome completes, you can get information about it. Click on **Reference dataset** and then on one of the genomes that has been processed.

Under **Ribosomal RNA**, **Feature locations** you will find the position within the genome of each 16S rRNA gene sequence. Clicking on **Classify with RDP** classifies the 16S rRNA gene sequences using the RDP database.

Clicking on **Quality**, **Full report** gives the number of essential genes found, which have multiple copies, and a list of the missing essential genes. **Completeness** is the percentage of essential genes found in the genome.

Under **Gene prediction** you have the opportunity to download protein sequences, gene sequences, and a **gff3** \(General Feature Format\) file. The gff3 file gives information on features in the genome sequence.

**Assembly** reports statistics on the assembly.

