---
nav_title: Engagement Reports
page_order: 22
local_redirect: 
  report-glossary: '/docs/user_guide/data_and_analytics/report_metrics/'
---
# Engagement Reports
With Engagement Reports, customers can now extract data across multiple Campaigns and/or Canvases.  Customers have the ability to:

1. Run a one time report or Schedule a report to be sent later.
2. Email the report Daily, Weekly, or Monthly.
3. Set a defined time range of data to include or do a rolling window with scheduled reports.
4. Choose a defined number of messages to be included in the report or automatically insert messages based on tags.
5. Select from over 20+ KPI Metrics.
6. Choose various delimiters and compression types for the report.


## How To Use

### Creating A New Report

Navigate to Data >> Engagement Reports. Note that your Braze user account must have ‘Export User Data’ access to utilize Engagement Reports. To create a new report, click ‘Create New Report’ located on the top right of the page.


![engagement_reports_menu][1]


#### Add Messages
The Add Messages tab allows you to select your messages in two ways:

![engagement_reports_message_selection][2]

Manually select Campaigns or Canvases - This option gives you the freedom to choose whichever Campaigns or Canvases you would like in this report.

Automatically select Campaigns or Canvases - This option gives you the ability to automatically include all messages based on a specific tag. You can target messages that have any one or all of the tags listed.  This option is useful if you are setting up recurring reports and utilize our tagging system.


#### Add Stats
The Stats tab will automatically show you stats for the types of Campaigns or Canvases you have selected.  For instance, if you picked Email messages, you will only see the Email Stats.  If you picked a combination of Email and Push, you will see the stats for those two channels.

| channel| stats available|
| ------| --------------|
| Email | Sent, Opens, Unique Opens, Clicks, Unique Clicks, Unsubscribes, Bounces, Delivered, Reported Spam |
| Push  | Sent, Opens, Influenced Opens, Bounces, Body Clicks |
| Web Push | Sent, Opens, Bounces, Body Clicks |
| In App Message | Impressions, Clicks, First Button Clicks, Second Button Clicks |
| Webhook  |  Sent, Errors |

![engagement_report_add_stats][3]


#### Set Up Report
The Set Up Report tab allows you to enter your report name, select the compression and delimiter of the report and include whom you would like to send this report to.  Additional functionality now includes "Data Coverage" and "Schedule Report".

_Data Coverage_

Time Frame:
By default the data range shown will go from the earliest message selected till present date.  You can customize this by selecting the date dropdown and using the custom range selection OR by selecting the next radio button and defining your date range with the dropdown options available.

Data Display:
By default the data displayed in the engagement reports is daily (1 day). Should you like to view this data across different intervals, you can choose an explicit number of days or weeks to aggregate the data for the report. So instead of seeing daily metrics, you can look at your engagement by week, month, quarter, etc. Should a time-centric aggregation not suffice, you can also elect to export data at the Campaign or Canvas level.

![engagement_reports_data_coverage][4]


_Schedule Report_

There are two options when scheduling your report.

- Send Immediately: After the report is saved, Braze will send this report Immediately

- Send at a Designated Time:  This option gives you flexibility to choose how frequently you would like to receive this report.  You can choose to send this report every X days, weeks or months.  Additionally you can define when to stop sending the report.

![engagement_reports_schedule_report][5]


### About the Report

- The report is exported as a link embedded inside of a triggered email.
- Regardless of the number of Campaigns or Canvases selected, only a max of 2 CSV files will be generated -  one for all of the Campaign data, and one for all of the Canvas data.
- The report, when opened, will contain all the statistics selected in the ‘Add Stats’ section of the setup process.
- Certain data is aggregated at the ‘Campaign’ or 'Canvas' level versus at the individual 'variant' or 'step' level.

{% alert tip %}
Reports are not saved, and re-running the report can result in updated statistics.
{% endalert %}

[1]: {% image_buster /assets/img_archive/engagement_report_menuoption.png %}
[2]: {% image_buster /assets/img_archive/engagement_report_add_messages.png %}
[3]: {% image_buster /assets/img_archive/engagement_report_add_stats.png %}
[4]: {% image_buster /assets/img_archive/engagement_report_datacoverage.png %}
[5]: {% image_buster /assets/img_archive/engagement_report_reportschedule.png %}
[6]: {{ site.baseurl }}/developer_guide/rest_api/export/#canvas-data-export
[7]: {{ site.baseurl }}/developer_guide/rest_api/export/#canvas-details-endpoint
[8]: {{ site.baseurl }}/developer_guide/rest_api/export/#canvas-data-series-endpoint
[9]: {{ site.baseurl }}/developer_guide/rest_api/export/#canvas-data-summary-endpoint
[10]: {{ site.baseurl }}/user_guide/data_and_analytics/tracking/influenced_opens/#influenced-opens
