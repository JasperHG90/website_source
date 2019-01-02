---
title: "Using rstudio and sparklyr with an apache cluster on Google DataProc"
layout: post
date: "2016-10-23T12:19:00"
categories: ["tutorial"]
tag: ["R", "sparklyr", "rstudio", "apache spark", "apache hadoop", "apache hive", "google dataproc"]
author: Jasper Ginn
blog: true
description: In this post, I'd like to share how you can set up your own rstudio/apache cluster environment using Google dataproc
---

Last week, I came across [sparklyr](https://blog.rstudio.org/2016/09/27/sparklyr-r-interface-for-apache-spark/). Authored by the folks at [rstudio](https://www.rstudio.com/), it allows you to integrate your R workflow (and, more importantly, your [*dplyr*](https://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html) workflow) with [apache spark](http://spark.apache.org/). In one of [the examples](https://beta.rstudioconnect.com/content/1446/) on the sparklyr home page, the author shows how to set up rstudio and sparklyr on an [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (ECC). In this post, I'd like to share how you can set up your own rstudio/apache cluster environment using Google dataproc. Why dataproc instead of ECC? Because Google is kind (desperate?) enough to [give you](https://cloud.google.com/free-trial/) $300 worth of free playtime with their cloud services for 60 days!

The first part of this post covers setting up the apache cluster and rstudio. The second part shows you have to store data on hdfs and load it into hive, and the third part illustrates how you can use dplyr commands with sparklyr.

## Setting up Rstudio and the apache cluster

Whereas setting up and using an apache cluster was once an arcane process unsuited for the uninitiated, these days vendors such as [Amazon Web Services](https://aws.amazon.com/), [Microsoft Azure](https://azure.microsoft.com/nl-nl/) and [Google Cloud Computing](https://cloud.google.com/compute/) make the entire process a lot easier.

The first thing you should do is [sign up](https://cloud.google.com/free-trial/) for an account. Then, create a new project.

<https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/1.png">

Give your project a fancy name.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/2.png">
</div>

Go to the 'API' section in the console.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/3.png">
</div>

Enable the APIs with the red rectangle around them. If they are not in the list, use the ‘enable api’ button (marked in blue) to search for and activate them.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/4.png">
</div>

Go to the ‘networking’ pane under the settings menu.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/5.png">
</div>

Click on ‘firewall rules’ and then click on ‘add firewall rule’. We need to open up port 8787 (which rstudio server uses) in order to access rstudio from a browser.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/6.png">
</div>

Create the rule as shown in the picture and click ‘create’.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/7.png">
</div>

Go back to the menu, scroll all the way down, and select ‘Dataproc’

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/8.png">
</div>

Click on ‘create cluster’ and customize settings to your taste. *NB. If you're on a test plan you cannot exceed 8 cores.*

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/9.png">
</div>

Install the [Cloud SDK](https://cloud.google.com/sdk/) for your OS and follow the instructions to set it up properly.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/10.png">
</div>

Go back to the developers console and visit the ‘compute engine’ section.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/11.png">
</div>

Click on the arrow next to the master node ‘ssh’ option and select ‘view gcloud command’.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/12.png">
</div>

Copy-paste the command in a terminal, hit enter, and follow the instructions. (essentially, hit enter twice to make a new ssh key for your computer to login securely via ssh).

Congratulations, you are now logged in.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/13.png">
</div>

To access rstudio server, we will need a user/password combination. However, the current user ‘Jasper’, does not login using a password, but via ssh. The easiest thing to do is to add a new user. Execute

```shell
sudo adduser rstudio
```

and fill out a new password for the user. You can simply press enter when prompted to fill out a name, office etc.

Next, go to the [rstudio website](https://dailies.rstudio.com/rstudioserver/ubuntu/amd64/) and copy the link to the latest build.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/14.png">
</div>

Go back to the terminal and execute ‘wget `rstudio-server-link`’, as in the picture below.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/15.png">
</div>

In the terminal, execute

```shell
sudo apt-get install gdebi-core
```

Execute ‘ls -l’, and copy the name of your studio server distribution. Then execute 'sudo gdebi `your-rstudio-distribution`' in the terminal. When prompted if you want to install the package, type ‘y’ and hit enter.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/16.png">
</div>

If all goes well you should see the following:

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/17.png">
</div>

Let’s install some dependencies for the R packages we’ll be installing later. Copy-paste the following into the terminal and hit enter

```shell
sudo apt-get install -y libcurl4-openssl-dev libxml2-dev libssl-dev
```

Go back to the google developers console, and copy the public IP of your master node:

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/18.png">
</div>

Open up a new tab in your web browser and visit 'http://`your-public-ip`:8787'. You should see the following page:

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/19.png">
</div>

Sign in using the credentials you created for user 'rstudio'. Congrats! You now have a fully functioning stack rstudio plus spark cluster running. Let’s have some fun with it.

## Loading data using hive

I've set up a repository with scripts to download & analyze an old favorite of mine: [Phoenix event data](http://phoenixdata.org/). Go back to the terminal, switch to user 'rstudio' and clone [this repository](https://github.com/JasperHG90/sparkly-blog.git) with my R scripts.

```shell
# Switch to user 'rstudio'
su rstudio

# Navigate to rstudio home directory
cd /home/rstudio/

# Clone url
git clone https://github.com/JasperHG90/sparkly-blog.git
```

Then, create a directory for the rstudio user in hdfs (hadoop's file system), and create a subdirectory for the phoenix data.

```shell
# Create a directory for rstudio user
hadoop fs -mkdir /user/rstudio

# Give all permissions
hadoop fs -chmod 777 /user/rstudio

# Make directory for phoenix data
hadoop fs -mkdir /user/rstudio/phoenix/
```

The git repository contains a packrat library with all necessary packages, so you will not need to install any packages manually. Go back to the rstudio window in your browser and use the file navigation system at the bottom right-hand corner to locate and open the 'spark-blog.Rproj'.

Once opened, the packrat library should kick in and start installing packages. If nothing happens, execute the following in the R Console.

```r
packrat::restore()
```

Now, open 'main.R' and execute the following lines of code to download the daily phoenix datasets. This takes approximately 20 minutes.

```r
# Download phoenix data if data folder does not exist OR if there are no files in the data folder.
source(paste0(getwd(), "/functions/downloadPhoenixData.R"))
if(!dir.exists(paste0(getwd(), "/data/")) | length(list.files(paste0(getwd(), "/data/"))) == 0) {
  cat("Downloading phoenix data files to /data/ folder.")
  downloadPhoenixData()
} else{
  cat("Events are already downloaded. Moving on ...")
}
```

Once downloaded, go back to the terminal and copy the downloaded data to hdfs.

```shell
# Copy phoenix data to hdfs
hadoop fs -put /home/rstudio/sparkly-blog/data/* /user/rstudio/phoenix/
```

We'll create a SQL-like table of all the individual data files using [apache hive](https://hive.apache.org/). Do to this, execute

```shell
hive
```

in a terminal, and copy-paste the following lines of code to create the hive table:

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS phoenix
(
EventID string,
DateStamp string,
Year int,
Month int,
Day int,
SourceActorFull string,
SourceActorEntity string,
SourceActorRole string,
SourceActorAttribute string,
TargetActorFull string,
TargetActorEntity string,
TargetActorRole string,
TargetActorAttribute string,
EventCode string,
EventRootCode string,
PentaClass string,
GoldsteinScore float,
Issues string,
Lat float,
Lon float,
LocationName string,
StateName string,
CountryCode string,
SentenceID string,
URLs string,
NewsSources string
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
LINES TERMINATED BY '\n';
```

Next, load the phoenix data sets into the table:

```sql
LOAD DATA INPATH '/user/rstudio/phoenix' INTO TABLE phoenix;
```

Great! So now we're all set to connect to the data via sparklyr.

## Analyzing Phoenix data using sparklyr

The first thing you should do is load the necessary packages and connect to spark.

```r
# Connect to spark
library(tidyverse)
library(sparklyr)
library(lubridate)
library(ggplot2)

# Settings for spark
Sys.setenv(SPARK_HOME="/usr/lib/spark")
config <- spark_config()
# Connect to spark on master node
sc <- spark_connect(master = "yarn-client", config = config, version = '1.6.2')
```

Next, we cache the phoenix dataset in memory to speed up queries.

```r
# Cache phoenix hive table into memory
tbl_cache(sc, 'phoenix')
```

As it turns out, the 'datestamp' variable in the phoenix dataset was not formatted as a real date. This is not a huge problem, as we can format it properly via R:

```r
# Add a proper date to the dataset
phoenix_tbl <- tbl(sc, "phoenix") %>%
  # Create a date from year/month/day combination
  mutate(datestamp = as.date(
    paste0(year, "-",
           ifelse(month < 10, paste0("0", month), month), "-",
           day)
  )) %>%
  # Filter for dates that are outside of scope (e.g. anything before june 2014)
  filter(datestamp >= "2014-06-22")
{% endhighlight %}

Let's execute some straightforward queries on the data. First, how many events do we have over the entirety of the dataset?

{% highlight R %}
# Sum by date and plot
dplot <- phoenix_tbl %>%
  # Group by date
  group_by(datestamp) %>%
  # Sum by date
  tally() %>%
  # Arrange in descending order
  arrange(datestamp) %>%
  # Collect to R
  collect() %>%
  # Plot
  ggplot(., aes(x=as.Date(ymd(datestamp)), y=n)) +
    geom_line() +
    theme_bw() +
    scale_x_date(name = "Date") +
    scale_y_continuous(name = "Number of Events") +
    geom_smooth()

# View
dplot
```

As we can see, the data fluctuates a lot between August 2014 and January 2015.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/20.png">
</div>

It seems that the period between February 1st, 2015 and June 1st, 2016 is the most reliable in terms of data availability. Let's filter for those dates.

```r
# Until feb 2015: frequently no events (0). This is annoying. Also a whole just before july no events.
# Filter data between feb 15 and 1st of june
phoenix_tbl <- phoenix_tbl %>%
  # Filter for dates
  filter(datestamp >= "2015-02-01" & datestamp <= "2016-06-01")
```

Next, we'd like to know the average [goldstein score](http://web.pdx.edu/~kinsella/jgscale.html) per day. The goldstein score is an (imperfect) measure of tone ranging from -10 to 10; the worse the event, the lower it is.

```r
# Calculate the average goldstein score for each day and plot
avg.goldstein <- phoenix_tbl %>%
  # Group by day
  group_by(datestamp) %>%
  # For each day, calculate the average goldstein score
  summarize(avg_goldstein = mean(goldsteinscore)) %>%
  # Arrange by date
  arrange(datestamp) %>%
  # Collect
  collect() %>%
  # Plot
  ggplot(., aes(x=as.Date(ymd(datestamp)), y=avg_goldstein)) +
    geom_line() +
    theme_bw() +
    scale_x_date(name = "Date") +
    scale_y_continuous(name = "Average goldstein score")

# Plot
avg.goldstein +
  geom_hline(yintercept = mean(avg.goldstein$data$avg_goldstein) + 2*sd(avg.goldstein$data$avg_goldstein), color="red") +
  geom_hline(yintercept = mean(avg.goldstein$data$avg_goldstein) - 2*sd(avg.goldstein$data$avg_goldstein), color="red")
```

On most days, the average goldstein score hovers between 0 and 1. January 2nd seems especially negative.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/21.png">
</div>

Let's look at events in four countries: the USA, China, Russia and Great Britain.

```r
# Show mentions events happening in USA, RUS, CHN and GBR
mentions <- phoenix_tbl %>%
  # Group by date and countrycode
  group_by(datestamp, countrycode) %>%
  # Tally
  tally() %>%
  # Arrange by date
  arrange(datestamp) %>%
  # Group by date
  group_by(datestamp) %>%
  # Normalize per day
  mutate(norm = n / sum(n)) %>%
  # Filter for countries
  filter(countrycode %in% c("USA", "RUS", "CHN", "GBR")) %>%
  # Collect
  collect() %>%
  # Plot
  ggplot(., aes(x=as.Date(ymd(datestamp)), y=norm, group=countrycode, color=countrycode)) +
    geom_line() +
    theme_bw() +
    scale_x_date(name="Date") +
    scale_y_continuous(name="Percentage of events",
                       limits=c(0,0.25))

# Show plot
mentions
```

Events that happen in the USA are reported on most. However, there is a significant drop in events reported after October 2015. At the same time, Russia, China and Great Britain seem to increase their share of all daily events. This could have a lot of causes, but given the sudden change I'd say that either the news sources on which the data set is based were changed, or someone tweaked the algorithm used to geolocate the events.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/22.png">
</div>

Finally, we can plot a map of all violent events in Syria using the `ggmap` package

```r
# Plot map of violent events in Syria.
# See codebook: https://s3.amazonaws.com/oeda/docs/phoenix_codebook.pdf

# Function to plot map
plotMap <- function(data, map) {
  map +
    geom_point(data=data, aes(x=lon, y=lat, size=n),
                col="red", alpha=0.4) +
    theme_bw()
}

# Get map of Syria
library(ggmap)
syr = as.numeric(geocode("Syria"))
syrmap = ggmap(get_googlemap(center=syr, scale=2, zoom=7), extent="normal")

# Get lat/lon of violent events in syria
syr.events <- phoenix_tbl %>%
  # Filter where countrycode == SYR & pentaclass == 4
  filter(countrycode == "SYR",
         pentaclass == 4) %>%
  # Group by lat / lon combination & count
  group_by(lat,lon) %>%
  tally() %>%
  arrange(desc(n)) %>%
  # Collect
  collect() %>%
  # Plot
  plotMap(., syrmap)

# Show
syr.events
```

Here, we observe that most events have no *specific* location; they are simply located at the center of Syria. Other than that, we also observe that many events occur at hotbeds of violence such as Aleppo, Kobane and around Damascus.

<div>
  <https://blog.jasperginn.nl/img src="https://blog.jasperginn.nl/img/blog/sparklyr-and-rstudio/23.png">
</div>
