# Using Your Own Project

In this exercise we will use the genome project from the previous exercise. The genomes were all from the same genus, and we held back one other genome from the same genus. We will now use the genome that we held back as our query genome and see how closely it matches the other genomes in our project.

Open MiGA-Web in your browser and log in if necessary. Click on the white **Projects** button just to the left of the blue **Create project** button. Select the project that you created in the genome project exercise.

You are taken to a page where you can upload query datasets. Click on the blue **Upload query datasets** button. On the next page, select the **Type of dataset** \(**Genome** for this exercise\), the **Type of input** \(**Assembly in Fasta format** for this exercise\), and click on **Choose Files**. Browse to and select your query genome, then scroll to the bottom of the page and click on **Upload new dataset**.

There is a special daemons controller that you have to turn on to process query genomes. To do so, click on the green website icon in the upper right, select **Admin console**, **Control and review daemons**. At the top of the page under **Daemons controller**, click on the switch next to, "Toggle to turn the contoller on or off." If it is blue, with the white cirlce to the right, the daemons controller is on.  

Return to your project page and monitor progress as you did for the genome exercise.

After the project finishes, click on **Query datasets** and then the name of you query genome. In addition to everything reported for a reference genome, the **Distances** report will include distances to the closest match\(es\) in the reference database, the taxonomic classification with significance \(p-values\) for each rank, and a statement on taxonomic novelty. Also, under **Genome relatedness** you can download a phylogenetic tree in pdf or newick format showing the placement of the query genome relative to the reference genomes and view a table giving the average AAI to genomes in the reference database.

