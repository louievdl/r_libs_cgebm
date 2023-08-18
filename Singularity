Bootstrap: docker
From: rocker/tidyverse:4.3.1 # R version
#From: rocker/r-ubuntu # official repo, based on jammy

%runscript
 echo "Define actions for the container to be executed with the run command or"
 echo "when container is executed."

%files
 # src-on-host dest-in-image # no dest puts pkgs in root of image
 /home/lvande/org.Calbicans.eg.db.zip .
 /home/lvande/org.Osativa.eg.db.zip .
 /home/lvande/org.Xtropicalis.eg.db.zip .

%post

 echo `uname --all`
 echo `lsb_release -a`
 apt update
 apt -y install r-base r-base-dev r-recommended # r-base-dev necessary for installing r packages (e.g. by install.packages)
 apt-get clean
 # v4.3.1 comes with BiocManager 3.17, tidyverse 2.0.0

 R --version

 # install some annotation dbs, which will install deps also rqd for bespoke libs
 Rscript -e 'BiocManager::install(c("org.Hs.eg.db", "org.Mm.eg.db", "org.Xl.eg.db"))'

 # install bespoke annotation libs
 unzip ./org.Calbicans.eg.db.zip
 unzip ./org.Osativa.eg.db.zip
 unzip ./org.Xtropicalis.eg.db.zip
 Rscript -e 'install.packages("./org.Calbicans.eg.db", repos=NULL)'
 Rscript -e 'install.packages("./org.Osativa.eg.db", repos=NULL)'
 Rscript -e 'install.packages("./org.Xtropicalis.eg.db", repos=NULL)'
 Rscript -e 'library(org.Calbicans.eg.db); library(org.Osativa.eg.db); library(org.Xtropicalis.eg.db); sessionInfo()'
 rm -r ./org.Calbicans.eg.db.zip ./org.Osativa.eg.db.zip ./org.Xtropicalis.eg.db.zip ./org.Calbicans.eg.db ./org.Osativa.eg.db ./org.Xtropicalis.eg.db

 # remaining R pkg insts done in the following order.
 Rscript -e 'install.packages("gplots", repos = "https://cloud.r-project.org")'
 Rscript -e 'install.packages("igraph", repos = "https://cloud.r-project.org")' # clusterProfiler inst failed on missing igraph previously
 Rscript -e 'install.packages(c("vegan", "seriation", "openxlsx", "msigdbr"), repos = "https://cloud.r-project.org")'
 Rscript -e 'BiocManager::install("clusterProfiler")' # includes DOSE,enrichplot,fgsea as dep
 Rscript -e 'BiocManager::install(c("edgeR", "dada2", "phyloseq"))'
 Rscript -e 'BiocManager::install("erccdashboard")'
 # R pkg installation too fragile - igraph needed by phyloseq (and clusterProfiler?), but not downloaded by them.
 #Rscript -e 'install.packages(c("BiocManager", "vegan", "seriation", "openxlsx", "msigdbr", "gplots", "igraph"), repos = "https://cloud.r-project.org")'
 #Rscript -e 'BiocManager::install(c("edgeR", "dada2", "phyloseq", "clusterProfiler", "DOSE", "enrichplot"))'
 Rscript -e 'library(edgeR); library(tidyverse); library(openxlsx); library(msigdbr); library(gplots); library(edgeR); library(clusterProfiler); library(DOSE); library(enrichplot); sessionInfo()'
 Rscript -e 'library(seriation); library(vegan); library(dada2); library(phyloseq); sessionInfo()'
 Rscript -e 'library(erccdashboard); sessionInfo()'

%environment
 #export PATH="$PATH:/usr/games"
 export LC_ALL=C


