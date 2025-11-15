* Singularity → Now officially called Apptainer
* Docker → Industry-standard container engine

### Singularity / Apptainer
* Container platform designed for HPC (High Performance Computing), research labs, universities.
* Supports running containers without root privileges.
* Ideal for clusters, shared servers, scientific computing, bioinformatics pipelines.
* Uses `.sif` image format (Singularity Image File).

### Docker
* General-purpose container engine for developers, DevOps, microservices, CI/CD.
* Requires root privileges.
* Uses layered images from Docker Hub or private registries.

### Why HPC Doesn’t Allow Docker
* Docker requires a daemon running as root, high security risk.
* HPC systems share nodes between many users; root access → not allowed.
* Therefore HPC uses Singularity/Apptainer, because it:
    * Runs without root
    * Uses user namespaces
    * Cannot escalate privileges
    * Works well in shared environments

### Key Differences
* Docker:
    * Root required
    * Great for dev environment
    * Layered images
    * Uses Dockerfile
* Singularity/Apptainer:
    * No daemon
    * No root needed for running containers
    * Uses .def file for builds (Singularity Definition File)
    * Packs everything into one .sif file

```bash
apptainer run docker://python:3.10
apptainer pull ubuntu.sif docker://ubuntu
```
```bash
apptainer build myimage.sif docker://myrepo/myimage
```
```bash
apptainer build myimage.sif myimage.def
```
```bash
apptainer shell myimage.sif
apptainer exec myimage.sif python script.py
apptainer run myimage.sif
```

```bash
sudo dnf groupinstall -y "Development Tools"

sudo dnf install -y \
    wget \
    git \
    uuid-devel \
    cryptsetup \
    squashfs-tools \
    libseccomp-devel \
    openssl-devel \
    golang \
    libuuid-devel

sudo dnf install -y epel-release

#Set up Go environment
#If Go is installed from the repo, it probably works out of the box:
export GOPATH=/usr/local/go
export PATH=$PATH:/usr/local/go/bin
#Some systems place Go in /usr/lib/golang, adjust if needed.

#Download latest Apptainer source
wget https://github.com/apptainer/apptainer/releases/download/v1.3.2/apptainer-1.3.2.tar.gz
tar -xzf apptainer-1.3.2.tar.gz
cd apptainer-1.3.2

#Configure, build, install
./mconfig
make -C ./builddir
sudo make -C ./builddir install
#This installs apptainer to /usr/local/bin.

apptainer --version

#Enable unprivileged (non-root) runtime
sudo mkdir -p /etc/apptainer
#Allow unprivileged user namespaces:
sudo sh -c 'echo "user.max_user_namespaces=15000" > /etc/sysctl.d/90-apptainer.conf'
sudo sysctl --system

sysctl user.max_user_namespaces

apptainer run docker://alpine echo "hello"
```
or 
```bash
sudo dnf install -y epel-release
sudo dnf install -y apptainer
```

---

```bash
Bootstrap: docker #Tells Singularity/Apptainer to pull the base image from Docker Hub.
From: rocker/r-ver:4.5.1 #Uses the Docker image rocker/r-ver:4.5.1 as the base(already contains R version 4.5.1)

%labels #stores metadata inside the .sif image. Labels do not affect runtime; they are only information for users.
    Author Vikas (C2B2) #identifies the author of the image.
    Version v5-fixed #version of the image.
    Description "R 4.5.1 with Seurat 5.0.3, Matrix 1.7.4, SCTransform. Azimuth installable at runtime." #summary of what the container contains.

%environment #This section runs every time the container starts, not during build.
    export R_LIBS_USER=/groups/db3700_gp/ya2540/Rlibs #Sets the folder where R will install user-level packages.
    export TMPDIR=/groups/db3700_gp/ya2540/tmp #Sets the default temporary directory inside the HPC user space.
    export PATH=/usr/local/bin:$PATH #Adds /usr/local/bin to PATH so programs installed there are runnable.

%post 
    apt-get update && apt-get install -y \ 
        libcurl4-openssl-dev libssl-dev libxml2-dev \
        libhdf5-dev gfortran libatlas-base-dev \
        libopenblas-dev liblapack-dev libpng-dev \
        git curl

    #The %post section runs inside the container at build time.
        # apt-get update --> Updates package lists.
        # apt-get install -y --> installs required system libraries:
        # libcurl4-openssl-dev --> needed for R packages httr, curl, remotes.
        # libssl-dev --> needed for https connections, cryptography.
        # libxml2-dev --> XML parsing support (common in R).
        # libhdf5-dev --> HDF5 support for Seurat and bioinformatics.
        # gfortran --> Fortran compiler (needed for R matrix operations).
        # libatlas-base-dev, libopenblas-dev, liblapack-dev --> Math libraries for fast matrix operations.
        # libpng-dev --> PNG image support.
        # git, curl --> required for installing packages from GitHub and general downloads.

    Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org")' #Installs remotes, which allows R to install packages from GitHub and specific versions.

    Rscript -e 'remotes::install_version("Matrix", version = "1.7.4", repos = "https://cloud.r-project.org")' #Installs Matrix v1.7.4 (specific required version).

    Rscript -e 'remotes::install_version("SeuratObject", version = "5.0.2", repos = "https://cloud.r-project.org")' #Installs SeuratObject v5.0.2 (specific required version).

    Rscript -e 'remotes::install_version("Seurat", version = "5.0.3", repos = "https://cloud.r-project.org")' #Installs Seurat v5.0.3 (specific required version).


    Rscript -e 'remotes::install_github("satijalab/sctransform", ref = "master")' #Installs the SCTransform package directly from GitHub, Uses the latest master branch.

    Rscript -e 'install.packages(c("future", "future.apply", "ggplot2", "patchwork"), repos = "https://cloud.r-project.org")' #Installs additional R packages:

    rm -rf /root/.cache /tmp/* #Deletes caches to reduce image size.

%test #This section runs when someone tests the container:
    Rscript -e 'cat("Seurat:", as.character(packageVersion("Seurat")), "\n")'
    Rscript -e 'cat("Matrix:", as.character(packageVersion("Matrix")), "\n")'
    Rscript -e 'cat("SCTransform:", as.character(packageVersion("sctransform")), "\n")'

    #apptainer test image.sif
    #It prints installed versions:
        #Seurat
        #Matrix
        #SCTransform
    #Useful to verify correct installation.

%runscript
    exec R "$@"
#This defines the default executable when someone runs:
#apptainer run image.sif
#It will open the R console, passing any user arguments.
#apptainer run image.sif myscript.R --> R myscript.R

```

---

* **What is a `.def` file in Singularity/Apptainer?**
    * A `.def` file = **recipe file** used to **build a Singularity/Apptainer image**.
    * It contains:
        * Base image (Docker, Apptainer, scratch)
        * Packages you want to install
        * Environment variables
        * Labels
        * Test scripts
        * Run commands
    * It is similar to a **Dockerfile**.

<br>

* **Why do we need a `.def` file?**
    * You can **reproduce** the same container anywhere.
    * You can **version control** your environment.
    * You can **customize** OS, R/Python packages, paths, etc.
    * It creates a single output file: **`image.sif`**

<br>

* **Basic Structure of a `.def` File**
    * A definition file is divided into sections:
        ```bash
        Bootstrap:
        From:

        %labels
        %environment
        %post
        %files
        %runscript
        %test
        ```
    * Each block has a purpose.

<br>

* **Meaning of Each Section**
  * **Bootstrap**
    * Tells Singularity what type of base to use.
        ```bash
        Bootstrap: docker
        ```
    * This means: “Pull image from Docker Hub”.
    * Other options:
        * `library` (Apptainer library)
        * `localimage` (existing SIF)
        * `yum` (build from RHEL/CentOS repo)
        * `debootstrap` (Debian/Ubuntu)
        * `scratch`

<br>

  * **From**
    * Base image name.
        ```bash
        From: rocker/r-ver:4.5.1
        ```
    * Pull from Docker Hub → `rocker/r-ver` image
    * Version tag: `4.5.1`

<br>

  * **%labels**
    * Adds metadata to the final `.sif`.
        ```bash
        %labels
            Author Vikas (C2B2)
            Version v5-fixed
        ```
    * Used for:
        * Documentation
        * Tracking version
        * Owner information
    * You can show labels using:
        ```bash
        apptainer inspect image.sif
        ```

<br>

* **%environment**
  * This section defines **environment variables** that will be used whenever the container runs.
    ```bash
    %environment
        export R_LIBS_USER=/groups/db3700_gp/ya2540/Rlibs
        export TMPDIR=/groups/db3700_gp/ya2540/tmp
        export PATH=/usr/local/bin:$PATH
    ```
  * Meaning:
    * R libraries will be installed in `/groups/.../Rlibs`
    * Temporary files stored in `/groups/.../tmp`
    * Add `/usr/local/bin` to PATH

<br>

* **%files**
  * To copy files into the container.
    ```bash
    %files
        requirements.txt /root/requirements.txt
    ```

<br>

* **%help**
  * Help text shown using:
    ```bash
    apptainer run-help image.sif
    ```

<br>

* **%post**
  * This is the MOST IMPORTANT section.
    * Runs **inside the container at build time**
    * Install packages, run apt, pip, R, git etc.
    ```bash
    %post
        apt-get update && apt-get install -y \
            libcurl4-openssl-dev libssl-dev libxml2-dev \
    ```
  * Meaning: install dependencies.
  * Then R packages:
    ```bash
    Rscript -e 'install.packages("remotes", repos="https://cloud.r-project.org")'
    ```
  * Everything executed here becomes *permanent* inside the `.sif`.

<br>

**%test**
* Runs tests after building the image.
```bash
%test
    Rscript -e 'cat("Seurat:", packageVersion("Seurat"), "\n")'
```
* Useful to verify:
    * package installed
    * correct versions
    * environment works

<br>

* **%runscript**
  * What happens when user runs:
    ```bash
    apptainer run image.sif
    ```
  * Example:
    ```bash
    %runscript
        exec R "$@"
    ```
  * Meaning:
    * Default command = run R interpreter


* **How to Build a `.def` File**
  * Suppose your file name is:
    ```bash
    r_env.def
    ```
  * Build it:
    ```bash
    sudo apptainer build r_env.sif r_env.def
    ```
  * If you don’t have sudo (common in HPC):
    ```bash
    apptainer build --fakeroot r_env.sif r_env.def
    ```
    Or:

    ```bash
    apptainer build --remote r_env.sif r_env.def
    ```

* **How to Check or List Images**
  * List all `.sif` files in your directory:
    ```bash
    ls *.sif
    ```
  * Inspect an image:
    ```bash
    apptainer inspect r_env.sif
    ```
  * Check runscript:
    ```bash
    apptainer inspect --runscript r_env.sif
    ```
  * Execute commands inside container:
    ```bash
    apptainer exec r_env.sif Rscript myscript.R
    ```
  * Open shell:
    ```bash
    apptainer shell r_env.sif
    ```

* **Simple Example `.def` File**
    ```bash
    Bootstrap: docker
    From: ubuntu:22.04

    %post
        apt-get update
        apt-get install -y python3 python3-pip
        pip3 install numpy pandas

    %runscript
        python3 "$@"
    ```


* **R Example `.def` File**
    ```bash
    Bootstrap: docker
    From: rocker/r-base:4.3.2

    %post
        apt-get update
        apt-get install -y libcurl4-openssl-dev libssl-dev libxml2-dev
        Rscript -e 'install.packages("tidyverse", repos="https://cran.rstudio.com")'

    %runscript
        exec R "$@"
    ```


---
---

Here is **everything about `Rscript -e`**, with **all possible usages**, **examples**, and **theory explained in comments**, exactly the way you prefer.

---

## ✅ **What is `Rscript -e` ?**

* `Rscript` = command-line tool to run **R code** without opening the R console.
* `-e` = **expression flag**, meaning:
  → *run the following R code directly as a one-line command*.

Example:

```
Rscript -e 'print("Hello")'
```

This runs R code inside quotes.

---

## ✅ **Why use `Rscript -e` in Docker / Singularity / HPC?**

* Automate installation of R packages in containers.
* Run R commands during `%post` in a `.def` file.
* Run tests (`%test` section).
* Execute R scripts in batch mode.
* No need for interactive R console.

---

# ✅ **All important `Rscript -e` commands (with full explanations in comments)**

---

## 1️⃣ **Install CRAN Packages**

```
# Install a package from CRAN (default repo)
Rscript -e 'install.packages("ggplot2", repos="https://cloud.r-project.org")'
```

Explanation:

* `install.packages("ggplot2")` → installs **ggplot2**
* `repos=` → CRAN server location
* In containers, you MUST specify repos.

---

## 2️⃣ **Install multiple packages**

```
# Install multiple packages
Rscript -e 'install.packages(c("dplyr", "tidyr", "readr"), repos="https://cloud.r-project.org")'
```

---

## 3️⃣ **Install a specific version using {remotes}**

```
# Install specific version of a package
Rscript -e 'remotes::install_version("Matrix", version="1.7.4", repos="https://cloud.r-project.org")'
```

---

## 4️⃣ **Install from GitHub**

```
# Install R package from GitHub
Rscript -e 'remotes::install_github("satijalab/seurat")'
```

---

## 5️⃣ **Print installed package version**

```
# Print version from Rscript
Rscript -e 'packageVersion("Seurat")'
```

---

## 6️⃣ **Run an R expression**

```
# Simple calculation
Rscript -e '2 + 2'
```

---

## 7️⃣ **Load a library**

```
# Check if library loads correctly
Rscript -e 'library(Seurat)'
```

---

## 8️⃣ **Execute an R function**

```
# Summary of builtin dataset
Rscript -e 'summary(cars)'
```

---

## 9️⃣ **Create an R script file**

```
# Create a file using Rscript
Rscript -e 'write.csv(mtcars, "cars.csv")'
```

---

## 🔟 **Run multiple R commands (use ; or newline)**

```
Rscript -e 'x=5; y=10; print(x*y)'
```

---

# 🧠 **What exactly happens inside `Rscript -e` ? (Theory)**

* Shell passes the string inside `' '` to `Rscript`.
* Rscript interprets it as **inline R code**.
* No interactive environment is created.
* Suitable for automation, HPC, Singularity, Docker.

This makes it perfect for `.def` files.

---

# 🟢 **How it is used in Singularity (`%post` in your .def file)**

Example from your file:

```
Rscript -e 'remotes::install_version("Matrix", version = "1.7.4", repos = "https://cloud.r-project.org")'
```

Meaning:

* During container build,
* Rscript runs an R command
* Installs Matrix version 1.7.4 inside the container.

---

# 🟦 **More useful `Rscript -e` commands (advanced)**

---

## Install Bioconductor packages

```
# Install Bioconductor core
Rscript -e 'BiocManager::install()'

# Install a specific Bioconductor package
Rscript -e 'BiocManager::install("GenomicRanges")'
```

---

## Install package from local `.tar.gz`

```
# Install local source package
Rscript -e 'install.packages("mypkg.tar.gz", repos=NULL, type="source")'
```

---

## Run an entire script file

```
Rscript my_script.R
```

---

## Print environment variables from R

```
Rscript -e 'print(Sys.getenv())'
```

---

## Set library path inside command

```
Rscript -e '.libPaths("/my/custom/Rlibs"); install.packages("dplyr", repos="https://cloud.r-project.org")'
```
