---
layout: single
title:  "Java snippet for generating scheduled AppStore sales reports"
excerpt: "This is an old post from Blogger, now migrated here, for generating AppStore purchases."
date:   2013-04-18 11:00:00
classes: wide
tags:
  - java
  - programming
  - AppStore
  - sales
---

This is an old post migrated from [Blogger](http://thisshouldbethetitle.blogspot.com/2013/04/java-snippet-for-generating-scheduled.html). Some parts may be deprecated.    
{: .notice}

Hi developers! I've recently published my first iOS app on the AppStore and I thought that I should contribute as I did receive invaluable info from other developers too. 

I decided that a good point to start with is a Java script for scheduling downloads of our daily, weekly, monthly and yearly sales reports. You may find it useful to save some precious time. Hands on it. 

The first thing you need before programming anything is a tiny file named `Autoingestion.class`, a Java class provided by Apple to facilitate login and download sales reports. Go into the iTunes connect portal, click on *Sales and Trends* and then *Download User Guide*. You'll get downloaded a detailed guide *AppleStoreReportingInstructions.pdf* to use this java class and, more importantly, a link to download it. Although a thorough reading is recommended to adapt the following snippet, at least you'll be able to find 3 important values to make this work, apart from your **AppleID** and your **password**, your **vendorID**. 

Have it all? Fine! Let's code. 

Hereafter all the code can be found at [GenAppStoreSales](https://github.com/valillon/GenAppStoreSales).
{: .notice--primary}

The next method `autoingestionDownload()` will request your reports according to a given date. Some variables could be defined globally for efficiency but the script will be so light that efficiency is not a matter to be worried for now. 

```java
private static void autoingestionDownload(String reportName, String dateType, String dateCode) throws IOException
{   
    // Ingested String variables:
    // 'reportName':the current requested report (later defined)
    // 'dateType': scheduling period, i.e. daily, weekly, monthly or yearly.
    // 'dateCode': YYYYMMDD format (Year, Month & Day)

    //~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    // Your own iTunes account data (check The Guide to get your IDs) 
    String autoingestionPreArgs = " yourAppleID yourPassword yourVendorID Sales ";
    //~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    // 'sourcePath' globally stores the path to your already downloaded reports (later defined)
    File reportFile = new File(sourcePath, reportName); 
    Runtime rt = Runtime.getRuntime();
 
    if (!reportFile.isFile()) {  // if (the report has NOT been downloaded yet) then

        String s;
        System.out.println(reportName + " requesting...");

        // Runs Autoingestion with the requested input arguments
        Process downloadReport = rt.exec("java Autoingestion" + autoingestionPreArgs + dateType + " " + "Summary " + dateCode);

        // Read buffer allocation
        BufferedReader stdInput = new BufferedReader(new InputStreamReader(downloadReport.getInputStream()));
        BufferedReader stdError = new BufferedReader(new InputStreamReader(downloadReport.getErrorStream()));

        // Read the output from the command, line by line
        while ((s = stdInput.readLine()) != null)
            System.out.println(s);

        // Read any errors from the attempted command
        while ((s = stdError.readLine()) != null)
            System.out.println(s);

        // Reports are downloaded as compressed file. Let's unzip it
        rt.exec("gzip -d " + currentPath + "/" + reportName);

        // Move it to a storage folder
        rt.exec("mv " + reportName + " " + sourcePath + "/" + reportName);

        // I experienced that if the requested report is not ready yet, 
        // the Autoingestion.class could deliver the previous report file, so let's remove it anyway
        // You better don't keep any important *.gz file here ;)
        rt.exec("rm " + currentPath + "/*.gz");
      
    }
    // else System.out.println(reportName + " verified");
}
```


Well, this was the core routine which will be called by every report request at the main java function. Before going further, let's define a few global variables and two helpful methods: `updateRollCalendar()` will update the calendar variables and `printDatePeriod()` will prompt the download progression. 

```java
public static String currentPath;    // path where our .class is executed (pwd)
public static String sourcePath;     // SUBpath where our reports are stored

public static Calendar rollCalendar; // a calendar instance to roll days
 
// i guess these don't need to be described
public static int pursuedDay;
public static int pursuedMonth;
public static int pursuedYear;
public static int currentDay;
public static int currentMonth;
public static int currentYear;

private static void updateRollCalendar()
{
    pursuedDay = rollCalendar.get(Calendar.DATE);
    pursuedMonth = rollCalendar.get(Calendar.MONTH)+1; // MONTH runs [0..11] !!
    pursuedYear = rollCalendar.get(Calendar.YEAR);
}
private static void printDatePeriod(String periodType, String documentType)
{
    rollCalendar.add(Calendar.DATE, 1);  // adds 1day, best solution i could :P
    updateRollCalendar();
    System.out.print(periodType + " "+documentType+" verified from " + pursuedDay +"."+pursuedMonth+"."+pursuedYear);
    System.out.println(" to " + (currentDay-1) +"."+currentMonth+"."+currentYear);
    rollCalendar.add(Calendar.DATE, -1);  // subtracts 1day, best solution i could :P
    updateRollCalendar();
} 
```


Fine. Let's have a look to the `main` function. The following snippet is headed by the calendar and paths definition and then 4 similar loops for scheduling daily, weekly, monthly and yearly reports. Note that every report type has its own expiring date and no reports exist beyond your launching date. Obviously these 4 routine-loops could be grouped in a single method, but I decided to leave it as it is for the moment. On the contrary, differences between them could be more noticeable with this repetitive structure. To sum up, this code just requests those non-expired reports that have NOT been downloaded yet and NOT saved at the `sourcePath`. 

```java
public static void main(String[] args) throws IOException {

     System.out.print("\nRefreshing Apple Store Reports...");

    /* Init Calendars */
    rollCalendar = Calendar.getInstance();
    updateRollCalendar();
    currentYear = rollCalendar.get(Calendar.YEAR);
    currentMonth = rollCalendar.get(Calendar.MONTH)+1;
    currentDay = rollCalendar.get(Calendar.DATE);
    Calendar launchCalendar = Calendar.getInstance();
    //~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    // Put your own launching date here
    int launchYear = 2013, launchMonth = 2, launchDay = 28; //January=0; December=11 !!
    //~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    launchCalendar.set(launchYear, launchMonth, launchDay);

    /* Report storage folder */
    currentPath = new File("").getAbsolutePath();
    String sourceName = "/sources";
    File sourceDir = new File(currentPath, sourceName);
    if (!sourceDir.isDirectory()) {                    // Creates folder if needed
        if (!(new File(currentPath + sourceName)).mkdirs()) {
            System.out.println("[Error] Couldn't create 'source' folder.");
        }
    }
    sourcePath = sourceDir.getAbsolutePath();

    String dateCode, reportName;

    /* DAILY REPORT */
    System.out.println("\n-> Daily reports");
    for (int d = 0; d < 14; d++)   // daily reports are only available 14 days
    {
        rollCalendar.add(Calendar.DATE, -1);
        if (rollCalendar.compareTo(launchCalendar) <= 0)
            break;
        updateRollCalendar();
        dateCode = String.format("%d%02d%02d", pursuedYear, pursuedMonth, pursuedDay);
        reportName = "S_D_yourVendorID_" + dateCode + ".txt";
        autoingestionDownload(reportName, "Daily", dateCode);
    }
    printDatePeriod("DAILY","report");

    /* WEEKLY REPORT */
    System.out.println("\n-> Weekly reports");
    rollCalendar = Calendar.getInstance();
    rollCalendar.set(Calendar.DAY_OF_WEEK, Calendar.SUNDAY);
    pursuedDay = currentDay; pursuedMonth = currentMonth; pursuedYear = currentYear;
    for (int w = 0; w < 13; w++) // weekly reports are only available 13 weeks
    { 
        rollCalendar.add(Calendar.DATE, -7);
        if (rollCalendar.compareTo(launchCalendar) <= 0)
            break;
        updateRollCalendar();
        dateCode = String.format("%d%02d%02d", pursuedYear, pursuedMonth, pursuedDay);
        reportName = "S_W_yourVendorID_" + dateCode + ".txt";
        autoingestionDownload(reportName, "Weekly", dateCode);
    }
    printDatePeriod("WEEKLY","report");

    /* MONTHLY REPORTS */
    System.out.println("\n-> Monthly reports");
    rollCalendar = Calendar.getInstance();
    pursuedDay = currentDay; pursuedMonth = currentMonth-1; pursuedYear = currentYear;
    for (int m = 0; m < 12; m++) // monthly reports are only available 12 months
    { 
        rollCalendar.add(Calendar.MONTH, -1);
        rollCalendar.set(Calendar.DATE, rollCalendar.getActualMaximum(Calendar.DAY_OF_MONTH));
        if (rollCalendar.compareTo(launchCalendar) <= 0)
            break;
        updateRollCalendar();
        dateCode = String.format("%d%02d", pursuedYear, pursuedMonth);
        reportName = "S_M_yourVendorID_" + dateCode + ".txt";
        autoingestionDownload(reportName, "Monthly", dateCode);
    }
    printDatePeriod("MONTHLY","report");

    /* YEARLY REPORTS */
    System.out.println("\n-> Yearly reports");
    rollCalendar = Calendar.getInstance();
    rollCalendar.add(Calendar.DATE,-1);
    pursuedDay = currentDay-1; pursuedMonth = currentMonth; pursuedYear = currentYear;
    for (int y = 0; y < 100; y++) // yearly reports are available always (put just a high value)
    { 
        rollCalendar.add(Calendar.YEAR, -1);
        if (rollCalendar.compareTo(launchCalendar) <= 0)
            break;
        updateRollCalendar();
        dateCode = String.format("%d", pursuedYear);
        reportName = "S_Y_yourVendorID_" + dateCode + ".txt";
        autoingestionDownload(reportName, "Yearly", dateCode);
    }
    printDatePeriod("YEARLY","report");

    System.exit(0);
}
```


We're done! Only two last important requirements:   

1. The `Autoingestion.class` and your let's name it `generateScheduledReports.class` must lay together in the same path.   
2. File's paths must not contain spaces. If so, try to `String.replace` space characters as your shell could understand the command line executions along this script.   

Once your `generateScheduledReports.class` has been successfully generated (some libraries must be imported bla bla bla...) run daily `$ java GenAppStoreSalesReports` with your favorite scheduled solution and please spend that saved time to leave your computer away, take sun and fresh air! ;D 

Happy coding : ) 

Coda1: if some of you smarty-pants wondered why the variable `sourcePath` is named like that and not something like `storagePath`, it is because that folder will be used as a source storage folder for generating charts later on, with *jfreechart* for instance. But that's another post. 

Coda2: you can also have a look at [valillon.art/tonoamusic](http://valillon.art/tonoamusic).

Unfortunately the *TOnOa Music* app has been discontinued. 
{: .notice--info}
