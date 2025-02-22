---
nav_title: User Import
page_order: 13
---
# User Import

There are two approaches for importing customer data into your Braze dashboard:

## REST API
Braze’s User/Track REST API endpoint can be used to record custom events, user attributes, and purchases for users. See Braze’s [User Track Endpoint documentation][12] for more information.

## CSV
Braze’s User Import feature allows users to upload and update user profiles via CSV files. This feature supports recording and updating user attributes such as first name and email, in addition to custom attributes such as shoe size.

# CSV Imports

When importing your customer data it is necessary to specify each customer’s unique identifier, also known as `external_id`. Before starting your CSV import it’s important to understand from your engineering team how users will be identified in Braze. Typically this would be a database ID used internally. This should align with how users will be identified by the Braze SDK on mobile and web, and ensures that each customer will have a single user profile within Braze across their devices. Read more about [Braze’s user profile lifecycle][13].

When you provide an `external_id`, Braze will update any existing user with the same `external_id` or create a new identified user with that `external_id` set if one is not found.

[Download a CSV Import Template here.][template]

## Constructing Your CSV

Braze has a number of data types in Braze. When importing or updating user profiles via CSV, you can create or update Standard User Attributes or Custom Attributes.

Standard User Attributes are reserved keys in Braze, for example: `first_name` or `email`.

Custom Attributes are custom to your business: for example a travel booking app may have a custom attribute called `last_destination_searched`.

When importing customer data, it is important that the Column Headers you use map exactly to the Standard User Attributes. Otherwise, Braze will __automatically__ create a Custom Attribute on that user’s profile.

Braze accepts user data in the standard CSV format from files __up to 100MB in size__.

[Download a CSV Import Template here.][template]

## Data Point Considerations

Each piece of customer data imported via CSV will overwrite the existing value on user profiles and will count as a data point. Blank values will not overwrite existing values on the user profile, and you do not need to include all existing user attributes in your CSV file.

## Standard User Data Column Headers

Note that all fields are case sensitive. If you import a user with a column header `first_name`, Braze will update that profile’s first name field. However, if you import a user with a column header, `First_name` Braze will create a Custom Attribute called `First_name`. This will impact your segmentation and personalization abilities in Braze.

| USER PROFILE FIELD | DATA TYPE | INFORMATION | Mandatory |
|---|---|---|---|
| `external_id` | String | A unique user identifier for your customer. | Yes |
| `first_name` | String | The first name of your users as they have indicated (e.g. `Jane`). | No |
| `last_name` | String | The last name of your users as they have indicated (e.g. `Doe`). | No |
| `email` | String | The email of your users as they have indicated (e.g. `jane.doe@braze.com`). | No |
| `country` | String | Country codes must be passed to Braze in the ISO-3166-1 alpha-2 standard (e.g. `GB`). | No |
| `dob` | String | Must be passed in the format “YYYY-MM-DD” (e.g. `1980-12-21`). This will import your user’s Date of Birth and enable you to target users whose birthday is “today”. | No |
| `gender` | String | “M”, “F”, “O” (other), “N” (not applicable), “P” (prefer not to say) or nil (unknown). | No |
| `home_city` | String | The home city of your users as they have indicated (e.g. London). | No |
| `language` | String | Language must be passed to Braze in the ISO-639-1 standard (e.g. `en`). | No |
| `phone` | String | A telephone number as indicated by your users (e.g. `541 754 3010`). | No |
| `email_subscribe` | String | Available values are `opted_in` (explicitly registered to receive email messages), `unsubscribed` (explicitly opted out of email messages), and `subscribed` (neither opted in nor out). | No |
| `marked_email_as_spam_at` | String | Date at which the user’s email was marked as spam. Must be passed in ISO 8601 format or in yyyy-MM-dd’T’HH:mm:ss.SSSZ format. | No |
| `push_subscribe` | String | Available values are `opted_in` (explicitly registered to receive push messages), `unsubscribed` (explicitly opted out of push messages), and `subscribed` (neither opted in nor out). | No |
| `time_zone` | String | Time zone must be passed to Braze in the same format as the IANA Time Zone Database (e.g. `America/New_York` or `Eastern Time (US & Canada)`).  | No |

## Importing Custom Data via CSV

Any headers which do not exactly match non Standard User Data will create a Custom Attribute within Braze.

These data types are accepted in User Import:
- Datetime (Must be stored in the ISO 8601  format)
- Boolean (TRUE/FALSE)
- Number (Integer or Float with no spaces or commas, floats must use ‘.’ as the decimal separator)
- String (no commas)
- Blank (Blank values will not overwrite existing values on the user profile, and you do not need to include all existing user attributes in your CSV file.)

{% alert important %}
Arrays, Push Tokens, and Custom Event data types are not supported in User Import.
Especially for Arrays, commas in your CSV file will be interpreted as a column separator; therefore, any commas in values will cause errors parsing the file.

For uploading these kinds of values, please use Braze’s [User/Track REST API][14].
{% endalert %}

## Importing a CSV

To import your CSV file, navigate to the User Import page under the Users section on the left hand toolbar. In the lower text box, “Recent Imports”, there will be a table which lists up to twenty of your most recent imports, their file names, number of lines in the file, number of lines successfully imported, total lines in each file, and the status of each import.

[Download a CSV Import Template here.][template]

The upper box, “Import CSV”, will contain importing directions and a button to begin your import. Press the “Select CSV File” button and select your file of interest, then press “Start Upload.” Braze will upload your file and check the column headers as well as the data types of each column.

![CSV Import][3]

Once the upload is complete, you will see a modal window with a table previewing the contents of your file. All the information in this table is based off of the values in top few rows of your CSV file. For column headers, default attributes will be written in normal text, while custom attributes will be italicized and have their type noted in parentheses. There will also be a short summary of your file at the top of the pop-up.

You can import more than one CSV at the same time. CSV imports will run concurrently, and as such the order of updates is not guaranteed to be serial. If you require CSV imports run in a serial fashion, you should wait until a CSV import has finished before uploading a second one.

If Braze notices something malformed in your file during the upload, errors will be shown above the summary. A file can be imported with errors, but once started, an import cannot be cancelled or rolled-back. Review the preview, and if errors are found cancel the import and modify your file. It is important to examine the full file before upload as well. Braze does not scan every row of the input file at this stage; errors can exist which Braze does not catch while generating this preview.

Malformed rows and rows lacking an external ID will not be imported, all other errors can be imported, but may interfere with filtering when creating a segment.

{% alert warning %}
Errors are based solely off of data type and file structure - for example, a poorly formatted email address - would still be imported as it can still be parsed as a string.
{% endalert %}

![CSV Import Errors][6]

When you are satisfied with the upload, start the import. The pop-up will close and the import will begin in the background. You can track its progress on the User Import page, which will refresh every 5 seconds, or at the press of the refresh button in the Recent Imports box.

Under `Lines Processed`, you will see the progress of the import; the status will change to Complete when finished. You can continue to navigate around other parts of Braze during the import, you will receive modal notifications when the import begins and ends.

When the import process runs into an error, a yellow warning icon will be displayed next to the total number of lines in the file. You can mouse over the icon to see details into why certain lines failed. Once the import is Complete, all data will have been added to existing profiles or all new profiles will have been created.

# Segmenting

User Import creates and updates user profiles, and can also be used to create segments. To create a segment, check the ‘Create a segment from this CSV’ box.

![CSV Import Segmenting Users][7]

You can set the name of the segment or accept the default, which is the name of your file. Files which were used to create a segment will have a link to view the segment once the import has completed.

The filter used to create the segment selects users who were created or updated in a selected import, and is available with all other filters in the edit segment page.

{% alert update %}
As of 4/10/2018, for each user, only the last 100 CSVs the user was imported/updated in are cached. So, if you attempt to create a segment by filtering for members who were in an older import, the segment will not include users who have been in 100 or more imports since. Previous to 4/10/2018, Braze cached the last 10 CSVs that a user was imported/updated in.
{% endalert %}

# Troubleshooting

## No External ID

To connect data from User Import to user profiles, Braze requires an `external_id` in each row. Rows without a value in the `external_id` column will be excluded from the import. User profiles that lack an `external_id` cannot be created or updated via the User Import.

## Malformed Row

There must be a header row in order to properly import data. Each row must have the same number of cells as the header row. Rows whose length that have more or fewer values than the header row will be excluded from the import. Commas in a value will be interpreted as a separator and can lead to this error being thrown. Additionally, all data must be UTF-8 encoded.

## Multiple Data Types

Braze expects each value in a column to be of the same data type. Values which do not match their attribute’s data type will cause errors in segmenting.

## Incorrectly Formatted Dates
Dates not in the ISO 8601 format will not be read as datetimes on import.

## String Quotation

Values encapsulated in single (‘’) or double (“”) quotation marks will be read as strings on import.

## Data Imported as Custom Attribute
If you are seeing a piece of Standard User Data (e.g. `email` or `first_name` imported as a Custom Attribute), check the case and spacing of your CSV file. `First_name` would be imported as a Custom Attribute, `first_name` would be correctly imported into the “first name” field on a user’s profile.


{% alert important %}
Braze will ban or block users ("dummy users") with over 5 million sessions and no longer ingest their SDK events, because they are usually the result of misintegration. If you find that this has happened for a legitimate user, please reach out to your Braze account manager.
{% endalert %}

[fields]: {{ site.baseurl }}/developer_guide/rest_api/user_data/#custom-attribute-data-types
[1]: {% image_buster /assets/img_archive/userimport_examplecsv_noerrors.png %}
[2]: {% image_buster /assets/img_archive/userimport_nav.png %}
[3]: {% image_buster /assets/img/importcsv.png %}
[4]: {% image_buster /assets/img_archive/userimport_Uploaded_NoErrors.png %}
[5]: {% image_buster /assets/img_archive/userimport_Uploaded_Errors.png %}
[6]: {% image_buster /assets/img/csv-errors.png %}
[7]: {% image_buster /assets/img/segment-imported-upsers.png %}
[8]: {% image_buster /assets/img_archive/userimport_view_segment.png %}
[9]: {% image_buster /assets/img_archive/userimport_edit_segment.png %}
[10]: {% image_buster /assets/img_archive/userimport-import_started.png %}
[11]: {% image_buster /assets/img_archive/userimport-import_finished.png %}
[12]: {{ site.baseurl }}/developer_guide/rest_api/user_data/#user-track-endpoint
[13]: {{ site.baseurl }}/user_guide/data_and_analytics/user_data_collection/user_profile_lifecycle/
[14]: {{ site.baseurl }}/developer_guide/rest_api/user_data/#user-track-endpoint
[errors]:#common-errors
[date]: https://en.wikipedia.org/wiki/ISO_8601
[utf8]: https://en.wikipedia.org/wiki/UTF-8
[CAO]: {{ site.baseurl }}/user_guide/data_and_analytics/custom_data/custom_attributes/
[filtering]: {{ site.baseurl }}/user_guide/engagement_tools/segments/creating_a_segment/#creating-a-segment
[template]: {% image_buster /assets/download_file/braze-user-import-template-csv.xlsx %}
