# Edits by me
Few modification to let RiboseQC run on R 4.2.1.
1. Replaced ```nsns<-disjointExons(annotation,aggregateGenes=TRUE)``` in the ```prepare_annotation_files``` function with:  
   	```
      nsns<-exonicParts(annotation, linked.to.single.gene.only=F)
      mcols(nsns) <- DataFrame(mcols(nsns)[c(3,2)],exonic_part=NA) 
      #disjointExons is deprecated
      #replacing disjointExons with exonicParts and changing the format back by Tim-Yu
      ```
      See [Tim-Yu's github](https://github.com/Tim-Yu/RiboseQC).  
2. Removed  line ```genome_sequence<-get(library(GTF_annotation$genome,character.only = TRUE))``` from the ```load_annotation``` function.
3. Add line ```gtfdata <- gtfdata[!is.na(gtfdata$transcript_id),]``` This is to remove rows where ```transcript_id==NA``` from the ```gtfdata``` GRanges object.   
4. Command out ```#stopifnot(length(unique(gtfdata$transcript_id))==n_transcripts-1)```
5. Switch colors "blue" and "forestgreen". So Frame 0 is Dark Red, Frame 1 is Blue and Frame 2 is ForestGreen. (RiboseQC uses Dark Red, ForestGreen and Blue). This modification is to match the output with RiboPlotR.
6. Modify chunk_size limits to `chunk_size < 1e+09` was `chunk_size < 1e+08` (i.e, 100M reads)
7. For the `RiboseQC_analysis` function, changed the default `readlength_choice_method = "max_coverage"` to `readlength_choice_method = "max_inframe"`

Run the following code to install and load.
```
install_github("hsinyenwu/RiboseQC_R4.2.1")
library("RiboseQC")
```
# Original readme by authors:
# Ribo-seQC
A comprehensive analysis tool for Ribo-seq and small RNA-seq data

**Ribo-seQC** (*RiboseQC*) is an R package (to be submitted to *Bioconductor*) that performs quality control analysis of small RNA-seq data, with a focus on Ribo-seq and related techniques. Thanks to syntax and functions present in *Bioconductor* packages like *GenomicFeatures*, *rtracklayer* or *BSgenome*, this package can perform comprehensive analyses on a variety of genomic regions. In addition, Ribo-seQC allows to automatically generate an html report for each analyzed sample, allowing for quick and interactive comparison of multiple samples at once.

This tools focuses on the analysis of different read lengths, taking into account the genomic regions they map to (e.g. coding sequence, UTRs, non-coding RNAs, mitochondria or chloroplasts, etc...). Other useful features, such as automatic P-sites position calculation or analysis the top mapping positions, are available in the Ribo-seQC package, and we encourage to donwload and have a look at the vignette (**RiboseQC.html** https://htmlpreview.github.io/?https://github.com/lcalviell/Ribo-seQC/blob/master/RiboseQC.html ), our manual (**RiboseQC-manual.pdf**), and our manuscript (to be added soon...).

An example Ribo-seQC html report for Ribo-seq data from Arabodopsis roots and shoots (Hsu *et al*, PNAS 2016) is available here, and can be downloaded and opened using a browser like Chrome, Firefox or others (*Warning*, file size is ~120Mb):

https://drive.google.com/open?id=13OBuS4CG0GA6j3Dt68zRtC8KBzNkmO7J

Here another report created using data from a TCP-seq (Archer *et al*, Nature 2016) experiment in yeast (*Warning*, file size is ~110Mb): 

https://drive.google.com/open?id=1568rBoFgYBREhrNeHQr1-66nQVKA8WEZ



To install Ribo-seQC:

```
library("devtools")
install_github(repo = "lcalviell/Ribo-seQC")
library("RiboseQC")

```

Two simple steps are required to use Ribo-seQC on your data:
```
?prepare_annotation_files
```
parses a *.gtf* and a *.2bit* file. (this need to be done once per each annotation-genome combination, a .2bit file can be obtained from a fasta file using the *faToTwoBit* software from UCSC: https://genome.ucsc.edu/goldenpath/help/twoBit.html - http://hgdownload.soe.ucsc.edu/admin/exe/ )


and
```
?RiboseQC_analysis
```

the master function used to perform the entire analysis workflow.
Please check the vignette for some example workflows.


For any question, please email:

calviello.l.bio@gmail.com, dominique.sydow@posteo.de (analysis and data visualization), Dermot.Harnett@mdc-berlin.de (package mantainer), Uwe.ohler@mdc-berlin.de (project supervisor).


Enjoy!


