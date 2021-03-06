## Rstudio Integration

RStudio offers a polished experience for developing R programs that is impossible to reproduce in a notebook.  Databricks offers two choices for sticking with your favorite IDE - hosted in the cloud or on your local machine.  In this section we'll go over setup and best practices for both options.

**Contents**

* [Hosted RStudio Server](#hosted-rstudio-server)
  * [Accessing Spark](#accessing-spark)
  * [Persisting Work](#persisting-work)
  * [Git Integration](#git-integration)
* [RStudio Desktop with Databricks Connect](#rstudio-desktop-with-databricks-connect)
  * [Setup](#connecting-to-spark)
  * [Limitations](#limitations)
 
 ___
 
### Hosted RStudio Server
The simplest way to use RStudio with Databricks is to install the open source version of Rstudio Server on the driver node of a cluster, as illustrated below.

<img src="https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/images/rstudioServerarchitecture.png?" raw = true>

This is achieved by attaching the [RStudio Server installation script](https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/Customizing.md#rstudio-server-installation) to your cluster.  Full details for setting up hosted Rstudio Server can be found in the official docs [here](https://docs.databricks.com/spark/latest/sparkr/rstudio.html#get-started-with-rstudio-server-open-source).  If you have a license for RStudio Server Pro you can [set that up](https://docs.databricks.com/spark/latest/sparkr/rstudio.html#install-rstudio-server-pro) as well.

After the init script has been installed on the cluster, login information can be found in the `Apps` tab of the cluster UI:

<img src ="https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/images/Rstudio_integration.png?" height = 275 width = 2000>

The hosted RStudio experience should feel nearly identical to your desktop experience.  In fact, you can also customize the environment further by supplying an additional init script to [modify `Rprofile.site`](https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/Customizing.md#modifying-rprofile-in-rstudio).  One major limitation is the inability to host Shiny Apps today, though this is on the roadmap for 2020.

#### Accessing Spark

_(This section is taken from [Databricks documentation](https://docs.databricks.com/spark/latest/sparkr/rstudio.html#get-started-with-rstudio-server-open-source).)_

From the RStudio UI, you can import the SparkR package and set up a SparkR session to launch Spark jobs on your cluster.

```r
library(SparkR)
sparkR.session()
```

You can also attach the sparklyr package and set up a Spark connection.

```r
SparkR::sparkR.session()
library(sparklyr)
sc <- spark_connect(method = "databricks")
```

This will display the tables registered in the metastore.  

<img src="https://docs.databricks.com/_images/rstudiosessionuisparklyr.png" height=300 width=420>


Tables from the `default` database will be shown, but you can switch the database using `sparklyr::tbl_change_db()`.

```r
## Change from default database to non-default database
tbl_change_db(sc, "not_a_default_db")
```

#### Persisting Work

Databricks clusters are treated as ephemeral computational resources - the default configuration is for the cluster to automatically terminate after 120 minutes of inactivity. This applies to your hosted instance of RStudio as well.  To persist your work you'll need to use [DBFS](https://github.com/marygracemoesta/R-User-Guide/blob/master/Getting_Started/DBFS.md) or integrate with version control.  

By default, the working directory in RStudio will be on the driver node.  To change it to a path on DBFS, simply set it with `setwd()`.

```r
setwd("/dbfs/my_folder_that_will_persist")
```

You can also access DBFS in the File Explorer.  Click on the `...` all the way to the right and enter `/dbfs/` at the prompt.

<img src="https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/images/rstudio-gotofolder.png" height = 250 width = 450>

Then the contents of DBFS will be available:

<img src="https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/images/file_explorer_rstudio_dbfs.png" height=200 width=450>

You can store RStudio Projects on DBFS and any other arbitrary file.  When your cluster is terminated at the end of your session, the work will be there for you when you return.

#### Git Integration

### RStudio Desktop with Databricks Connect

#### Setup

#### Limitations

## Rstudio Desktop Integration
Databricks also supports integration with Rstudio Desktop using Databricks Connect. Please refer to the [PDF](https://github.com/marygracemoesta/R-User-Guide/blob/master/Developing_on_Databricks/DB%20Connect%20with%20RStudio%20Dekstop.pdf) For step by step instructions. 

## Differences in Integrations
When it comes to the two different integrations with Rstudio - the distinction between the two become prevalant when looking at the architecture. In the Rstudio Server integration, Rstudio lives inside the driver. 


Where as Rstudio Desktop + DB Connect uses the local machine and the drive and submit queries to the nodes managed by the Databricks cluster. 

## Gotchas 
Something important to note when using the Rstudio integrations:
- Loss of notebook functionality: magic commands that work in Databricks notebooks, do not work within Rstudio
- As of September 2019, `sparklyr` is *not* supported using Rstudio Desktop + DB Connect 


___
[Back to table of contents](https://github.com/marygracemoesta/R-User-Guide#contents)

