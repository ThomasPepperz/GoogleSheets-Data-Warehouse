# GoogleSheets-Data-Warehouse

# Introduction to GoogleSheets-Data-Warehouse
The *Sender Agency Data Warehouse* is an interrelated set of R scripts designed to create data warehouses for clients in *Google Sheets*,  populate it with historical data, and then update each tab daily with new data. Data is sourced from a variety of third-party platforms such as Google Analytics (eCommerce, Google Ads, website user, and website session data), Klaviyo, and Facebook Marketing API for Facebook Ads data. Similar instructions found on the *Sender Agency Data Warehouse* Github repository can be reviewed in [Google Docs](https://docs.google.com/document/d/13dAlgocHdLjtOkFQajzoV66IwPPEc35Kks27QWPhWe4/edit).

# Data Sources Commonly-used in Digital Marketing

## JustUno

*Justuno: The AI Visitor Conversion Optimization Platform* (hereafter referred to as simply "Justuno') is a conversion marketing and analytics platform that provides retailers with tools to convert shoppers along with data insights to optimize marketing campaigns. Some of functionality Justuno features includes Email Pop Ups (e.g. exit intent pop ups), integration of captured leads with email services such as MailChimp, Bronto, Klaviyo, Hubspot, Constant Contact, and Rejoiner, banners and slide-ins that are customizable with HTML, cart and checkout abandonment offers, customizable single-use or bulk coupon codes (e.g. countdown timer, spin-to-win, age verification, holiday specific, mobile, social media), targeting and segmentation (configurable accordign to exit, page views, referral site, time on site, visit frequency, geo-location, device type, scroll, cart value, order history, local date and time, and previous engagement activity), A/B Testing, drag-and-drop design canvas, and more.

## Klaviyo

Klaviyo


## Shopify

Shopify

## Google Analytics

Google Analytics

## Google Ads

Google Ads

## Facebook Ads via the Facebook Marketing API

Facebook Ads

# DTC KPI Template
The DTC KPI Template is a series of sheets inside a Google Sheets workbook that resembles a traditional-looking spreadsheet KPI tracker complete with formulas and Excel-style formatting. The following image is an example of one such sheet (the "Email" sheet) from the KPI Tracker workbook.

![An example of what a sheet in the the DTC KPI Tracker Template workbook looks like](https://github.com/SENDERLLC/Sender-Agency-Data-Warehouse/blob/main/documentation/sender-agency-dtc-kpi-template-1.png)

The following sections describes how the *DTC KPI Template* is set up, comments on its design, instructs how to apply it to a new instance of a client data warehouse, and details the Google Sheets formula used throughout the template: `Summary KPIs`, `Paid Campaigns`, `Email`, and `Email Summary Charts`. 

## Summary KPIs

### GoogleSheets Formula for Creating Dynamic Week Ranges

The *DTC KPI Template* reports data aggregated by week for the most recent four weeks, including the current week. Some of the datasets have a column named `week` which is a number ranging from 1 to 52, representing the week of the year. For those datasets (e.g. "GA: Session Metrics"), there is no to extract the week of the year from the `date` column. But for some datasets (such as the Klaviyo datasets), in order to extract the week number from a date, formulas in the *DTC KPI Template* depend upon the `WEEKNUM()` function, which returns the week of the year for any given date. In order to pull data from the warehouse for the current week, a second function, the `TODAY()` function, is used to return the current day's date. In order to obtain the current year of the week, formulas wrap `=WEEKNUM()` around `TODAY()`: `=WEEKNUM(TODAY())`. 

In order to return the week-of-the-year number for last week, simply substract 1: `=WEEKNUM(TODAY())-1`. To go back two weeks, use `=WEEKNUM(TODAY())-2` and so on.

The following formula pulls all data from a sheet named 'KL: Klaviyo All Data' such that

1.) The `WEEKNUM` of the date column (`!A2:A` in this example) is equal to whatever the current day's `WEEKNUM` is (`=WEEKNUM(TODAY))`)
2.) The metric name (found in `!C2:C`) is equal to `"Subscribed to List"` (in this example). 

```
=SUMPRODUCT(WEEKNUM('KL: Klaviyo All Data'!A2:A)=WEEKNUM(TODAY()),'KL: Klaviyo All Data'!C2:C = "Subscribed to List", 'KL: Klaviyo All Data'!B2:B)
```

The previous formula takes the `SUMPRODUCT` of each day's data for a given week (in the previous example, it is always this week's data). The previous formula treats Monday as the first day of the week, but because there is a one-day reporting lag for each data warehouse, if a user were looking at the sheet on a Tuesday, the data returned by the previous formula would represent only one day's worth of data (Monday's). If a user were looking at the sheet on a Thursday, the data returned by the previous formula woudl represent the summed product of Monday, Tuesday, and Wednesday's data for the given metric. 

![An example how the warehouse uses the WEEKNUM formula dynamically](https://github.com/SENDERLLC/Sender-Agency-Data-Warehouse/blob/main/documentation/sender-agency-formula-explanation-1.png)

### GoogleSheets Formula for Tracking Percent Change from Week to Week

The *DTC KPI Tracker* also tracks the percent change of a week's data from the previous week's data. The formula used to track the percent change of a metric from one week to the next week is

```
=IFERROR((C7-E7)/E7, "NA")
```

where `C7` represents the current week's value for a metric and `E7` represents the previous week's value for that metric. Thus, the formula could be understood as `([new_week_value - old_week_value) / old_week_value`. To avoid division-by-zero errors (`#DIV/0!` in GoogleSheets and Excel), the function `IFERROR()` wraps around the percent change formula and returns "NA" in cases where the internal function returns an error. 

### GoogleSheets Conditional Formatting for Percent Change Columns

To apply or modify the conditional formatting to the percent change columns in the Google Sheets, simply click the cell that needs to be modified or have formatting applied and then click on the *Format* tab > *Conditional formatting* tab > *+ Add rule* button. To highlight a cell green when the percent change is positive, click the built-in rule "greater than" and select 0 as the value. Create a second rule and change the color the cell is to be highlighted to the red-equivalent. For the rule, select "less than or equal," and again for the value, select 0. The following image shows an example of what the cell ought to look like as well as the conditonal formatting rules being applied. 

![An example how the warehouse uses the WEEKNUM formula dynamically](https://github.com/SENDERLLC/Sender-Agency-Data-Warehouse/blob/main/documentation/conditional-formatting.png)

## Paid Campaigns


## Email


### FORMULA


#### KLAVIYO (ALL DATA): "Subscribed to List"
```
=SUMPRODUCT(WEEKNUM('KL: Klaviyo All Data'!A2:A)=WEEKNUM(TODAY()),'KL: Klaviyo All Data'!C2:C = "Subscribed to List", 'KL: Klaviyo All Data'!B2:B)
```

#### # KLAVIYO (ALL DATA): "Active on Site"
```
=SUMPRODUCT(WEEKNUM('KL: Klaviyo All Data'!A2:A)=WEEKNUM(TODAY()),'KL: Klaviyo All Data'!C2:C = "Active on Site", 'KL: Klaviyo All Data'!B2:B)
```

## Email Summary Charts

