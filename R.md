* Programming language for statistics, data analysis, machine learning, and bioinformatics.
* Popular in HPC, academics, genomics, finance, machine learning.
* Uses CRAN (Comprehensive R Archive Network) for packages.

* How to Install R
  ```bash
  sudo apt install r-base #Ubuntu
  sudo dnf install R #CentOS/RHEL
  ```

* How to Open R
  ```bash
  R #Start R console
  q() #Stop R
  ```

---
---

Below is your **complete and beginner-friendly R tutorial**, written in **simple bullet points** as you prefer.
This covers everything — from basics → advanced → data science → plotting → statistics → packages → real projects.

---

# **🔥 Complete R Programming Tutorial (Beginner → Advanced)**

---

# **1. What is R?**

* Programming language for **statistics**, **data analysis**, **machine learning**, and **bioinformatics**.
* Popular in **HPC**, **academics**, **genomics**, **finance**, **machine learning**.
* Uses **CRAN** (Comprehensive R Archive Network) for packages.

---

# **2. How to Install R**

* Linux:

```
sudo apt install r-base
```

* RHEL/CentOS:

```
sudo dnf install R
```

* Windows: download from CRAN
* macOS: install from CRAN

---

# **3. How to Open R**

* Terminal:

```
R
```

* Start R console
* Stop R:

```
q()
```

---

# **4. R Script vs Console**

* **Console** → interactive commands
* **Script** (.R file) → saved and reusable code

Run a script:

```
Rscript myscript.R
```

---

# **5. R Basic Syntax**

* Assignment:

```
x <- 10
y = 20
```

* Print:

```
print(x)
```

* Comments:

```
# This is a comment
```

* Check type:

```
class(x)
```

---

# **6. Data Types**

* Numeric: `10, 5.6`
* Integer: `10L`
* Character: `"Hello"`
* Logical: `TRUE, FALSE`
* Complex: `2 + 3i`

---

# **7. Vectors**

```
v <- c(1,2,3,4)
length(v)
v[1]       # indexing
```

---

# **8. Matrices**

```
m <- matrix(1:9, nrow=3)
m[2,3]
```

---

# **9. Factors (Categorical data)**

```
gender <- factor(c("male","female","male"))
levels(gender)
```

---

# **10. Lists**

```
lst <- list(name="Mahin", age=24, scores=c(10,20))
lst$name
```

---

# **11. Data Frames**

```
df <- data.frame(name=c("A","B"), age=c(20,25))
df$name
df[1,]
```

---

# **12. Reading/Writing Data**

* CSV:

```
df <- read.csv("file.csv")
write.csv(df, "out.csv")
```

* Text:

```
read.table("file.txt")
```

* Excel:

```
install.packages("readxl")
library(readxl)
read_excel("file.xlsx")
```

---

# **13. Packages – Install & Load**

```
install.packages("ggplot2")
library(ggplot2)
```

---

# **14. Functions**

```
add <- function(a,b) {
  return(a+b)
}
add(5,10)
```

---

# **15. Control Flow**

* If:

```
if(x > 10) print("big")
```

* For Loop:

```
for(i in 1:5) print(i)
```

* While:

```
while(x < 10) x <- x + 1
```

---

# **16. Apply Family**

```
lapply(df, mean)
sapply(df, mean)
```

---

# **17. Plotting with Base R**

```
plot(df$age)
hist(df$age)
boxplot(df$age)
```

---

# **18. ggplot2 (Advanced Plotting)**

```
library(ggplot2)
ggplot(df, aes(x=age)) + geom_histogram()
```

---

# **19. Data Manipulation with dplyr**

```
install.packages("dplyr")
library(dplyr)

df %>% filter(age > 20)
df %>% select(name)
df %>% mutate(new_age = age + 5)
df %>% arrange(desc(age))
```

---

# **20. Tidyverse**

Install:

```
install.packages("tidyverse")
library(tidyverse)
```

Includes: dplyr, ggplot2, tidyr, readr, tibble, etc.

---

# **21. Statistical Analysis**

* Mean:

```
mean(df$age)
```

* Median:

```
median(df$age)
```

* SD:

```
sd(df$age)
```

* t-test:

```
t.test(group1, group2)
```

* Correlation:

```
cor(df$age, df$salary)
```

---

# **22. Machine Learning in R**

### Linear Regression:

```
model <- lm(y ~ x, data=df)
summary(model)
```

### Random Forest:

```
install.packages("randomForest")
library(randomForest)
rf <- randomForest(y ~ ., data=df)
```

### K-means Clustering:

```
kmeans(df, centers=3)
```

---

# **23. R for Bioinformatics**

Popular packages:

* Seurat
* DESeq2
* edgeR
* limma
* Bioconductor Packages

Install Bioconductor:

```
install.packages("BiocManager")
BiocManager::install()
```

Install a package:

```
BiocManager::install("DESeq2")
```

---

# **24. Working with R in Singularity / Apptainer**

Example:

```
apptainer exec myimage.sif Rscript script.R
```

---

# **25. Projects You Can Build**

* Data analysis dashboard using Shiny
* Machine learning model with caret
* Genomics pipeline using Seurat
* Data visualization using ggplot2
* Financial analysis toolkit

---

# **26. Learning Path (Step-by-Step)**

1. Basics: variables, vectors, data frames
2. Functions, loops, apply
3. Data read/write
4. Plotting (ggplot2)
5. dplyr for manipulation
6. Statistics
7. Machine learning
8. Shiny Apps
9. Bioinformatics (if required)

---

# **27. Best Free Resources**

* CRAN R manual
* R for Data Science (Hadley Wickham)
* Swirl package (interactive learning):

```
install.packages("swirl")
library(swirl)
swirl()
```

---

If you want, I can also create:

✔ A full **R PDF book**
✔ A **30-day R learning plan**
✔ A **bioinformatics R guide** (Seurat, Single-cell analysis)
✔ R projects with complete code

Just tell me!

---
---

Here are **all the different ways to install packages in R**, explained in **simple, clean bullet points** with examples.

---

## ✅ **1. Install from CRAN (Most Common Way)**

CRAN = Official R package repository.

**Command:**

```
install.packages("ggplot2")
```

**Multiple packages:**

```
install.packages(c("dplyr", "tidyr", "readr"))
```

**Specify CRAN mirror:**

```
install.packages("MASS", repos="https://cloud.r-project.org")
```

---

## ✅ **2. Install from Bioconductor (Bioinformatics Packages)**

Bioconductor = specialized for genomics, sequencing, bioinformatics.

**First install manager:**

```R
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
```

**Install packages:**

```R
BiocManager::install("GenomicFeatures")
BiocManager::install(c("DESeq2", "edgeR"))
```

---

## ✅ **3. Install from GitHub (Latest Development Versions)**

Used for R packages in GitHub repositories.

**Install `remotes` or `devtools`:**

```
install.packages("remotes")
```

**Install package:**

```
remotes::install_github("satijalab/seurat")
```

**Install a specific branch:**

```
remotes::install_github("satijalab/seurat", ref="develop")
```

---

## ✅ **4. Install from Local `.tar.gz` or Source File**

Used in offline environments / HPC clusters.

**Command:**

```
install.packages("mypkg.tar.gz", repos = NULL, type = "source")
```

---

## ✅ **5. Install a Specific Package Version**

Useful when software requires exact versions.

**Using `remotes`:**

```
remotes::install_version("Matrix", version="1.6-1")
```

---

## ✅ **6. Install from GitLab, Bitbucket, or URL**

**From GitLab:**

```
remotes::install_gitlab("username/repo")
```

**From Bitbucket:**

```
remotes::install_bitbucket("username/repo")
```

**Direct URL of tar.gz:**

```
install.packages("https://myserver.com/pkg.tar.gz", repos=NULL, type="source")
```

---

## ✅ **7. Install Using Conda (R inside Conda)**

For environments using conda.

```
conda install -c r r-ggplot2
conda install -c bioconda bioconductor-deseq2
```

---

## ✅ **8. Install Using Singularity / Apptainer (Inside Container)**

Inside a container’s %post section:

```
Rscript -e 'install.packages("ggplot2", repos="https://cloud.r-project.org")'
```

Using remotes:

```
Rscript -e 'remotes::install_github("satijalab/sctransform")'
```

---

## ✅ **9. Install Using pak (Fast Package Manager)**

Pak is faster and smarter than install.packages.

```
install.packages("pak")
pak::pak("dplyr")
```

---

## ✅ **10. Install from RStudio UI (Clicking)**

In RStudio → **Packages → Install**
Type name → Install.

---

## Summary (Super Short)

You can install R packages using:

* **CRAN** → `install.packages()`
* **Bioconductor** → `BiocManager::install()`
* **GitHub** → `remotes::install_github()`
* **Source file** → `install.packages("x.tar.gz", repos=NULL)`
* **Specific versions** → `remotes::install_version()`
* **URL** → `install.packages("url")`
* **Conda** → `conda install r-xxx`
* **Container builds** → `Rscript -e "install.packages()"`
* **RStudio GUI** → Click-to-install

---

If you want, I can also give:

✔ Complete R tutorial
✔ Complete Seurat + SCTransform tutorial
✔ Complete R environment setup for HPC
✔ Your def file explained line-by-line

Just tell me what you want next.

---
---

Here is the **clear, complete explanation of `Rscript -e`**, in simple bullet points:

---

## **What is `Rscript -e`?**

* `Rscript` is the command-line tool used to run **R code** from the terminal.
* The `-e` flag means **“execute the following expression”**.
* So `Rscript -e "<R code>"` lets you run a **single line of R code** without opening the R console.

---

## **Examples**

### **1. Print something**

```
Rscript -e 'print("Hello from R!")'
```

### **2. Get R version**

```
Rscript -e 'R.version.string'
```

### **3. Install a package**

```
Rscript -e 'install.packages("ggplot2", repos="https://cloud.r-project.org")'
```

### **4. Check package version**

```
Rscript -e 'packageVersion("ggplot2")'
```

### **5. Run multiple expressions**

```
Rscript -e 'x <- 10; y <- 20; print(x+y)'
```

---

## **Where is it used?**

* In **shell scripts**
* In **Singularity/Apptainer %post** sections
* In **Dockerfiles**
* In **automation pipelines (SLURM, HPC)**

Example from your Singularity definition:

```
Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org")'
```

This command:

1. Opens R
2. Runs `install.packages("remotes")`
3. Closes R

This is ideal for automated installs.

---

## **Difference between `R` and `Rscript`**

* `R`
  → Opens R interactive console
* `Rscript`
  → Runs R code **non-interactively** (script mode)

---

## **When to use `Rscript -e`**

Use it when:

* building Docker/Singularity images
* writing Linux automation scripts
* installing packages in HPC
* running quick R commands without entering interactive R

---

If you want, I can also explain:

✔ How `Rscript` loads libraries
✔ How to run `.R` files
✔ How to pass arguments to R scripts (`commandArgs()`)
✔ Complete R tutorial (basics → advanced → tidyverse → Seurat)

Just tell me.


---
---

Here is a **simple and complete explanation of CRAN** in your preferred bullet-point style:

---

## **What is CRAN?**

**CRAN = Comprehensive R Archive Network**

---

## **Meaning**

* It is the **official repository** for R packages.
* It is maintained by the **R Foundation**.
* It contains **verified**, **tested**, and **trusted** R packages.
* Works similar to:

  * Python → PyPI
  * Linux → yum/dnf repos
  * Node.js → npm
* Whenever you run:

  ```
  install.packages("ggplot2")
  ```

  It downloads from **CRAN** by default.

---

## **Why CRAN exists**

* To provide a **central, safe, stable** place for R packages.
* To ensure all packages pass strict checks (CRAN enforces quality).
* To make installation easy for users worldwide.

---

## **What CRAN contains**

* Thousands of R packages (ggplot2, dplyr, Seurat, tidyverse, etc.)
* Source code
* Windows / macOS binaries
* Package documentation & vignettes
* R versions, manuals, news, and updates

---

## **How R installs from CRAN**

When you run:

```
install.packages("dplyr", repos="https://cloud.r-project.org")
```

R will:

1. Connect to a CRAN mirror
2. Download the package (latest version)
3. Compile it (if needed)
4. Install it to your R library path

---

## **CRAN vs GitHub vs Bioconductor**

* **CRAN**

  * Stable packages
  * Official testing
  * Most commonly used
* **GitHub**

  * Development versions
  * For bleeding-edge experimental code
  * Installed using `remotes::install_github()`
* **Bioconductor**

  * For genomics/bioinformatics
  * Installed using `BiocManager::install()`

---

## **CRAN Mirrors**

* CRAN has servers worldwide.
* Example popular mirror:

  ```
  https://cloud.r-project.org
  ```
* Mirrors help fast download and redundancy.

---

## **Where to see CRAN**

You can visit:
[https://cran.r-project.org](https://cran.r-project.org)

---

If you want, I can also explain:
✔ What is a CRAN mirror
✔ What is CRAN Task View
✔ How `install.packages()` works internally
✔ Why clusters/HPC always use cloud.r-project.org
✔ How CRAN checks packages before approving

Just tell me!


---
---

