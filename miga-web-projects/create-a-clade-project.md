# Create a Clade Project

A clade project is a collection of closely related genomes, _e.g._ from the same species. The genomes are compared against each other.

For this exercise you may download several genomes of the same species from NCBI or use the 10 _Dehalocccoides_ genomes available at [https://github.com/jfq3/data\_sets](https://github.com/jfq3/data_sets).

Open MiGA-Web in your browser and log in if necessary. Click on **Create new project**.

On the **New project** page, enter a short name for your project in the **Path** box. Then select **Clade** as the **Type** of project you want to run.

Under the section **Identity Engines**, the recommended settings are already filled in. You may change these to make your analysis run more quickly, but with some loss of sensitivity.

You may add a name, description, and comments for your project in the **Optional information** section if you wish.

Scroll down to the bottom of the page and click on the blue **Create new MiGA project** button.

After creating the project, you are taken to a page for uploading datasets. Click on **Upload reference datasets**. This will take you to another page for uploading genome sequences.

The **Type of dataset** is **Genome**, and the **Type of input** is **Assembly in Fasta format**. Click on **Choose Files** and select the 10 pseudomonad genomes \(or other genomes\) you are using. Scroll down to the bottom of the page and click on the blue button **Upload new dataset**.

To start processing, you must turn on the daemon. Click on the green website icon in the upper right, select **Admin console**, **Control and review daemons**, and turn on the daemon for your project.

Return to the Project page and monitor progress with the progress bar. This exercise took about 90 minutes on my computer.

After processing is complete, all of the information available for genome projects is available from the menu in the middle of the project page. Because this is a clade project, additional information is available under the headings **Clade Finding**, **Subclades**, and **Orthologous groups of proteins**. Explore this information by clicking on each of these headings.

In this example, MiGA proposes 3 subclades. Clicking on "report" under the heading **Subclades** presents a graphic showing how the clustering was done and a phylogenetic tree, also available in Newick format \(click on "ani tree."\)

Other clade related information is available for each of the reference sequences under **Distances**. For each genome there is a statement about its closest relatives and ANI and AAI tables showing relatedness to the other genomes in the dataset.

