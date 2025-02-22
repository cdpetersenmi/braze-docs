---
nav_title: Making an API Call
platform: Message_Building_and_Personalization
subplatform: Personalization
page_order: 1
---

# Making an API Call

{% raw %}

Messages sent by Braze can retrieve content from a web server to be included in a message by using the `{% connected_content %}` tag. For example, the following message body will access the url `http://numbersapi.com/random/trivia` and include a fun trivia fact in your message:

```
Hi there, here is fun some trivia for you!: {% connected_content http://numbersapi.com/random/trivia %}
```

You can also include user profile attributes as variables in the URL string when making Connected Content requests. As an example, you may have a web service that returns content based on a user's email address and ID. If you're passing attributes containing special characters, such as @, make sure to use the Liquid filter `url_param_escape` to replace any characters not allowed in URLs with their URL-friendly escaped versions, as shown in the e-mail address attribute below.

```
Hi, here are some articles that you might find interesting:

{% connected_content http://www.yourwebsite.com/articles?email={{${email_address} | url_param_escape}}&user_id={{${user_id}}} %}
```

If the URL is unavailable, Braze will render an empty string in its place. Because Braze delivers messages at a very fast rate, be sure that your server can handle thousands of concurrent connections so we do not overload your server when pulling down content. When using public APIs, ensure your usage will not violate any rate-limiting that the API provider may employ. Braze requires that server response time is less than 2 seconds for performance reasons; if the server takes longer than 2 seconds to respond, the content will not be inserted.

If the endpoint returns JSON, you can detect that by checking if the `connected` value is null, and then [conditionally abort the message][1]. Braze only allows URLs that communicate over port 80 (HTTP) and 443 (HTTPS).
{% endraw %}

{% alert note %}
* Attribute values must be surrounded by `${}` in order to operate properly within Braze's version of Liquid Syntax.
* Connected Content calls will happen at the time the message is sent, with the exception of In-App Messages, which will make this call at the time the message is viewed.
* Connected Content calls do not follow redirects.
* Braze's systems may make the same Connected Content API call more than once per recipient. That is because Braze may need to make a Connected Content API call to render a message payload, and message payloads can be rendered multiple times per recipient for the purposes of validation, retry logic, or other internal purposes. Your systems should be able to tolerate the same Connected Content call being made more than one time per recipient.
{% endalert %}

{% raw %}
### Using Basic Authentication

If the URL requires basic authentication, Braze can generate a basic authentication credential for you to use in your API call. In the Connected Content tab in Manage App Group, you can manage existing basic authentication credentials and add new ones.

![Basic Authentication Credential Management][34]

To add a new credential, click Add Credential. You can then name your credential and put in the username and password.

![Basic Authentication Credential Creation][35]

You can then use this basic authentication credential in your API calls by referencing the token's name:

```
Hi there, here is fun some trivia for you!: {% connected_content https://yourwebsite.com/random/trivia :basic_auth credential_name %}
```

>  If you delete a credential, keep in mind that any Connected Content calls trying to use it will be aborted.

{% endraw %}

## Connected Content IP Whitelisting

When a message using Connected Content is sent from Braze, the Braze servers automatically make network requests to our customers’ or third parties’ servers to pull back data.  

With IP whitelisting, you can verify that Connected Content requests are actually coming from Braze, adding an additional layer of security.

Braze will send Connected Content requests from the IP ranges below. Braze has a reserved a set of IPs that are used for all services, not all of which are active at a given time.  This ensures that if Braze needs to send from a different data center, or do maintenance, Braze can do so without impact to customers. Braze may use one, a subset or all of the IPs listed below when making Connected Content requests. 

| For Instances `US-01`, `US-02`, `US-03`, `US-04`, `US-06` and `US-08`: |
|---|
| `23.21.118.191`
| `34.206.23.173`
| `50.16.249.9`
| `52.4.160.214`
| `54.87.8.34`
| `54.156.35.251`
| `52.54.89.238`
| `18.205.178.15`

| For Instance `EU-01`: |
|---|
| `52.58.142.242`
| `52.29.193.121`
| `35.158.29.228`



[1]: #aborting-connected-content
[6]: {% image_buster /assets/img_archive/Connected_Content_Syntax.png %} "Connected Content Syntax Usage Example"
[7]: http://openweathermap.org/api
[8]: http://developer.nytimes.com/docs/read/article_search_api_v2
[9]: http://open-platform.theguardian.com/documentation/
[10]: http://alchemyapi.readme.io/v1.0/docs/introduction
[11]: http://platform.seatgeek.com/
[12]: http://developer.tmsapi.com/docs/read/data_v1_1/movies/movie_showtimes
[13]: http://www.bandsintown.com/api/overview
[14]: http://www.last.fm/api
[15]: http://developer.ebay.com/devzone/shopping/docs/concepts/shoppingapiguide.html
[16]: [success@braze.com](mailto:success@braze.com)
[17]: {% image_buster /assets/img_archive/connected_weather_push2.png %} "Connected Content Push Usage Example"
[18]: http://numbersapi.com/
[19]: http://developer.eventbrite.com/
[20]: http://api.eventful.com/
[21]: http://www.discogs.com/developers/
[22]: http://www.songkick.com/developer
[23]: http://www.enclout.com/api/v1/yahoo_finance
[24]: http://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html
[25]: http://www.semantics3.com/products/pull
[26]: http://factual.com/products/cpg
[27]: http://blog.clearbit.com/logo
[28]: http://api.tfl.gov.uk/#Line
[29]: http://datamine.mta.info/
[30]: {{ site.baseurl }}/user_guide/personalization_and_dynamic_content/connected_content/about_connected_content/
[31]: https://docs.transifex.com/api/translation-strings
[32]: {% image_buster /assets/img_archive/TransifexUI.png %}
[33]: {% image_buster /assets/img_archive/terminal.png %}
[34]: {% image_buster /assets/img_archive/basic_auth_mgmt.png %}
[35]: {% image_buster /assets/img_archive/basic_auth_token.png %}
[36]: https://www.barchartondemand.com/free
[37]: https://www.coindesk.com/api/
[38]: http://developer.ticketmaster.com/products-and-docs/apis/getting-started/
[39]: https://sunrise-sunset.org/api
[40]: http://www.brewerydb.com/
[41]: https://developers.zomato.com/api
[42]: https://airvisual.com/api
[43]: https://developer.nutritionix.com/
[44]: https://open.fda.gov/api/
[45]: https://ndb.nal.usda.gov/ndb/doc/index
[46]: http://www.json.org
[47]: {{ site.baseurl }}/user_guide/engagement_tools/campaigns/testing_and_more/rate-limiting/#delivery-speed-rate-limiting
[48]: https://developer.accuweather.com/accuweather-locations-api/apis
[49]: https://developer.accuweather.com/accuweather-forecast-api/apis
[50]: https://developer.accuweather.com/accuweather-current-conditions-api/apis
[51]: https://developer.accuweather.com/accuweather-indices-api/apis
[52]: https://developer.accuweather.com/accuweather-weather-alarms-api/apis
[53]: https://developer.accuweather.com/accuweather-alerts-api/apis
[54]: https://developer.accuweather.com/accuweather-imagery-api/apis
[55]: https://developer.accuweather.com/accuweather-tropical-api/apis
[56]: https://developer.accuweather.com/accuweather-translations-api/apis
[57]: https://developer.accuweather.com
[58]: https://developer.accuweather.com/user/me/apps
[59]: https://developer.accuweather.com/weather-alarm-thresholds
[60]: {% image_buster /assets/img_archive/Accuweather_APIKey2.png %}
[61]: https://developer.accuweather.com/weather-icons
[62]: {{ site.baseurl }}/user_guide/personalization_and_dynamic_content/connected_content/about_connected_content/
